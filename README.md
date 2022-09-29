# ProcessFlowDiagram tool
[English](./README_EN.md)

PlantUML + VSCodeでプロセスフロー図(PFD)を描くための
- PlantUMLプロシージャ
- VSCodeユーザスニペット

のセットです。

ユーザスニペットは、[Markdown Preview Enhanced](https://shd101wyy.github.io/markdown-preview-enhanced/#/) extensionとセットで使うことを想定しています。

## PlantUMLプロシージャ
`pfd_tool.pu`がプロシージャにあたります。

PFDを描くにあたって必要となる、`入力`と`プロセス`、`プロセス`と`出力`の接続を簡略化するためのプロシージャが定義されています。  
`入力`, `出力`は、`,`または` `(半角スペース)区切りで複数指定することができます。

|プロシージャ|使用例|説明|結果|
|---|---|---|---|
|$p(outputs, inputs, process)|$p("out", "in1,in2", "process")|事前に定義済みの要素を使ってプロセスを描画します。<br>```in1 --> process```<br>```in2 --> process```<br>```process --> out```<br>と同じです。事前に定義がない場合は、表示が異なる場合があります。|![p_sample](./image/2021-03-29-22-43-13.png)|
|$d(outputs, inputs, process)|$d("out", "in1 in2", "process")|プロシージャ内で要素を定義しながらプロセスを描画します。<br>```agent "in1" as in1```<br>```agent "in2" as in2```<br>```usecase "process" as process```<br>```in1 --> process```<br>```in2 --> process```<br>```process --> out```<br>と同じです。<br>`inputs, outputs`の要素を`c:`で始めた場合、その要素は雲形(`cloud`として定義)になります。<br>また、事前に定義がある場合は、エラーとなります。|![p_sample](./image/2021-03-29-22-43-13.png)|
|$c(color, inouts)|$c("#pink", "in1 in2")|それまでに定義された入出力に指定された色を付与します。<br>```agent "in1" as in1 #pink```<br>```agent "in2" as in2 #pink```<br>と同じです。<br>`$c`は`$d`による要素定義の後に使用するようにしてください。|![p_sample](./image/2021-06-03-19-52-32.png)|
|$cp(color, process)|$c("#skyblue", "process")|それまでに定義されたプロセスに指定された色を付与します。<br>```usecase "process" as process #skyblue```と同じです。<br>`$cp`は`$d`による要素定義の後に使用するようにしてください。|![p_sample](./image/2021-07-18-14-51-41.png)|

### 設定項目
デフォルトでは、入出力の区切り文字は`,`と` `に設定されています。  
英語環境をご利用の場合など` `区切りが不便な場合は、`pfd_tool.pu`の
```
!$space_delimiter = 1
```
を0に変更することで、`,`のみを区切り文字とすることができます。

## VSCodeユーザスニペット
`markdown.json`がスニペットにあたります。  
ファイル->設定->ユーザスニペットと開き、markdown.jsonに内容を追加してください。  
![](./image/2021-03-29-22-54-16.png) 

またその際、`PFD template`のパス(画像の選択部分)を適切な値に修正してください。  
![](./image/2021-03-29-22-56-12.png)

プロシージャの入力をサポートするスニペットが定義されています。  
スニペットの呼び出しは、markdownドキュメントにおいて`使用例`の文字列入力後に`Ctrl+Space`で行います(VSCodeの設定によります)。

|スニペット|使用例|説明|
|---|---|---|
|PFD template|pfd|Markdown Preview EnhancedのPlantUMLコードブロックを、<br>`$d`プロシージャとともに出力します。<br>また、上述の区切り文字設定も合わせて出力します。|
|draft process|d|`$d`プロシージャを出力します。|
|draft process from clipboard|c|クリップボードの内容が`in`, `out`に入った`$d`プロシージャを出力します。|
|define color for deliverables|c|`$c`プロシージャを出力します。|
|define color for process|cp|`$cp`プロシージャを出力します。|
|multi line note|n|複数行ノートのテンプレートを出力します。<br>リンク先は適切に設定してください。|
|multi line note to clipboard|nc|複数行ノートのテンプレートを出力します。<br>リンク先はクリップボードの要素になります。|


## 使用イメージ
1. `pfd`スニペットを使って、PFDを描き始めます。
1. 最初に出力された`$d`の`out`を最終成果物に置き換えます。
1. `in`にプロセスに必要なものを`,`(または` `)区切りで列挙します。
1. `p`を最終成果物を生み出すプロセス名にします。
1. `r`, `c`スニペットを使ってプロセスで入出力をつなぎます。
1. プロセスの入出力に色をつける場合、`$c`を使って設定します。
1. 清書する場合、`$d`を`$p`に置き換え、要素の定義を追加します。

```puml
!include ./pfd_tool.pu
!$space_delimiter = 1

$d("満腹", "ラーメン お箸", "食べる")
$d("ラーメン", "カップ麺 卵 熱湯", "ラーメンをつくる")
$d("カップ麺", "c:今日の気分", "カップ麺を選ぶ")
$d("熱湯", "水,電気ポット", "お湯を沸かす")

```

## 制限事項
### 記号の取り扱い
PlantUMLの要素で文字列に記号(`！`や`？`など)が含まれると、要素接続時にエラーとなる場合があります。  
これを回避するため、`$d`プロシージャではエイリアス設定時に一部の記号を削除しています。
```
agent "今日なに食べる？" as 今日なに食べる
```
したがって、記号のみが異なる文字列同士は同じエイリアスに置換されるため、意図しない動きとなる場合があります。

例：  
以下の場合、inとoutが同じになってしまいます。
```
$d("今日なに食べる？", "今日なに食べる！", "なに食べるか決める")
```
```puml
!include ./pfd_tool.pu

$d("今日なに食べる？", "今日なに食べる！", "なに食べるか決める")
```
