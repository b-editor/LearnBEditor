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

上のように変更してビルド、