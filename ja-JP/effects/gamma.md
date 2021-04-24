## ガンマ調整

### Example

![](https://github.com/b-editor/BEditor/raw/main/docs/example/gamma.jpg)

### Description

画像のガンマを調整します。

### Remarks

このエフェクトはGpu処理に対応しています。

### Properties

#### ガンマ

* 最大 - 300
* 最小 - 1
* デフォルト - 100

### Source codes

* [エフェクトの定義](https://github.com/b-editor/BEditor/blob/main/src/BEditor.Primitive/Effects/PrimitiveImages/GammaCorrection.cs)
* [エフェクトの処理](https://github.com/b-editor/BEditor/blob/main/src/BEditor.Drawing/PixelOperation/GammaOperation.cs)
