## データバインディングについて

データバインディングを利用すると型が同じ場合、プロパティの値を別のプロパティと同期することが出来ます。

### 設定方法
以下のような場合。
```
ColorProperty <=> ColorProperty
p1 <=> p2
```

* オブジェクトビューアーから  __p1__ を右クリックし、パスをコピーをクリック。
* __p2__ のプロパティの __┇>バインド__ をクリック。
* パスをペーストしバインドします。

### 利用可能な例

* TextPropertyとDocumentProperty
* CheckPropertyとExpandGroup
* ValuePropertyとValueProperty
* TextPropertyとTextProperty
* ColorPropertyとColorProperty

(注) アニメーション系のプロパティのデータバインディングはサポートされません。
