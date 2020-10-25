+++
title = "輝度の計算"
date = "2013-07-01"
tags = ["Programming"]
+++

シェーダーでは、よくこんなコードを見かける。

```c
const mediump vec3 Perception = vec3(0.299, 0.587, 0.114);

void main(void)
{
    mediump vec3 color = texture2D(Sampler, TextureCoord).xyz;
    mediump float luminance = dot(Perception, color);
    gl_FragColor = (luminance > Threshold) ? vec4(color, 1) : vec4(0);
}
```

輝度がThresholdを上回れば、テクスチャの色を使い、そうでなければ黒にするということは分かる。それにしても、luminanceを取得するときにテクスチャの色と内積をとっているPerceptionとはなんだろうか。謎めいた値がハードコードされている。

どうも人間の光受容体の特性を考慮するための値らしい。

[Photoreceptor cell][]

人間の目は青にはあまり反応せず、緑には強く反応する。そのままRGBを平均した値を輝度に使ってしまうと、青が明るすぎ、緑が暗すぎる不自然な画像になってしまうというわけだ。

ちなみにこの係数はNTSC係数と呼ばれ、日本や北米でのテレビ用の規格らしい。  
一般的なsRGB色空間のモニタでは

`vec3(0.2126, 0.7152, 0.0722)`

こちらの係数を利用したほうが良いようだ。

 

 

  [Photoreceptor cell]: http://en.wikipedia.org/wiki/Photoreceptor_cell
    "Photoreceptor cell"
