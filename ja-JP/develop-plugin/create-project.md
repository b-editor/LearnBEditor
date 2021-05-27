## プラグイン用のC#プロジェクトを作成する。

### 開発環境
BEditorのプラグインを開発するには、
.NET5が使える環境が必要です。  
このページではVisual Studio Codeでの開発方法を説明します。  

### プロジェクトを作成する。

以下のコマンドをターミナルに入力します。  
プロジェクト名は一般的に大文字から始まるものが多いです。

``` bash
$ dotnet new classlib -o <プロジェクト名>

$ code ./<プロジェクト名>
```

生成された プロジェクトファイル (<プロジェクト名>.csproj)の設定をします。  
以下をコピペ、(実行ファイルがあるフォルダ)と(プロジェクト名)は置き換えてください。

``` xml
<Project Sdk="Microsoft.NET.Sdk">

    <PropertyGroup>
        <TargetFramework>net5.0</TargetFramework>
        <OutputPath>(実行ファイルがあるフォルダ)\user\plugins\(プロジェクト名)</OutputPath>
        <AppendTargetFrameworkToOutputPath>false</AppendTargetFrameworkToOutputPath>
        <AppendRuntimeIdentifierToOutputPath>false</AppendRuntimeIdentifierToOutputPath>
        <CopyLocalLockFileAssemblies>true</CopyLocalLockFileAssemblies>
    </PropertyGroup>

    <PropertyGroup Condition="'$(Configuration)'=='Debug'">
      <Optimize>false</Optimize>
    </PropertyGroup>

    <ItemGroup>
        <PackageReference Include="BEditor.Audio" Version="0.1.3" />
        <PackageReference Include="BEditor.Compute" Version="0.1.3" />
        <PackageReference Include="BEditor.Core" Version="0.1.3" />
        <PackageReference Include="BEditor.Drawing" Version="0.1.3" />
        <PackageReference Include="BEditor.Graphics" Version="0.1.3" />
        <PackageReference Include="BEditor.Media" Version="0.1.3" />
        <PackageReference Include="BEditor.Packaging" Version="0.1.3" />
        <PackageReference Include="BEditor.Settings" Version="0.1.3" />
    </ItemGroup>

</Project>
```

### パッケージソースの追加
`./packages` フォルダーを作成、BEditorのリリースから `PluginDevelop.zip` を展開し全ての `*.nuget` ファイルを入れます。  
`./Nuget.config` ファイルを作成して以下をコピペします。
``` xml
<?xml version="1.0" encoding="utf-8"?>
<configuration>
  <packageSources>
    <add key="BEditor Local" value=".\packages" />
  </packageSources>
</configuration>
```

### アセンブリのエントリーポイント

Class1.cs の namespace <プロジェクト名> 内を以下のようにします。

``` C#
public class Plugin
{
    public static void Register()
    {
        // PluginクラスのRegisterメソッドが
        // アセンブリのエントリーポイントになります。

        // ここでプラグインクラスの登録やエフェクトの追加、
        // サービスの登録などを行います。
    }
}
```

### プラグインの登録

実際にBEditorに表示されるプラグインを作成するには、  
以下のように書き換えます。

``` C#
using System;
using BEditor.Plugin;

public class SamplePlugin : PluginObject
{
    public SamplePlugin(PluginConfig config) : base(config)
    {
    }

    // プラグインの名前
    public override string PluginName => "サンプルプラグイン";

    // プラグインの説明
    public override string Description => "プラグインの説明";

    // プラグインを識別するのId
    // 開発環境では Zero のままで大丈夫ですが、
    // 公開する場合はGUID生成ツールなどで生成したIdを指定してください。
    public override Guid Id { get; } = Guid.Parse("00000000-0000-0000-0000-000000000000");

    // プラグインの設定、詳しくは https://github.com/b-editor/BEditor/blob/main/extensions/BEditor.Extensions.AviUtl/EntryPlugin.cs#L360 をご覧ください。
    public override SettingRecord Settings { get; set; } = new();
}

public class Plugin
{
    public static void Register()
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
プラグインを管理を開き、読み込まれているプラグインに作成したプラグインがあれば正常に読み込まれています。