review-jsbook.cls Users Guide
====================

現時点における最新版 `jsbook.cls  2018/06/23 jsclasses (okumura, texjporg)` をベースに、Re:VIEW 向け review-jsbook.cls を実装しました。

過去の Re:VIEW 2 で jsbook.cls で作っていた資産を、ほとんどそのまま Re:VIEW 3 でも利用できます。

## 特徴

 * クラスオプション `cameraready` により、「印刷用」「電子用」の用途を明示的な意思表示として与えることで、用途に応じた PDF ファイル生成を行えます。
 * （基本的に）クラスオプションを `<key>=<value>` で与えられます。
 * クラスオプション内で、用紙サイズや基本版面を自由に設計できます。

ここで、クラスオプションとは、親 LaTeX 文章ファイルにおいて、以下のような位置にカンマ（,）区切りで記述するオプションです。

```latex
\documentclass[クラスオプションたち（省略可能）]{review-jsbook}
```

## Re:VIEW で利用する

クラスオプションオプションたちは、Re:VIEW 設定ファイル config.yml 内の texdocumentclass において、以下のような位置に記述します。

```yaml
texdocumentclass: ["review-jsbook", "クラスオプションたち（省略可能）"]
```

## 利用可能なクラスオプションたち

### 用途別 PDF データ作成 `cameraready=<用途名>`

印刷用 `print`、電子用 `ebook` のいずれかの用途名を指定します。

 * `print`［デフォルト］：印刷用 PDF ファイルを生成します。
   * トンボあり、デジタルトンボあり、hyperref パッケージを `draft` モードで読み込み、表紙は入れない
 * `ebook`：電子用PDFファイルを生成します。
   * トンボなし、hyperref パッケージを読み込み、表紙を入れる

### 表紙の挿入有無 `cover=<trueまたはfalse>`

`cameraready` の値によって表紙（config.yml の coverimage に指定した画像）の配置の有無は自動で切り替わりますが、`cover=true` とすれば必ず表紙を入れるようになります。

なお、config.yml の coverimage で指定する画像ファイルは、原寸を想定しています。

### 特定の用紙サイズ `paper=<用紙サイズ>`

利用可能な特定の用紙サイズを指定できます。

 * `a3` 
 * `a4` 
 * `a5`［デフォルト］
 * `a6` 
 * `b4`：JIS B4 
 * `b5`：JIS B5
 * `b6`：JIS B6 
 * `a4var`：210mm x 283mm
 * `b5var`：182mm x 230mm
 * `letter`
 * `legal`
 * `executive`

### トンボ用紙サイズ `tombopaper=<用紙サイズ>` および塗り足し幅 `bleed_margin=<幅>`

`tombopaper` ではトンボ用紙サイズを指定できます。
［デフォルト］値は自動判定します。

`bleed_margin` では塗り足し領域の幅を指定できます。
［デフォルト］3mm になります。

### カスタム用紙サイズ `paperwidth=<用紙横幅>`, `paperheight=<用紙縦幅>`

カスタム用紙サイズ `paperwidth=<用紙横幅>`, `paperheight=<用紙縦幅>` （両方とも与える必要があります）を与えることで、特定の用紙サイズで設定できない用紙サイズを与えられます。

たとえば、B5変形 `paperwidth=182mm`, `paperheight=235mm`。

### 基本版面設計 `Q=<級数>`, `W=<字詰>`, `L=<行数>`, `H=<行送り>` , `head=<天>`, `gutter=<ノド>`

基本版面 QWLH, 天、ノドを与えます。
天、ノドをそれぞれ与えない場合、それぞれ天地、左右中央になります。

 * `Q=13`［デフォルト］：文字サイズを級数（1Q = 1H = 0.25mm）で与えます。
 * `W=35`［デフォルト］：1行字詰めを与えます。
 * `L=32`［デフォルト］：行数を与えます。
 * `H=22`［デフォルト］：行送り（1Q = 1H = 0.25mm）を与えます。
 * `head=<幅>`：天を与えます。［デフォルト］は天地中央です。
 * `gutter=<幅>`：ノドを与えます。［デフォルト］は左右中央です。

例をいくつか挙げます。

 * `paper=a5, Q=13, W=35, L=32, H=22,`
 * `paper=a5, Q=14, W=38, L=34, H=20.5, head=20mm, gutter=20mm,`
 * `paper=b5, Q=13, W=43, L=35, H=24,` 
 * `paper=b5, Q=14, W=40, L=34, H=25.5,` 

さらに、ヘッダー、フッターに関する位置調整は、TeX のパラメータ `\headheight`, `\headsep`, `\footskip` に対応しており、それぞれ `headheight`, `headsep`, `footskip` を与えられます。

## 開始ページ番号 `startpage=<ページ番号>`

大扉からのページ開始番号を指定します。

［デフォルト］は1です。表紙・表紙裏（表1・表2）のぶんを飛ばしたければ、`startpage=3` とします。

## 通しページ番号（通しノンブル） `serial_pagination=<trueまたはfalse>`

大扉からアラビア数字でページ番号を通すかどうかを指定します。

 * `true`：大扉を開始ページとして、前付（catalog.yml で PREDEF に指定したもの）、さらに本文（catalog.yml で CHAPS に指定したもの）に連続したページ番号をアラビア数字で振ります（通しノンブルと言います）。
 * `false`［デフォルト］：大扉を開始ページとして前付の終わり（通常は目次）までのページ番号をローマ数字で振ります。本文は 1 を開始ページとしてアラビア数字で振り直します（別ノンブルと言います）。

### 隠しノンブル 'hiddenfolio=<プリセット>'

印刷所固有の要件に合わせて、ノドの目立たない位置に小さくノンブルを入れます。
'hiddenfolio` にプリセットを与えることで、特定の印刷所さん対応の隠しノンブルを出力することができます。
利用可能なプリセットは、以下のとおりです。

 * `default`：トンボ左上（塗り足しの外）にページ番号を入れます。
 * `marusho-ink`（丸正インキ）：塗り足し幅を5mmに設定、ノド中央にページ番号を入れます。
 * `nikko-pc`（日光企画）, `shippo`（ねこのしっぽ）：ノド中央にページ番号を入れます。

独自の設定を追加したいときには、review-jsbook.cls の実装を参照してください。

ページ番号は紙面に入れるものと同じものが入ります。アラビア数字で通したいときには、上記の `serial_pagination=true` も指定してください。

## 標準で review-jsbook.cls を実行したときの jsbook.cls との違い

 * jsbook.cls のクラスオプション `uplatex`：これまで texdocumentclass に指定が必要だった `uplatex` オプションは不要となっています。
 * jsbook.cls のクラスオプション `nomag`：用紙サイズや版面設計は、すべて review-jsbook.cls 側で行います。
 * hyperref パッケージ：あらかじめ hyperref パッケージを組み込んでおり、`cameraready` オプションにより用途別で挙動を制御します。
 * 各種相対フォントサイズコマンド `\small`, `\footnotesize`, `\scriptsize`, `\tiny`, `\large`, `\Large`, `\LARGE`, `\huge`, `\Huge`, `\HUGE` は、級数ベースに書き換えています。おおむね妥当な値になっているはずです。