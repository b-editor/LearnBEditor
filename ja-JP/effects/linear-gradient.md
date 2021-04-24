## 線形グラデーション

### Example

![](https://github.com/b-editor/BEditor/raw/main/docs/example/linear-gradient.jpg)

### Description

グラデーションを元の画像に合成します。

### Remarks

このエフェクトはSkia実装です。

### Properties

#### 開始位置 X (%)

* 最大 - 100
* 最小 - 0
* デフォルト - 0

#### 開始位置 Y (%)

* 最大 - 100
* 最小 - 0
* デフォルト - 0

#### 終了位置 X (%)

* 最大 - 100
* 最小 - 0
* デフォルト - 100

#### 終了位置 Y (%)

* 最大 - 100
* 最小 - 0
* デフォルト - 100

#### 色

* デフォルト - "#FFFF0000,#FF0000FF"

#### アンカー

* デフォルト - "0,1"

#### モード

* デフォルト - Repeat
* アイテム - "Clamp, Repeat, Mirror, Decal"

### Source codes

* [エフェクトの定義](https://github.com/b-editor/BEditor/blob/main/src/BEditor.Primitive/Effects/PrimitiveImages/LinearGradient.cs)