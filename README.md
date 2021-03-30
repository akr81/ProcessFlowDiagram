# ProcessFlowDiagram tool
PlantUML + VSCodeでプロセスフロー図(PFD)を描くための
- PlantUMLプロシージャ
- VSCodeユーザスニペット

のセットです。

ユーザスニペットは、[Markdown Preview Enhanced](https://shd101wyy.github.io/markdown-preview-enhanced/#/) extensionとセットで使うことを想定しています。

## PlantUMLプロシージャ
`pfd_tool.pu`がプロシージャにあたります。

PFDを描くにあたって必要となる、`入力`と`プロセス`、`プロセス`と`出力`の接続を簡略化するためのプロシージャが定義されています。

|プロシージャ|使用例|説明|結果|
|---|---|---|---|
|$p(process, inputs, outputs)|$p("process", "in1,in2", "out")|事前に定義済みの要素を使ってプロセスを描画します。<br>```in1 --> process```<br>```in2 --> process```<br>```process --> out```<br>と同じです。事前に定義がない場合は、表示が異なる場合があります。|![p_sample](./image/2021-03-29-22-43-13.png)|
|$d(process, inputs, outputs)|$d("process", "in1,in2", "out")|プロシージャ内で要素を定義しながらプロセスを描画します。<br>```agent in1```<br>```agent in2```<br>```usecase process```<br>```in1 --> process```<br>```in2 --> process```<br>```process --> out```<br>と同じです。事前に定義がある場合は、エラーとなります。|![p_sample](./image/2021-03-29-22-43-13.png)|


## VSCodeユーザスニペット
`markdown.json`がスニペットにあたります。  
ファイル->設定->ユーザスニペットと開き、markdown.jsonに内容を追加してください。  
![](./image/2021-03-29-22-54-16.png) 

またその際、`PFD template`のパス(画像の選択部分)を適切な値に修正してください。  
![](./image/2021-03-29-22-56-12.png)

プロシージャの入力をサポートするスニペットが定義されています。  
スニペットの呼び出しは、markdownドキュメントにおいて`使用例`の文字列入力後に`Ctrl+Space`で行います。

|スニペット|使用例|説明|結果|
|---|---|---|---|
|PFD template|pfd|Markdown Preview EnhancedのPlantUMLコードブロックを、<br>`$d`プロシージャとともに出力します。|![](./image/2021-03-29-23-09-35.png)|
|draft process|d|`$d`プロシージャを出力します。|![](./image/2021-03-29-23-11-30.png)|
|draft process from clipboard|c|クリップボードの内容が`in`, `out`に入った`$d`プロシージャを出力します。|![](./image/2021-03-29-23-13-21.png)|
|reverse draft process from current line|r|カーソルがある行の`$d`プロシージャについて、<br>`in`と`out`を入れ替えた`$d`プロシージャを出力します。<br>行末で使用しないと正しく動作しません。|![](./image/2021-03-29-23-17-22.png)|
|multi line note|n|複数行ノートのテンプレートを出力します。|![](./image/2021-03-30-21-39-29.png)|


## 使用イメージ
1. `pfd`スニペットを使って、PFDを描き始めます。
1. 最初に出力された`$d`の`p`を最終成果物を生み出すプロセス、`out`を最終成果物に置き換えます。
1. `in`にプロセスに必要なものを`,`区切りで列挙します。
1. `r`, `c`スニペットを使ってプロセスで入出力をつなぎます。
1. 要素の色を変えるなど清書する場合、`$d`を`$p`に置き換え、要素の定義を追加します。

```puml
!include ./pfd_tool.pu

$d("食べる", "ラーメン,お箸", "満腹")
$d("カップ麺をつくる", "カップ麺,卵,熱湯", "ラーメン")
$d("お湯を沸かす", "水,電気ポット", "熱湯")
$d("卵を準備する", "冷蔵庫", "卵")

```
