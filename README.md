# ProcessFlowDiagram tool
[English](./README_EN.md)

PlantUML + VSCode(またはObsidian)でプロセスフロー図(PFD)を描くための
- PlantUMLプロシージャ
- VSCodeユーザスニペット
- Obsidianテンプレート

のセットです。

<!-- TOC -->

- [ProcessFlowDiagram tool](#processflowdiagram-tool)
  - [環境](#環境)
  - [使用方法](#使用方法)
    - [PFD](#pfd)
      - [include](#include)
      - [entities](#entities)
      - [p](#p)
    - [CRT](#crt)
      - [entities](#entities-1)
      - [connect\_result\_from\_causes](#connect_result_from_causes)
    - [プロシージャの設定](#プロシージャの設定)
      - [項目番号の表示/非表示](#項目番号の表示非表示)
      - [改行の自動挿入](#改行の自動挿入)
    - [設定項目](#設定項目)
  - [VSCodeユーザスニペット/Obsidianテンプレート](#vscodeユーザスニペットobsidianテンプレート)
  - [VSCodeユーザスニペット](#vscodeユーザスニペット)
  - [使用イメージ](#使用イメージ)
    - [注意点](#注意点)
  - [使用イメージ](#使用イメージ-1)
  - [制限事項](#制限事項)
    - [記号の取り扱い](#記号の取り扱い)

<!-- /TOC -->

<!-- /TOC -->
<!-- /TOC -->

## 環境
以下の環境での動作を想定しています(後述のプロシージャ自体はPlantUMLが利用できる環境であれば使用可能です)。

|                 | VSCode                                                                                     | Obsidian                                                 |
|-----------------|--------------------------------------------------------------------------------------------|----------------------------------------------------------|
| プラグイン           | [Markdown preview enhanced](https://github.com/shd101wyy/vscode-markdown-preview-enhanced) | [PlantUML](https://github.com/joethei/obsidian-plantuml) |
| plantuml.jar(*) | >=2022.09                                                                                  | ←                                                        |

(*) プラグインに同梱されているplantuml.jarが古い場合、プロシージャが利用している関数が実装されていないためエラーとなります。  
[PlantUMLのサイト](https://plantuml.com/ja/download)からplantuml.jarを入手し、プラグイン設定で指定するなどしてください。

## 使用方法

### PFD

PFDは`入出力(中間成果物)`(四角形)と`プロセス`(楕円)からなる図で、本プロシージャはその定義・接続の記述を簡略化するためのものです。

![サンプル](http://www.plantuml.com/plantuml/png/HS_1IWCn483XUvuYJNkei6p8hWSfA3s8Pn-W8Ddis0wRJ48oG_7jZNhm-C_73CoviQyiAQeTKeYoWhunbtQojlhxU4M-oI8DQiZ4G3o6dApVwfEpxpK7QEqjvlih-clroIQFPCld-i4vnx68Khi3ILgWmggVjLFgq6yW8xIZkIWaDrnKE25Dmq_D-uAq_OwoyBvjNqiKw6qU-cOv6gSd_Q9dCsZJV2eHN9S_c3iy_iFKuGoqzi--0G00)

上記サンプルを出力するための[サンプルコード](https://www.plantuml.com/plantuml/uml/HS-nQWCn383XtK_XEXrI26Tyrw5aA1bAvpv0e3W-DH5doyQI2NtxYdRe9AIFVz299NOP9Pi7f9J72lOcqlZeVO_twOfo6vV1q5Cb0P8frUBZrb-2_-Wr0VDxhlStZDSUYoyHoRUlzLDgpLCRnZngWuB0UtSsIE5mDkc8DEj4MdODQVizrI4xthulUO3kxadBjdV1plRLBkvW7SVICWILFrWKdlzXLZW1hVh93m00)をベースに説明します。

#### include

`pfd_tool.pu`に必要なプロシージャが定義されているため、まずはこのファイルをincludeする必要があります。

(1)ローカルにダウンロードしたライブラリファイル(`pfd_lib.zip`)からincludeする場合と、(2)ネットワーク経由でincludeする場合の2通りがあります。  
実行環境に応じた方を利用してください。

(1)の場合
```
!import PATH_TO_LIB/pfd_lib.zip
!include pfd_tool.pu
```

(2)の場合
```
!include https://raw.githubusercontent.com/akr81/ProcessFlowDiagram/main/pfd_tool.pu
```

#### entities

```
$entities("入出力1,入出力2,...")
```

PFDを構成する入出力((中間)成果物)を、","区切りで定義します。  
**定義した順**にエイリアスとして`連番`が付与され、サンプル画像のように標準では`連番`つきで表示されます。  
(非表示にしたい場合は、[項目番号の表示/非表示](#%E9%A0%85%E7%9B%AE%E7%95%AA%E5%8F%B7%E3%81%AE%E8%A1%A8%E7%A4%BA%E9%9D%9E%E8%A1%A8%E7%A4%BA)を参照してください)。

各成果物について、以下の装飾をすることができます。

- 先頭に`c:`を付与
  - 表示を雲形(cloud)とします。
- 末尾に`#COLOR`を付与
  - 表示色を`COLOR`にします。

[サンプルコード](https://www.plantuml.com/plantuml/uml/3SwnZeCm303GFLzn1pVSIOY8wr8nCLIT-mTL22vO97OKsqBz-pA-xKsYeQhbvBwHOh85lZRL8gFtDHpzYhhPR08rCYcGzf6p3tkz3lvHEOB8FV5nmx3Ma7qEIBwybgSofxwOSpz0YeeWlmtIqCKHwRz3khG5QJ_9fgtixpaPD7zk0bhEomS0)

![サンプル](https://www.plantuml.com/plantuml/png/3SwnZeCm303GFLzn1pVSIOY8wr8nCLIT-mTL22vO97OKsqBz-pA-xKsYeQhbvBwHOh85lZRL8gFtDHpzYhhPR08rCYcGzf6p3tkz3lvHEOB8FV5nmx3Ma7qEIBwybgSofxwOSpz0YeeWlmtIqCKHwRz3khG5QJ_9fgtixpaPD7zk0bhEomS0)


#### p

```
$p("入力1 入力2 > 出力", "プロセス名")
```

プロセスを定義し、定義済みの入出力とプロセスを接続します。  
入力・出力には`entities`で付与されたエイリアスを使用することができ、`>`の左側が入力、右側が出力として扱われます。  
こちらも**定義した順**にエイリアスとして`p連番`が付与されます。

プロセスについても、`$entities`と同様に表示色を設定することができます。

[サンプルコード](https://www.plantuml.com/plantuml/uml/JS-nJiCm4CRnFKzXt0v5YcAICA0Eg0DYvWtGrJdQa-spvJkhuktnWA3Zz_zDtqaKghOKXmTY7zk6vgfQvEXSXTjq8RssSnEiFhCYw-HpSX3go-m-QlOeyXxpxOtWKY6v1CIkV6sVcdESIXia41VeP3XlA5ZCydGNAt3uZSCUMKa9vM29vz4VYPUHUDqLuj1dRhgJy7sE3UtZi2y7Evl5l9hLZiOAf19n_eErm_C_B1s64_BMZ_u0)

![サンプル](https://www.plantuml.com/plantuml/png/JS-nJiCm4CRnFKzXt0v5YcAICA0Eg0DYvWtGrJdQa-spvJkhuktnWA3Zz_zDtqaKghOKXmTY7zk6vgfQvEXSXTjq8RssSnEiFhCYw-HpSX3go-m-QlOeyXxpxOtWKY6v1CIkV6sVcdESIXia41VeP3XlA5ZCydGNAt3uZSCUMKa9vM29vz4VYPUHUDqLuj1dRhgJy7sE3UtZi2y7Evl5l9hLZiOAf19n_eErm_C_B1s64_BMZ_u0)


### CRT

原因と結果を因果で表すCRT(現状ツリー)風の図を描くために使用します。

[サンプルコード](https://www.plantuml.com/plantuml/uml/DSonJiCm4CRntKzX2WD8A5RQj48Cg4DXPc6h51tNexLgtqNdi_hwEA1Ctt_uzDiN0xMQaxkBZAcUo5_Cfl8QWiAjEqUxrEzQI57OYAr3oG6k-jA7JnMaKZwIt0uHpWevP8WSK6qqaTHDrRa7OeiMgJokZkxhy7u_HRu-7nr2G_ibrajibXYMsBPOFJK8XPdGHgLIauq_AdcS7__q6lm6_03-pS_gjksCxjeEVZRwLhi_QazygZdz0W00)

![サンプル](https://www.plantuml.com/plantuml/png/DSonJiCm4CRntKzX2WD8A5RQj48Cg4DXPc6h51tNexLgtqNdi_hwEA1Ctt_uzDiN0xMQaxkBZAcUo5_Cfl8QWiAjEqUxrEzQI57OYAr3oG6k-jA7JnMaKZwIt0uHpWevP8WSK6qqaTHDrRa7OeiMgJokZkxhy7u_HRu-7nr2G_ibrajibXYMsBPOFJK8XPdGHgLIauq_AdcS7__q6lm6_03-pS_gjksCxjeEVZRwLhi_QazygZdz0W00)

#### entities

PFDの場合と同様です。

#### connect_result_from_causes

```
$connect_result_from_causes("原因1 原因2 > 結果1, 原因3 原因4 > 結果2")
```

`$entity`で定義された要素を原因/結果として接続し、因果関係を表します。

接続にあたって、以下の装飾をすることができます。

| 装飾方法 | 例 | 効果 |
|---|---|---|
| エイリアスの末尾にアルファベット1文字を付与 | 2a | 複数の原因をANDとして表す楕円を表示 |


### プロシージャの設定
いずれもinclude後、プロシージャの呼び出し前に設定する必要があります。

#### 項目番号の表示/非表示
`!$numbered = 0`と設定すると、定義された入出力とプロセスに付与された連番を非表示にすることができます。  

[サンプルコード](https://www.plantuml.com/plantuml/uml/HScnJiCm403GtL_XkXcA5CKaOK2LG1qGCt-07ETeJx7FbkzE5NzF9Ze-lGjBQA8vcGDIJBg2lObKFdlVmdsuejnqKOOM2mcG3B5a7xRsElglLY8mVwPo_y1mRI7x791y_Lc_fPGqL3Ncq97c1Hgiziq-6zC12Ge2dnpIsCSpqjGZpSUAjEqiAaVUVfUau9vCLqzkj9DzjxFhhUEGQuBnF-OzNXunxN41MZMdFm00)

![サンプル](https://www.plantuml.com/plantuml/png/HScnJiCm403GtL_XkXcA5CKaOK2LG1qGCt-07ETeJx7FbkzE5NzF9Ze-lGjBQA8vcGDIJBg2lObKFdlVmdsuejnqKOOM2mcG3B5a7xRsElglLY8mVwPo_y1mRI7x791y_Lc_fPGqL3Ncq97c1Hgiziq-6zC12Ge2dnpIsCSpqjGZpSUAjEqiAaVUVfUau9vCLqzkj9DzjxFhhUEGQuBnF-OzNXunxN41MZMdFm00)


#### 改行の自動挿入

`!$target_length = 任意の文字数`と設定すると、設定した文字数で各要素が改行されます。  
`!$newline_at_space`(1または0)を同時に設定することによって、動作が変わります。

| `$newline_at_space`の値 | 動作                                                                                                             |
|------------------------|----------------------------------------------------------------------------------------------------------------|
| 1                      | 設定した`$target_length`を超えた後、最初に現れた半角空白を改行に置き換えます。<br>(英語利用時に、単語の途中で改行されなくすることを意図しています) |
| 0                      | 設定した`$target_length`ごとに改行が挿入されます。                                                                          |

<details>
<summary>以前の書き方の説明</summary>


PFDを描くにあたって必要となる、`入力`と`プロセス`、`プロセス`と`出力`の接続を簡略化するためのプロシージャが定義されています。  
`入力`, `出力`は、`,`または` `(半角スペース)区切りで複数指定することができます。

| プロシージャ                       | 使用例                          | 説明                                                                                                                                                                                                                                                                                                                                | 結果                                         |
|------------------------------|---------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------|
| $p(outputs, inputs, process) | $p("out", "in1,in2", "process") | 事前に定義済みの要素を使ってプロセスを描画します。<br>```in1 --> process```<br>```in2 --> process```<br>```process --> out```<br>と同じです。事前に定義がない場合は、表示が異なる場合があります。                                                                                                                                                                      | ![p_sample](./image/2021-03-29-22-43-13.png) |
| $d(outputs, inputs, process) | $d("out", "in1 in2", "process") | プロシージャ内で要素を定義しながらプロセスを描画します。<br>```agent "in1" as in1```<br>```agent "in2" as in2```<br>```usecase "process" as process```<br>```in1 --> process```<br>```in2 --> process```<br>```process --> out```<br>と同じです。<br>`inputs, outputs`の要素を`c:`で始めた場合、その要素は雲形(`cloud`として定義)になります。<br>また、事前に定義がある場合は、エラーとなります。 | ![p_sample](./image/2021-03-29-22-43-13.png) |
| $c(color, inouts)            | $c("#pink", "in1 in2")          | それまでに定義された入出力に指定された色を付与します。<br>```agent "in1" as in1 #pink```<br>```agent "in2" as in2 #pink```<br>と同じです。<br>`$c`は`$d`による要素定義の後に使用するようにしてください。                                                                                                                                                                    | ![p_sample](./image/2021-06-03-19-52-32.png) |
| $cp(color, process)          | $c("#skyblue", "process")       | それまでに定義されたプロセスに指定された色を付与します。<br>```usecase "process" as process #skyblue```と同じです。<br>`$cp`は`$d`による要素定義の後に使用するようにしてください。                                                                                                                                                                                              | ![p_sample](./image/2021-07-18-14-51-41.png) |

### 設定項目
デフォルトでは、入出力の区切り文字は`,`と` `に設定されています。  
英語環境をご利用の場合など` `区切りが不便な場合は、`pfd_tool.pu`の
```
!$space_delimiter = 1
```
を0に変更することで、`,`のみを区切り文字とすることができます。

</details>

## VSCodeユーザスニペット/Obsidianテンプレート
`vscode`フォルダ内のファイルがスニペット、`obsidian`フォルダ内のファイルがテンプレートにあたります。  
必要に応じて呼び出せるよう設定を行ってください。  
その際、呼び出し側のmarkdownファイルから`pfd_tool.pu`が参照できるよう、パスを編集してください(スニペット/テンプレートからの挿入後に編集することも可)。

以下のスニペット/テンプレートが含まれています。

| 項目      | 呼び出し(VSCode) | 動作                           | 用途                                                                     |
|-----------|----------------|------------------------------|------------------------------------------------------------------------|
| PFDテンプレート | pfd            | コードブロックとPFDの記述サンプルを合わせて挿入 | 描き始める際に1回呼び出すことを想定しています                                          |
| プロセス追加  | p              | pプロシージャを挿入                   | 描きながら必要に応じて呼び出すことを想定しています                                        |
| CRTテンプレート | crt            | コードブロックとCRTの記述サンプルを合わせて挿入 | 描き始める際に1回呼び出すことを想定しています                                          |
| ノート追加   | n              | ノートブロックを挿入                   | 描きながら必要に応じて呼び出すことを想定しています<br>末尾の接続先として任意のエイリアスを指定してください |

<details>
<summary>以前のスニペットの説明</summary>

## VSCodeユーザスニペット
`markdown.json`がスニペットにあたります。  
ファイル->設定->ユーザスニペットと開き、markdown.jsonに内容を追加してください。  
![](./image/2021-03-29-22-54-16.png) 

またその際、`PFD template`のパス(画像の選択部分)を適切な値に修正してください。  
![](./image/2021-03-29-22-56-12.png)

プロシージャの入力をサポートするスニペットが定義されています。  
スニペットの呼び出しは、markdownドキュメントにおいて`使用例`の文字列入力後に`Ctrl+Space`で行います(VSCodeの設定によります)。

| スニペット                         | 使用例 | 説明                                                                                                         |
|-------------------------------|--------|------------------------------------------------------------------------------------------------------------|
| PFD template                  | pfd    | Markdown Preview EnhancedのPlantUMLコードブロックを、<br>`$d`プロシージャとともに出力します。<br>また、上述の区切り文字設定も合わせて出力します。 |
| draft process                 | d      | `$d`プロシージャを出力します。                                                                                          |
| draft process from clipboard  | c      | クリップボードの内容が`in`, `out`に入った`$d`プロシージャを出力します。                                                             |
| define color for deliverables | c      | `$c`プロシージャを出力します。                                                                                          |
| define color for process      | cp     | `$cp`プロシージャを出力します。                                                                                         |
| multi line note               | n      | 複数行ノートのテンプレートを出力します。<br>リンク先は適切に設定してください。                                                          |
| multi line note to clipboard  | nc     | 複数行ノートのテンプレートを出力します。<br>リンク先はクリップボードの要素になります。                                                        |

</details>

## 使用イメージ

1. PFDテンプレートを使って、PFDを描き始めます。
1. `$entities`に最終成果物から順に中間成果物を列挙します。
1. `$p`で最終成果物と入力となる中間成果物を接続します。
1. 2-3を必要なだけ繰り返します。

### 注意点

`$entities`に記述した入出力が不要であることが途中で分かった場合、単純に削除してしまうとエイリアスの連番が変わってしまい、プロセスとの接続関係が意図しないものになってしまいます。  
その場合、先頭に半角スペース1つを入れることで、要素を非表示にすることができます。  
(`$p`などで接続関係に指定されている場合はエイリアス名の要素として残ってしまうので、完全な対策ではありません)

<details>
<summary>以前の使用イメージ</summary>

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
</details>