## プラグインをパッケージ化する

プラグインをパッケージ化するとプラグインを簡単にインストールすることができます。  
パッケージを作成するにはメインウィンドウの  
 __プラグイン>プラグインを管理>プラグインのパッケージを作成__ を開きます。

![screenshot](https://beditor.net/imgs/create-package-screenshot.jpg)

### 設定する項目
#### 情報
* 名前 - パッケージの名前
* 著者 - パッケージを作った人 (AssemblyCompanyAttributeと同じ値)
* ウェブサイト - ウェブサイトのURL
* 短い説明 - 次より短いな説明
* 説明 - 説明
* Id - PluginObject.Idと同じ値
* ライセンス - このパッケージのライセンス (AssemblyCopylightAttributeと同じ値)

#### バージョン
* 短い更新ノート - 次より短いな更新ノート
* 更新ノート - 更新ノート

#### 出力
* 出力先のディレクトリ - パッケージを出力するディレクトリ
* アセンブリファイル - プラグインのエントリーポイントを含むアセンブリファイル

パッケージを作成するには __出力>作成__ をクリックします。

### パッケージファイルについて
パッケージファイルは拡張子が __.bepkg__ のzipファイルです。  
なので簡単に中のファイルを編集することができます。  
  
[設定する項目](#設定する項目) で設定したパッケージファイルの情報は  
`PACKAGEINFO` ファイル中にJson形式で保存されています。  
次のJsonが `PACKAGEINFO` ファイルです。

``` Json
{
  "main_assembly": "最初に読み込まれるアセンブリファイル",
  "name": "パッケージの名前",
  "author": "著者",
  "homepage": "ホームページへのUrl",
  "description_short": "短い説明",
  "description": "説明 (複数行可)",
  "tag": "タグ (カンマで分ける)",
  "id": "PluginObject.Idと同じUUID",
  "license": "ライセンス",
  "versions": [
    {
      "version": "パッケージのバージョン (メジャー.マイナー.ビルド)",
      "download_url": "",
      "update_note": "更新ノート",
      "update_note_short": "短い更新ノート",
      "release_datetime": "公開した日時 (例[2021-05-30T00:00:00.0000000])"
    }
  ]
}
```

このJsonはパッケージソースで使用されるJsonの `packages` 内の要素と同じなので、
このJsonの `download_url` を指定して パッケージソースのJsonの `packages` に追加することができます。
