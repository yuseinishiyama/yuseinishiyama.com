+++
title = "Objective-Cで文字列定数"
date = "2013-06-02"
+++

Objective-Cで文字列定数の宣言をする方法について考えてみた。  
最も手っ取り早いのは当然マクロだろう。

```objc
#define CONST_STR @"Const Str"
```

だが、当然二重定義の問題があるので好ましいとは言えない。  
より好ましいのは以下の方法だろう。

```objc
static NSString * const kConstStr = @"Const Str";
```

constの位置に注意。  
文字列に対するポインタにconstを指定している。  

（ちなみにObective-Cではメンバ変数のプレフィックは「\_」が完全に推奨されている一方で、定数にこうしたハンガリアン記法を使うかどうか迷いどころだが、たいがいのフレームワークでは定数はこの形で書かれているし、実際しっくりくる。）

さてこの定数を外部に公開したいとする。  
ここでこんな風にやってしまうと間違いだ。

```objc
static NSString * const kConstStr = @"Const Str";
```

[http://stackoverflow.com/a/7642561][]  
ここにstaticに関する分かりやすい説明がある。

> static in Objective-C means a different thing than static in a C++
> class, in the context of static class data members and static class
> methods. In C and Objective-C, a static variable or function at global
> scope means that that symbol has internal linkage.
>
> Internal linkage means that that symbol is local to the current
> translation unit, which is the current source file (.c or .m) being
> compiled and all of the header files that it recursively includes.
> That symbol cannot be referenced from a different translation unit,
> and you can have other symbols with internal linkage in other
> translation units with the same name.
>
> So, if you have a header file declaring a variable as static, each
> source file that includes that header gets a separate global
> variable—all references to that variable within one source file will
> refer to the same variable, but references in different source files
> will refer to different variables.
>
> If you want to have a single global variable, you can't have it in
> class scope like in C++. One option is to create a global variable
> with external linkage: declare the variable with the extern keyword in
> a header file, and then in one source file, define it at global scope
> without the extern keyword. Internal linkage and external linkage are
> mutually exclusive—you cannot have a variable declared as both extern
> and static.
>
> An alternative, as Panos suggested, would be to use a class method
> instead of a variable. This keeps the functionality within the scope
> of the class, which makes more sense semantically, and you can also
> make it @private if you so desire. It does add a marginal performance
> penalty, but that's highly unlikely to be the bottleneck in your
> application (if you suspect it is, always profile first)

つまりCやObjective-Cのstaticは内部結合であり、ヘッダ上のstatic変数を異なるソースファイルから読み取ると、それぞれのファイル内で独立した変数として認識されてしまうというわけだ。  
試しに以下のような実験をしてみた。

```objc
#import <Foundation/Foundation.h>

@interface ClassA : NSObject

+ (NSInteger)staticVar;

+ (void)setStaticVar:(NSInteger)value;

+ (NSInteger)staticVarOfClassD;

+ (void)setStaticVarOfClassD:(NSInteger)value;

+ (NSInteger)externVar;

+ (void)setExternVar:(NSInteger)value;

+ (NSInteger)externVarOfClassD;

+ (void)setExternVarOfClassD:(NSInteger)value;

@end

static NSInteger staticVar = 0;

extern NSInteger externVar;
```

```objc
#import "ClassA.h"

@interface ClassD : NSObject

+ (NSInteger)staticVar;

+ (void)setStaticVar:(NSInteger)value;

@end

@implementation ClassD

+ (NSInteger)staticVar
{
    return staticVar;
}

+ (void)setStaticVar:(NSInteger)value
{
    staticVar = value;
}

+ (NSInteger)externVar
{
    return externVar;
}

+ (void)setExternVar:(NSInteger)value
{
    externVar = value;
}

@end

NSInteger externVar = 0;

@implementation ClassA

+ (NSInteger)staticVarOfClassD
{
    return [ClassD staticVar];
}

+ (void)setStaticVarOfClassD:(NSInteger)value
{
    [ClassD setStaticVar:value];
}

+ (NSInteger)externVarOfClassD
{
    return [ClassD externVar];
}

+ (void)setExternVarOfClassD:(NSInteger)value
{
    [ClassD setExternVar:value];
}

+ (NSInteger)staticVar
{
    return staticVar;
}

+ (void)setStaticVar:(NSInteger)value
{
    staticVar = value;
}

+ (NSInteger)externVar
{
    return externVar;
}

+ (void)setExternVar:(NSInteger)value
{
    externVar = value;
}

@end
```

```objc
#import <Foundation/Foundation.h>

@interface ClassB : NSObject

+ (NSInteger)staticVar;

+ (void)setStaticVar:(NSInteger)value;

+ (NSInteger)externVar;

+ (void)setExternVar:(NSInteger)value;

@end
```

```objc
#import "ClassB.h"

#import "ClassA.h"

@interface ClassB ()

@end

@implementation ClassB

+ (NSInteger)staticVar
{
    return staticVar;
}

+ (void)setStaticVar:(NSInteger)value
{
    staticVar = value;
}

+ (NSInteger)externVar
{
    return externVar;
}

+ (void)setExternVar:(NSInteger)value
{
    externVar = value;
}

@end
```

```objc
#import <Foundation/Foundation.h>

@interface ClassC : NSObject

+ (NSInteger)staticVar;

+ (void)setStaticVar:(NSInteger)value;

+ (NSInteger)externVar;

+ (void)setExternVar:(NSInteger)value;

@end
```

```objc
#import "ClassC.h"

#import "ClassA.h"

@implementation ClassC

+ (NSInteger)staticVar
{
    return staticVar;
}

+ (void)setStaticVar:(NSInteger)value
{
    staticVar = value;
}

+ (NSInteger)externVar
{
    return externVar;
}

+ (void)setExternVar:(NSInteger)value
{
    externVar = value;
}

@end
```

 

```objc
#import <Foundation/Foundation.h>

#import "ClassA.h"
#import "ClassB.h"
#import "ClassC.h"

static const NSString * const kConst = @"const";

int main(int argc, const char * argv[])
{
    @autoreleasepool {
        [ClassA setStaticVar:0];
        [ClassA setExternVar:0];
        NSLog(@"staticVar of classA is %ld", (long)[ClassA staticVar]);
        NSLog(@"staticVar of classB is %ld", (long)[ClassB staticVar]);
        NSLog(@"staticVar of classC is %ld", (long)[ClassC staticVar]);
        NSLog(@"staticVar of classD is %ld \n\n", (long)[ClassA staticVarOfClassD]);

        NSLog(@"extern of classA is %ld", (long)[ClassA externVar]);
        NSLog(@"extern of classB is %ld", (long)[ClassB externVar]);
        NSLog(@"extern of classC is %ld", (long)[ClassC externVar]);
        NSLog(@"extern of classD is %ld \n\n", (long)[ClassA externVarOfClassD]);

        [ClassB setStaticVar:1];
        [ClassB setExternVar:1];
        NSLog(@"staticVar of classA is %ld", (long)[ClassA staticVar]);
        NSLog(@"staticVar of classB is %ld", (long)[ClassB staticVar]);
        NSLog(@"staticVar of classC is %ld", (long)[ClassC staticVar]);
        NSLog(@"staticVar of classD is %ld \n\n", (long)[ClassA staticVarOfClassD]);

        NSLog(@"extern of classA is %ld", (long)[ClassA externVar]);
        NSLog(@"extern of classB is %ld", (long)[ClassB externVar]);
        NSLog(@"extern of classC is %ld", (long)[ClassC externVar]);
        NSLog(@"extern of classD is %ld \n\n", (long)[ClassA externVarOfClassD]);

        [ClassC setStaticVar:2];
        [ClassC setExternVar:2];
        NSLog(@"staticVar of classA is %ld", (long)[ClassA staticVar]);
        NSLog(@"staticVar of classB is %ld", (long)[ClassB staticVar]);
        NSLog(@"staticVar of classC is %ld", (long)[ClassC staticVar]);
        NSLog(@"staticVar of classD is %ld \n\n", (long)[ClassA staticVarOfClassD]);

        NSLog(@"extern of classA is %ld", (long)[ClassA externVar]);
        NSLog(@"extern of classB is %ld", (long)[ClassB externVar]);
        NSLog(@"extern of classC is %ld", (long)[ClassC externVar]);
        NSLog(@"extern of classD is %ld \n\n", (long)[ClassA externVarOfClassD]);

        [ClassA setStaticVarOfClassD:3];
        [ClassA setExternVarOfClassD:3];
        NSLog(@"staticVar of classA is %ld", (long)[ClassA staticVar]);
        NSLog(@"staticVar of classB is %ld", (long)[ClassB staticVar]);
        NSLog(@"staticVar of classC is %ld", (long)[ClassC staticVar]);
        NSLog(@"staticVar of classD is %ld \n\n", (long)[ClassA staticVarOfClassD]);

        NSLog(@"extern of classA is %ld", (long)[ClassA externVar]);
        NSLog(@"extern of classB is %ld", (long)[ClassB externVar]);
        NSLog(@"extern of classC is %ld", (long)[ClassC externVar]);
        NSLog(@"extern of classD is %ld \n\n", (long)[ClassA externVarOfClassD]);
     }
    return 0;
}
```

出力結果がこれ。

```objc
2013-06-02 21:21:04.953 StaticExternTest[1020:303] staticVar of classA is 0
2013-06-02 21:21:04.955 StaticExternTest[1020:303] staticVar of classB is 0
2013-06-02 21:21:04.955 StaticExternTest[1020:303] staticVar of classC is 0
2013-06-02 21:21:04.956 StaticExternTest[1020:303] staticVar of classD is 0 

2013-06-02 21:21:04.956 StaticExternTest[1020:303] extern of classA is 0
2013-06-02 21:21:04.957 StaticExternTest[1020:303] extern of classB is 0
2013-06-02 21:21:04.957 StaticExternTest[1020:303] extern of classC is 0
2013-06-02 21:21:04.957 StaticExternTest[1020:303] extern of classD is 0 

2013-06-02 21:21:04.958 StaticExternTest[1020:303] staticVar of classA is 0
2013-06-02 21:21:04.958 StaticExternTest[1020:303] staticVar of classB is 1
2013-06-02 21:21:04.959 StaticExternTest[1020:303] staticVar of classC is 0
2013-06-02 21:21:04.959 StaticExternTest[1020:303] staticVar of classD is 0 

2013-06-02 21:21:04.959 StaticExternTest[1020:303] extern of classA is 1
2013-06-02 21:21:04.960 StaticExternTest[1020:303] extern of classB is 1
2013-06-02 21:21:04.960 StaticExternTest[1020:303] extern of classC is 1
2013-06-02 21:21:04.960 StaticExternTest[1020:303] extern of classD is 1 

2013-06-02 21:21:04.961 StaticExternTest[1020:303] staticVar of classA is 0
2013-06-02 21:21:04.961 StaticExternTest[1020:303] staticVar of classB is 1
2013-06-02 21:21:04.961 StaticExternTest[1020:303] staticVar of classC is 2
2013-06-02 21:21:04.962 StaticExternTest[1020:303] staticVar of classD is 0 

2013-06-02 21:21:04.962 StaticExternTest[1020:303] extern of classA is 2
2013-06-02 21:21:04.962 StaticExternTest[1020:303] extern of classB is 2
2013-06-02 21:21:04.963 StaticExternTest[1020:303] extern of classC is 2
2013-06-02 21:21:04.963 StaticExternTest[1020:303] extern of classD is 2 

2013-06-02 21:21:04.963 StaticExternTest[1020:303] staticVar of classA is 3
2013-06-02 21:21:04.964 StaticExternTest[1020:303] staticVar of classB is 1
2013-06-02 21:21:04.964 StaticExternTest[1020:303] staticVar of classC is 2
2013-06-02 21:21:04.964 StaticExternTest[1020:303] staticVar of classD is 3 

2013-06-02 21:21:04.965 StaticExternTest[1020:303] extern of classA is 3
2013-06-02 21:21:04.965 StaticExternTest[1020:303] extern of classB is 3
2013-06-02 21:21:04.966 StaticExternTest[1020:303] extern of classC is 3
2013-06-02 21:21:04.966 StaticExternTest[1020:303] extern of classD is 3
```

実際にヘッダファイルから読み込まれたstatic変数は、それぞれ独立して変更されてしまっている。  

一方で、externは外部結合であり、期待通りの動作をする。externだと二重定義も防げる

外部のファイルからも参照したい定数を宣言する場合は、

```objc
extern NSString * const kConstStr;
```

```objc
NSString * const kConstStr = @"Const Str";
```

このようにするのが確実だろう。

このあたりの細かい挙動は忘れやすい上に、適当にやっても問題が顕在化しないことのほうが多いからこそ、ベストプラクティスを知っておきたいところだ。

 [http://stackoverflow.com/a/7642561]: http://stackoverflow.com/a/7642561
