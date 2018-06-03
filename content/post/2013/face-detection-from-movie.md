+++
title = "動画に対する顔検出"
date = "2013-12-24"
tags = ["Programming"]
+++

(この投稿はQiitaの[iOS Second Stage Advent Calendar
2013][]の25日目の記事です)

もはや非常に一般的となった、CIDetectorによる顔検出。  

ただ、よく見るサンプルはほとんどカメラの入力に対するリアルタイム検出ばかりだったので、

試しに動画に対して顔を検出するサンプルを作成してみた。

<https://github.com/yuseinishiyama/VideoFaceDetection>

CIDetectorとAVAssetReaderを組み合わせるだけで簡単にできる。

VFDVideoFaceDetector.h

    :::objective-c
	#import <Foundation/Foundation.h>
	
	typedef void(^VFDVideoReaderCompletionHandler)(NSArray *allFeatures);
	
	@interface VFDVideoFaceDetector : NSObject
	- (void)readFromURL:(NSURL *)targetURL
	complitionHandler:(VFDVideoReaderCompletionHandler)completionHandler;
	@end

VFDVideoFaceDetector.m

    :::objective-c
	#import "VFDVideoFaceDetector.h"
	#import <AVFoundation/AVFoundation.h>

	@interface VFDVideoFaceDetector ()
	@property (nonatomic, copy) VFDVideoReaderCompletionHandler completionHandler;
	@end

	@implementation VFDVideoFaceDetector {
	AVAsset *_asset;
	    CIDetector *_detector;
	    NSMutableArray *_allFeatures;
	}

	- (id)init
	{
	    if (self = [super init]) {
	        _allFeatures = [NSMutableArray arrayWithCapacity:0];
	    }
	    return self;
	}

	- (void)readFromURL:(NSURL *)targetURL
	  complitionHandler:(VFDVideoReaderCompletionHandler)completionHandler
	{
	    self.completionHandler = completionHandler;
	    [self _setupDetector];
	    __weak VFDVideoFaceDetector *weakSelf = self;
	    _asset = [AVAsset assetWithURL:targetURL];
	    [_asset loadValuesAsynchronouslyForKeys:@[@"tracks"] completionHandler:^{
	        [weakSelf _startReading];
	    }];
	}

	- (void)_setupDetector
	{
	    CIContext *context = [CIContext contextWithOptions:nil];
	    NSDictionary *detectorOptions = @{CIDetectorAccuracy : CIDetectorAccuracyHigh};
	    _detector = [CIDetector detectorOfType:CIDetectorTypeFace context:context options:detectorOptions];
	}

	- (void)_startReading
	{
	    AVAssetReader *reader = [AVAssetReader assetReaderWithAsset:_asset error:nil];
	    NSDictionary *outputSettings = @{(NSString *)kCVPixelBufferPixelFormatTypeKey:[NSNumber numberWithInt:kCVPixelFormatType_32BGRA]};
	    AVAssetReaderTrackOutput *output = [AVAssetReaderTrackOutput assetReaderTrackOutputWithTrack:[_asset tracksWithMediaType:AVMediaTypeVideo][0]
	                                                                                  outputSettings:outputSettings];
	    [reader addOutput:output];
	    [reader startReading];
	    while ([reader status] == AVAssetReaderStatusReading) {
	        [self _readNextVideoFrame:[output copyNextSampleBuffer]];
	    }
	    [self _endReading];
	}

	- (void)_endReading
	{
	    _completionHandler(_allFeatures);
	}

	- (void)_readNextVideoFrame:(CMSampleBufferRef)sampleBuffer
	{
	    if (!sampleBuffer) return;
	    CVPixelBufferRef pixelBuffer = CMSampleBufferGetImageBuffer(sampleBuffer);
	    CFDictionaryRef attachments = CMCopyDictionaryOfAttachments(kCFAllocatorDefault, sampleBuffer, kCMAttachmentMode_ShouldPropagate);
	    CIImage *convertedImage = [[CIImage alloc] initWithCVPixelBuffer:pixelBuffer options:(__bridge NSDictionary *)attachments];
	    if (attachments)
	       CFRelease(attachments);

	    enum {
	        PHOTOS_EXIF_0ROW_TOP_0COL_LEFT          = 1, //   1  =  0th row is at the top, and 0th column is on the left (THE DEFAULT).
	        PHOTOS_EXIF_0ROW_TOP_0COL_RIGHT         = 2, //   2  =  0th row is at the top, and 0th column is on the right.
	        PHOTOS_EXIF_0ROW_BOTTOM_0COL_RIGHT      = 3, //   3  =  0th row is at the bottom, and 0th column is on the right.
	        PHOTOS_EXIF_0ROW_BOTTOM_0COL_LEFT       = 4, //   4  =  0th row is at the bottom, and 0th column is on the left.
	        PHOTOS_EXIF_0ROW_LEFT_0COL_TOP          = 5, //   5  =  0th row is on the left, and 0th column is the top.
	        PHOTOS_EXIF_0ROW_RIGHT_0COL_TOP         = 6, //   6  =  0th row is on the right, and 0th column is the top.
	        PHOTOS_EXIF_0ROW_RIGHT_0COL_BOTTOM      = 7, //   7  =  0th row is on the right, and 0th column is the bottom.
	        PHOTOS_EXIF_0ROW_LEFT_0COL_BOTTOM       = 8  //   8  =  0th row is on the left, and 0th column is the bottom.
	    };

	    NSDictionary *options = @{CIDetectorImageOrientation : [NSNumber numberWithInt:PHOTOS_EXIF_0ROW_RIGHT_0COL_TOP]};
	    NSArray *features = [_detector featuresInImage:convertedImage options:options];
	    [features enumerateObjectsUsingBlock:^(CIFaceFeature *feature, NSUInteger idx, BOOL *stop) {
	        NSLog(@"%@", NSStringFromCGRect([feature bounds]));
	    }];
	    [_allFeatures addObject:features];
	    CFRelease(sampleBuffer);
	}

	@end

AVAssetReaderから取得できるCMSampleBufferをCIImageに変換してCIDetectorに渡す。  

とりあえず、Portraitの動画に対応するように、CIDetectorImageOrientationを設定している。  

試しに、自分の顔が真ん中に写った動画をインカメラで撮影したもの(3秒程度)を読み込んでみた結果が以下。

    :::objective-c
	2013-12-24 21:41:57.503 VideoFaceDetection[9455:3e07] {{627.5, 63.75}, {565, 565}}
	2013-12-24 21:41:57.771 VideoFaceDetection[9455:3e07] {{623.75, 67.5}, {567.5, 567.5}}
	2013-12-24 21:41:58.049 VideoFaceDetection[9455:3e07] {{630, 87.5}, {537.5, 537.5}}
	2013-12-24 21:41:58.329 VideoFaceDetection[9455:3e07] {{602.5, 76.25}, {572.5, 572.5}}
	2013-12-24 21:41:58.611 VideoFaceDetection[9455:3e07] {{567.5, 68.75}, {600, 600}}
	2013-12-24 21:41:58.883 VideoFaceDetection[9455:3e07] {{575, 93.75}, {570, 570}}
	2013-12-24 21:41:59.158 VideoFaceDetection[9455:3e07] {{542.5, 86.25}, {595, 595}}
	2013-12-24 21:41:59.443 VideoFaceDetection[9455:3e07] {{545, 125}, {552.5, 552.5}}
	2013-12-24 21:41:59.725 VideoFaceDetection[9455:3e07] {{532.5, 127.5}, {570, 570}}
	2013-12-24 21:42:00.013 VideoFaceDetection[9455:3e07] {{512.5, 125}, {587.5, 587.5}}
	2013-12-24 21:42:00.294 VideoFaceDetection[9455:3e07] {{508.75, 132.5}, {582.5, 582.5}}
	2013-12-24 21:42:00.583 VideoFaceDetection[9455:3e07] {{511.25, 130}, {575, 575}}
	2013-12-24 21:42:00.866 VideoFaceDetection[9455:3e07] {{510, 118.75}, {575, 575}}
	2013-12-24 21:42:01.149 VideoFaceDetection[9455:3e07] {{511.25, 112.5}, {567.5, 567.5}}
	2013-12-24 21:42:01.435 VideoFaceDetection[9455:3e07] {{511.25, 110}, {570, 570}}
	2013-12-24 21:42:01.725 VideoFaceDetection[9455:3e07] {{515, 112.5}, {562.5, 562.5}}
	2013-12-24 21:42:02.009 VideoFaceDetection[9455:3e07] {{515, 115}, {557.5, 557.5}}
	2013-12-24 21:42:02.293 VideoFaceDetection[9455:3e07] {{512.5, 118.75}, {567.5, 567.5}}
	2013-12-24 21:42:02.578 VideoFaceDetection[9455:3e07] {{507.5, 112.5}, {585, 585}}
	2013-12-24 21:42:02.861 VideoFaceDetection[9455:3e07] {{515, 98.75}, {585, 585}}
	2013-12-24 21:42:03.133 VideoFaceDetection[9455:3e07] {{532.5, 98.75}, {585, 585}}
	2013-12-24 21:42:03.400 VideoFaceDetection[9455:3e07] {{533.75, 70}, {615, 615}}
	2013-12-24 21:42:03.656 VideoFaceDetection[9455:3e07] {{566.25, 46.25}, {592.5, 592.5}}
	2013-12-24 21:42:03.911 VideoFaceDetection[9455:3e07] {{597.5, 26.25}, {580, 580}}
	2013-12-24 21:42:04.171 VideoFaceDetection[9455:3e07] {{621.25, 23.75}, {565, 565}}
	2013-12-24 21:42:04.431 VideoFaceDetection[9455:3e07] {{641.25, 32.5}, {547.5, 547.5}}
	2013-12-24 21:42:04.694 VideoFaceDetection[9455:3e07] {{653.75, 43.75}, {545, 545}}
	2013-12-24 21:42:04.959 VideoFaceDetection[9455:3e07] {{657.5, 56.25}, {545, 545}}
	2013-12-24 21:42:05.221 VideoFaceDetection[9455:3e07] {{653.75, 62.5}, {557.5, 557.5}}
	2013-12-24 21:42:05.484 VideoFaceDetection[9455:3e07] {{641.25, 66.25}, {582.5, 582.5}}
	2013-12-24 21:42:05.748 VideoFaceDetection[9455:3e07] {{666.25, 97.5}, {545, 545}}
	2013-12-24 21:42:06.017 VideoFaceDetection[9455:3e07] {{660, 101.25}, {555, 555}}
	2013-12-24 21:42:06.282 VideoFaceDetection[9455:3e07] {{650, 108.75}, {572.5, 572.5}}
	2013-12-24 21:42:06.543 VideoFaceDetection[9455:3e07] {{683.75, 95}, {552.5, 552.5}}
	2013-12-24 21:42:06.797 VideoFaceDetection[9455:3e07] {{691.25, 87.5}, {545, 545}}
	2013-12-24 21:42:07.062 VideoFaceDetection[9455:3e07] {{692.5, 68.75}, {540, 540}}
	2013-12-24 21:42:07.327 VideoFaceDetection[9455:3e07] {{675, 52.5}, {557.5, 557.5}}
	2013-12-24 21:42:07.592 VideoFaceDetection[9455:3e07] {{687.5, 57.5}, {540, 540}}
	2013-12-24 21:42:07.854 VideoFaceDetection[9455:3e07] {{700, 70}, {527.5, 527.5}}
	2013-12-24 21:42:08.144 VideoFaceDetection[9455:3e07] {{703.75, 87.5}, {517.5, 517.5}}
	2013-12-24 21:42:08.401 VideoFaceDetection[9455:3e07] {{701.25, 87.5}, {537.5, 537.5}}
	2013-12-24 21:42:08.662 VideoFaceDetection[9455:3e07] {{712.5, 93.75}, {527.5, 527.5}}
	2013-12-24 21:42:08.917 VideoFaceDetection[9455:3e07] {{708.75, 102.5}, {517.5, 517.5}}
	2013-12-24 21:42:09.184 VideoFaceDetection[9455:3e07] {{708.75, 115}, {510, 510}}
	2013-12-24 21:42:09.440 VideoFaceDetection[9455:3e07] {{695, 111.25}, {527.5, 527.5}}
	2013-12-24 21:42:09.677 VideoFaceDetection[9455:3e07] {{670, 107.5}, {552.5, 552.5}}
	2013-12-24 21:42:09.935 VideoFaceDetection[9455:3e07] {{653.75, 122.5}, {562.5, 562.5}}
	2013-12-24 21:42:10.198 VideoFaceDetection[9455:3e07] {{635, 146.25}, {560, 560}}
	2013-12-24 21:42:11.997 VideoFaceDetection[9455:3e07] {{591.25, 117.5}, {595, 595}}
	2013-12-24 21:42:12.268 VideoFaceDetection[9455:3e07] {{598.75, 111.25}, {580, 580}}
	2013-12-24 21:42:12.539 VideoFaceDetection[9455:3e07] {{598.75, 98.75}, {592.5, 592.5}}
	2013-12-24 21:42:12.805 VideoFaceDetection[9455:3e07] {{607.5, 106.25}, {577.5, 577.5}}
	2013-12-24 21:42:13.079 VideoFaceDetection[9455:3e07] {{596.25, 110}, {590, 590}}
	2013-12-24 21:42:13.354 VideoFaceDetection[9455:3e07] {{600, 105}, {595, 595}}
	2013-12-24 21:42:13.623 VideoFaceDetection[9455:3e07] {{607.5, 121.25}, {582.5, 582.5}}
	2013-12-24 21:42:13.896 VideoFaceDetection[9455:3e07] {{605, 121.25}, {587.5, 587.5}}
	2013-12-24 21:42:14.172 VideoFaceDetection[9455:3e07] {{610, 127.5}, {575, 575}}
	2013-12-24 21:42:14.438 VideoFaceDetection[9455:3e07] {{612.5, 127.5}, {582.5, 582.5}}
	2013-12-24 21:42:14.708 VideoFaceDetection[9455:3e07] {{606.25, 121.25}, {587.5, 587.5}}
	2013-12-24 21:42:14.977 VideoFaceDetection[9455:3e07] {{608.75, 117.5}, {592.5, 592.5}}
	2013-12-24 21:42:15.246 VideoFaceDetection[9455:3e07] {{608.75, 110}, {600, 600}}
	2013-12-24 21:42:15.512 VideoFaceDetection[9455:3e07] {{622.5, 113.75}, {575, 575}}
	2013-12-24 21:42:15.779 VideoFaceDetection[9455:3e07] {{615, 112.5}, {585, 585}}
	2013-12-24 21:42:16.050 VideoFaceDetection[9455:3e07] {{618.75, 118.75}, {580, 580}}
	2013-12-24 21:42:16.316 VideoFaceDetection[9455:3e07] {{615, 115}, {585, 585}}
	2013-12-24 21:42:16.584 VideoFaceDetection[9455:3e07] {{617.5, 115}, {585, 585}}
	2013-12-24 21:42:16.851 VideoFaceDetection[9455:3e07] {{620, 117.5}, {582.5, 582.5}}
	2013-12-24 21:42:17.119 VideoFaceDetection[9455:3e07] {{618.75, 117.5}, {580, 580}}
	2013-12-24 21:42:17.394 VideoFaceDetection[9455:3e07] {{621.25, 120}, {585, 585}}
	2013-12-24 21:42:17.666 VideoFaceDetection[9455:3e07] {{622.5, 126.25}, {580, 580}}

この座標を使って、動画にアニメーションをマージしたものを作成してみたかったのだが、今回はここまで。

  [iOS Second Stage Advent Calendar 2013]: http://qiita.com/advent-calendar/2013/ios-2
