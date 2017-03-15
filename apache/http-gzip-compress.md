# HTTPレスポンスのgzip圧縮

## 参考

- [gzip圧縮転送についてトコトン調べてみた - satoyan419.com](http://satoyan419.com/gzip-compression/)
- [HTTP compression - Wikipedia](https://en.wikipedia.org/wiki/HTTP_compression)
- [HTTP圧縮 | J-Stream CDN情報サイト](https://tech.jstream.jp/blog/cache/http_compression/)
- [mod_deflate - Apache HTTP サーバ バージョン 2.2](https://httpd.apache.org/docs/2.2/ja/mod/mod_deflate.html)

## 概要

- HTTP 1.1の技術
- HTTPにおけるクラサバ通信において、サーバーから返却するコンテンツを圧縮することで帯域を減らす技術
- HTTPレスポンスで返却するコンテンツ（text/css/js)をgzip圧縮し帯域の消費を減らす
- jpegやpdfなどすでに圧縮済みのものは逆にファイルサイズが大きくなったりするため圧縮しない
- 帯域を減らすことで通信速度を上昇させる（ユーザビリティの向上）

## 設定方法

### 2パターンある

Apache module | 方式                                   | デメリット
:------------ | :----------------------------------- | :--------------------------
mod_rewrite   | あらかじめgzip形式のファイルを用意しておき、それにアクセスさせる方法 | 都度コンテンツ更新のたびにファイルを作成する必要がある
mod_deflate   | アクセスの都度コンテンツを圧縮しレスポンスを返す方法           | CPU負荷がかかる

### クライアント＆サーバー設定

ブラウザが圧縮ファイルをサポートしている場合、HTTPリクエストに次のような「Accept-Encoding」ヘッダを付加します。

```
GET user-domain/index.html HTTP/1.1
Host: user-domain
User-Agent: Mozilla/5.0 (Windows NT 5.1; rv:12.0) Gecko/20100101 Firefox/12.0
Accept: */*
Accept-Language: ja,en-us;q=0.7,en;q=0.3
Accept-Encoding: gzip, deflate ⇦
Connection: keep-alive
Pragma: no-cache
Cache-Control: no-cache
```

このリクエストを受信したサーバは、圧縮ファイルを返却できる場合、次のような「Content-Encoding」ヘッダを付与したレスポンス（と圧縮したファイル）を返却します。

```
HTTP/1.1 200 OK
Date: Thu, 31 May 2012 10:02:44 GMT
Server: Apache/2.2.8 (Win32) DAV/2 mod_ssl/2.2.8 OpenSSL/0.9.8g mod_autoindex_color mod_fastcgi/2.4.6 PHP/5.2.5
Last-Modified: Sat, 16 Jun 2012 06:24:56 GMT
Etag: "8000000043450-25c3-4c111066fbeff"
Accept-Ranges: bytes
Content-Length: 9667
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Content-Type: text/javascript; charset=utf-8
Content-Encoding: gzip ⇦
```

[mod_deflate - Apache HTTP サーバ バージョン 2.2](https://httpd.apache.org/docs/2.2/ja/mod/mod_deflate.html)

## mod_deflate利用時の注意点(proxy)

- [mod_deflate - Apache HTTP サーバ バージョン 2.2](https://httpd.apache.org/docs/2.2/ja/mod/mod_deflate.html#page-header)

mod_deflate モジュールは Vary: Accept-Encoding HTTP 応答ヘッダを送信して、適切な Accept-Encoding リクエストヘッダを送信するクライアントに対してのみ、 プロクシサーバがキャッシュした応答を送信するように注意を喚起します。 このようにして、圧縮を扱うことのできないクライアントに 圧縮された内容が送られることのないようにします。

もし特別に何かに依存して除外したい場合、例えば User-Agent ヘッダなどに依存している場合、手動で Vary ヘッダを設定して、 追加の制限についてプロクシサーバに注意を行なう必要があります。 例えば User-Agent に依存して DEFLATE を追加する典型的な設定では、次のように追加することになります。

```
Header append Vary User-Agent
```

リクエストヘッダ以外の情報 (例えば HTTP バージョン) に依存して圧縮するかどうか決める場合、 Vary ヘッダを * に設定する必要があります。 このようにすると、仕様に準拠したプロクシはキャッシュを全く行なわなくなります。

例

```
Header set Vary *
```

## Apache以外でも使われてる？

* [nginxパフォーマンスチューニング〜静的コンテンツ配信編〜 - Qiita](http://qiita.com/cubicdaiya/items/2763ba2240476ab1d9dd)
* [node.js HTTPサーバでレスポンスをgzipやdeflateで圧縮する - Node.js/JavaScript入門](http://kaworu.jpn.org/javascript/node.js_HTTP%E3%82%B5%E3%83%BC%E3%83%90%E3%81%A7%E3%83%AC%E3%82%B9%E3%83%9D%E3%83%B3%E3%82%B9%E3%82%92gzip%E3%82%84deflate%E3%81%A7%E5%9C%A7%E7%B8%AE%E3%81%99%E3%82%8B)
* [Amazon API GatewayがContent-Encoding: gzipをサポートをしていた件 - Qiita](http://qiita.com/ma2shita/items/4e3c16617a6e17dda01e)

## おまけ

### payload

[ペイロードとは - IT用語辞典 Weblio辞書](http://www.weblio.jp/content/%E3%83%9A%E3%82%A4%E3%83%AD%E3%83%BC%E3%83%89)

```
ペイロードとは、IT用語としては、パケット通信においてパケットに含まれるヘッダやトレーラなどの付加的情報を除いた、データ本体のことである。
```

### gzipとdeflate(デフレート)

評価軸 | deflate                                        | gzip
:-- | :--------------------------------------------- | :-------------------------------
説明  | LZ77とハフマン符号化を組み合わせた可逆データ圧縮アルゴリズム。gzipなどで使われている | データ圧縮プログラムのひとつ、およびその圧縮データのフォーマット

- HTTPヘッダにおけるdeflateは素のdeflateではなくzlib＋deflateのこと。
- 参考

  - [gzip圧縮されたデータの展開方法いろいろ - Qiita](http://qiita.com/mpyw/items/eb6ef5e444c2250361b5)
  - [HTTP - HTTP圧縮でdeflateではなくgzipが採用される理由とは?(5026)｜teratail](https://teratail.com/questions/5026)
