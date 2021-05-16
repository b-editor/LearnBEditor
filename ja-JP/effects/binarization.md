## ２階調化

### Example

![](https://beditor.net/imgs/example/binarization.jpg)

### Description

画像を２階調化します。

### Remarks

このエフェクトはGpu処理に対応しています。

### Properties

#### 閾値

* 最大 - 255
* 最小 - 0
* デフォルト - 127

### Source codes

* [エフェクトの定義](https://github.com/b-editor/BEditor/blob/main/src/BEditor.Primitive/Effects/PrimitiveImages/Binarization.cs)
* [エフェクトの処理](https://github.com/b-editor/BEditor/blob/main/src/BEditor.Drawing/PixelOperation/BinarizationOperation.cs)
