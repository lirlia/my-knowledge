# unicode

* [Unicodeとは？ その歴史と進化、開発者向け基礎知識 - Build Insider](http://www.buildinsider.net/language/csharpunicode/01)

## unicodeとUTF-8

* unicode 符号化文字集合
* UTF-8 符号化方式の一種

> Unicodeは「符号化文字集合」と言われ、世界中の文字を集め、それぞれの文字に対して、番号を付けて管理している文字の集合の事です。例えば、「あ」という文字は、Unicodeでは、16進表記で「304002」、10進表記で「3162114」で登録されています。(16進数はパソコン上の数字の数え方です)

* [文字コードとは？~UTF-8はパソコンの世界共通語~｜データ分析用語を解説 - データビジュアライズで経営を視える化する/graffe グラーフ](http://www.graffe.jp/blog/1278/)

そもそも同列におらず、UTF-8はunicodeを解釈する時の方式の一つ。shift-JISやeucも符号化方式の一種。（一口に文字コードというのが誤り）

## unicode とは

> Unicodeで規定されている文字1つ1つには、最大で21bits（16進数で5～6桁）の数値が割り振られている。この数値をコードポイント（code point： 符号点、符号位置）という。

※16進数は 0000~1111 の4bitで表すことができる

> この21bitsのコードポイントがそのままテキストファイルに保存されるわけではない。<br>
<br>UTF-32： コードポイントをそのまま4Bytesの固定長で記録する<br>
UTF-16： 2Bytes以下に収まるコードポイントはそのまま2Bytesの整数として記録し、それを超えるものはペア（2×2で4Bytes）を使って記録する<br>
UTF-8： 1～4Bytesの可変長で記録する<br>

一口に文字と言っても様々な要素がある。

* 書記素クラスター： ユーザーが文字として認識する単位。フォントレンダリングや文字の削除／選択にはこの単位での処理が必要
* コードポイント： Unicodeが値を定めている文字の単位
* 符号化： 符号化の方式（UTF-16やUTF-8）によって、保存や通信の際に必要となるデータ量が変わる

## ASCII とは

> 最初に文字コードを標準化したのはアメリカであり、その文字コードはASCII（American Standard Code for Information Interchange）と呼ばれている。各国の文字コードはASCIIをベースに、裁量が認められていた数文字の変更や、ASCIIが使っていない8bits（80以上）のコードポイントに文字を追加したものである。

> Unicode登場以前は、国ごと・言語ごとにばらばらに文字コードを定めていた。文字数の少ない西欧のラテン文字圏では、1Byteの共通の文字コードを使っていたが、ギリシャ語やアラビア語を使う国々ではそれぞれ独自の文字コードを持つ必要があった。まして、文字数が多い日本語や中国語はいわずもがなである。

1Byte=8bit=256文字しかなかったが、海外ではそもそもそんなに文字がないので困っていない。

> 当初、言語を混在させたい場合には、エスケープシーケンス（＝ESC文字（1B）に続く数文字で何らかの制御を行う手法）を使って文字コードを切り替えて使う想定だった。実際、日本語でいうと俗にいう「JISコード」（正式にはISO-2022-JPという規格になる）がそれに当たる。ASCII（正確にはその日本版であるJIS X 0201）でラテン文字を入力したければ1B 28 42（ESC ( B）という3文字で、日本語文字コード（JIS X 0208と呼ばれている）で日本語文字を入力したければ1B 24 42（ESC $ B）という3文字で、文字コードを切り替える。


> 文字集合： どういう文字にコードポイントを与えるか、コードポイントの数値の範囲をどうするか(JIS X 0208は文字集合を規定する規格)<br><br>
符号化方式： 文字集合をどういうバイト列で表現するか(ISO-2022-JP、Shift_JIS、EUC-JPはいずれも、JIS X 0201（ASCII）とJIS X 0208という2つの文字集合を記録するための符号化方式)

## 歴史

* ASCII作ったけど、文字数が全然足りない
* unicodeを作ろう！（当時2byte 65536文字あれば足りると思われてた）
* ASCII と互換性ないとだめだよね

> 既存資産を考えると、ASCIIとの互換性は必須である。Unicodeでももちろんそれを考慮して、1Byte領域（00～FF）のコードポイントが全てLatin-1と同じになっている。しかし、2Bytes固定長にするなら、図4に示すように00を挟む必要があって、ASCII文字列をそのまま扱うことができない。

> 元のLatin-1文字列が入ったバイト列の2倍の長さのメモリを確保する1文字1文字、00を挟みつつ文字をコピーしていく前述の通り、ASCIIとの互換性は非常に重要である。そこで、やはり、UnicodeでもASCII互換な符号化方式が必要とされた。それがUTF-8である。UTF-8ではコードポイントを、表3に示すルールで符号化する。