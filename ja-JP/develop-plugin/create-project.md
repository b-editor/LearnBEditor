## プラグイン用のC#プロジェクトを作成する。

### 開発環境
BEditorのプラグインを開発するには、
.NET5が使える環境が必要です。

例えば:
* Visual Studio
* Rider
* Visual Studio Code

などです。

このページではVisual Studio Codeでの開発方法を説明します。

### プロジェクトを作成する。

以下のコマンドをターミナルに入力します。  
プロジェクト名は一般的に大文字から始まるものが多いです。

``` bash
$ dotnet new classlib -o <プロジェクト名>

$ code ./<プロジェクト名>
```

生成された <プロジェクト名>.csprojファイルの設定をします。
以下をコピペ、(実行ファイルがあるフォルダ)と(プロジェクト名)は置き換えてください。

``` xml
<Project Sdk="Microsoft.NET.Sdk">

    <PropertyGroup>
        <TargetFramework>net5.0</TargetFramework>
        <OutputPath>(実行ファイルがあるフォルダ)\user\plugins\(プロジェクト名)</OutputPath>
        <AppendTargetFrameworkToOutputPath>false</AppendTargetFrameworkToOutputPath>
        <AppendRuntimeIdentifierToOutputPath>false</AppendRuntimeIdentifierToOutputPath>
    </PropertyGroup>

    <PropertyGroup Condition="'$(Configuration)'=='Debug'">
      <Optimize>false</Optimize>
    </PropertyGroup>

    <ItemGroup>
        <Reference Include="BEditor.Core">
            <HintPath>(実行ファイルがあるフォルダ)\BEditor.Core.dll</HintPath>
        </Reference>
        <Reference Include="BEditor.Graphics">
            <HintPath>(実行ファイルがあるフォルダ)\BEditor.Graphics.dll</HintPath>
        </Reference>
        <Reference Include="BEditor.Drawing">
            <HintPath>(実行ファイルがあるフォルダ)\BEditor.Drawing.dll</HintPath>
        </Reference>
        <Reference Include="BEditor.Media">
            <HintPath>(実行ファイルがあるフォルダ)\BEditor.Media.dll</HintPath>
        </Reference>
        <!-- 0.0.6以降の場合 -->
        <Reference Include="BEditor.Audio">
            <HintPath>(実行ファイルがあるフォルダ)\BEditor.Audio.dll</HintPath>
        </Reference>

        <!-- 入れると便利 -->
        <PackageReference Include="System.Reactive" Version="5.0.0" />
        <PackageReference Include="Microsoft.Extensions.DependencyInjection" Version="5.0.0" />
        <PackageReference Include="Microsoft.Extensions.Logging" Version="5.0.0" />

        <!-- エフェクトなどを追加する系のプラグインの場合入れると便利です -->
        <PackageReference Include="ReactiveProperty" Version="7.8.0" />
    </ItemGroup>

</Project>
```

### アセンブリのエントリーポイント

Class1.cs の namespace <プロジェクト名> 内を以下のようにします。

``` C#
public class Plugin
{
    public static void Register(string[] args)
    {
        // PluginクラスのRegisterメソッドが
        // アセンブリのエントリーポイントになります。

        // ここでプラグインクラスの登録やエフェクトの追加、サービスの登録などを行います。
    }
}
```

### プラグインの登録

実際にBEditorに表示されるプラグインを作成するには、  
以下のように書き換えます。

``` C#
using BEditor.Plugin;

public class SamplePlugin : PluginObject
{
    public SamplePlugin(PluginConfig config) : base(config)
    {

    }

    public override string PluginName => "サンプルプラグイン";
    public override string Description => "プラグインの説明";

    public override SettingRecord Settings { get; set; } = new SettingRecord();
}

public class Plugin
{
    public static void Register(string[] args)
    {
        PluginBuilder.Configure<SamplePlugin>()
            .Register();
    }
}
```

読み込まれているかを確認するには、

```
dotnet build
```

でビルドし、<実行ファイルがあるフォルダ>\user\plugins\<プロジェクト名>\<プロジェクト名>.dllが存在することを確認します。  
BEditorを起動しプラグインを読み込むかのダイアログが表示されたら、チェックボックスにチェックを入れて閉じます、  
設定を開き、読み込まれているプラグインに作成したプラグインがあれば正常に読み込まれています。