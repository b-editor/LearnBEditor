## エフェクトを作成する

* [プラグイン用のC#プロジェクトを作成する。](https://beditor.net/docs/develop_plugin/create_project)を参照して、プラグインを作成します。

``` C#
using System.Collections.Generic;
using System.Runtime.Serialization;

using BEditor.Data;
using BEditor.Data.Primitive;
using BEditor.Drawing;
using BEditor.Drawing.Pixel;

[DataContract]
public class BlankEffect : ImageEffect
{
    public BlankEffect()
    {
        // コンストラクターです。
        // プロジェクトファイルを開いたときは呼び出されないので注意してください。
    }

    // エフェクトの名前です。
    // プロパティで表示されます。
    public override string Name => "空のエフェクト";

    // UIに表示するプロパティを返します。
    public override IEnumerable<PropertyElement> Properties => Array.Empty<PropertyElement>();

    // レンダリング時に呼び出されます。
    public override void Render(EffectRenderArgs<Image<BGRA32>> args)
    {
    }

    // プロジェクトを開いたとき、追加時、削除をやり直した時に呼び出されます。
    protected override void OnLoad()
    {
    }

    // プロジェクトを閉じた時、削除時、追加をやり直した時に呼び出されます。
    protected override void OnUnload()
    {
    }
}
```

上の例は空のエフェクトクラスです。
このエフェクトをBEditorに表示するにはエントリーポイントでプラグインにエフェクトを登録する必要があります。

``` C#
using BEditor.Data;
using BEditor.Plugin;

public class Plugin
{
    public static void Register(string[] args)
    {
        PluginBuilder.Configure<SamplePlugin>()
            .With(new EffectMetadata("空のエフェクト", () => new BlankEffect()))
            .Register();
    }
}
```

これをビルドして実行すると、ライブラリに "空のエフェクト" が追加されているはずです。

### プロパティを使う

上の例に少しコードを追加します。

``` C#
using System.Collections.Generic;
using System.Runtime.Serialization;

using BEditor.Data;
using BEditor.Data.Primitive;
+ using BEditor.Data.Property;
using BEditor.Drawing;
using BEditor.Drawing.Pixel;

[DataContract]
public class BlankEffect : ImageEffect
{
+   public static readonly CheckPropertyMetadata CheckboxMetadata = new("チェックボックス");

    public BlankEffect()
    {
+       Checkbox = new(CheckboxMetadata);
    }

    public override string Name => "空のエフェクト";

+-  public override IEnumerable<PropertyElement> Properties => new PropertyElement[] { Checkbox };

+   [DataMember]
+   public CheckProperty Checkbox { get; private set; }

    public override void Render(EffectRenderArgs<Image<BGRA32>> args)
    {
    }

    protected override void OnLoad()
    {
+       Checkbox.Load(CheckboxMetadata);
    }

    protected override void OnUnload()
    {
+       Checkbox.Unload();
    }
}

+ は追加、- は削除、+- は更新です。
```

上のように変更してビルド、クリップを追加して作成した "空のエフェクト" を追加します。  
以下のようになっていたら成功です。
![image]()

### プロパティの種類

* ButtonComponent - ボタンで特定のアクションを実行します。
* CheckProperty - チェックボックスでブーリアンを設定します。
* ColorAnimationProperty - 変化する色を設定します。
* ColorProperty - 色を設定します。
* DocumentProperty - 複数行の文字列を設定します。
* EaseProperty - 変化する値を設定します。
* Group - プロパティをまとめます。
    * ExpandGroup - プロパティをエクスパンダでまとめます。
        * Coordinate - XYZ, CenterXYZの座標を設定します。
        * Blend - 透明度、色、合成モードを設定します。
        * Angle - XYZ軸の角度を設定します。
        * Material - 環境色、反射色、輝きを設定します。
        * Zoom - XYZのスケールを設定します。
    * DialogProperty - プロパティをダイアログでまとめます。
* FileProperty - ファイルを設定します。
* FolderProperty - フォルダを設定します。
* FontProperty - フォントを設定します。
* LabelComponent - 文字列を表示します。
* SelectorProperty - リストからアイテムのインデックスを選択します。
* SelectorProperty{T} - リストからアイテムを選択します。
* TextProperty - 文字列を設定します。
* ValueProperty - 値を設定します。