# Linux Command

## find & xargs

```
find . -type f -name '*.txt' -print0 | xargs -0 rm -f
```

### find print0 

```
-print0
              真 を返す。ファイル名をフルパスで標準出力に表示し、各ファイル名にヌル文字を付加する。このオプション
              を用いれば、 find の出力を処理するプログラムにおいて改行文字を含んだファイル名を正しく解釈できる よ
              うになる。
```

### xargs -0 の意味

```
--null, -0
              標準入力からの文字列の区切りに、空白ではなくヌル文字が使われているとみなす。また引用符やバックス ラ
              ッ シュに特別の意味を持たせず、すべての文字をそのまま用いる。ファイル終了文字列も無効となり、他の文
              字列と同じように扱われる。入力される文字列に空白・引用符・バックスラッシュが含まれている場合に有 用
              で あろう。 GNU find の -print0 オプションの出力を、このオプションを指定した xargs の入力とすると良
              い。
```

### 動かしてみる

```
$ find . -name "*conf" -print0
./conf.d/filter.conf./conf.d/acl.conf./conf.d/wls.conf./conf./conf/httpd.conf
```

`-0`をつけないと、エラーが発生する。

```
find . -name "*conf" -print0 | xargs ls
xargs: Warning: a NUL character occurred in the input.  It cannot be passed through in the argument list.  Did you mean to use the --null option?
./conf.d/filter.conf
```

### メリット

ファイル名に改行コードが含まれている場合にも、正しく動かすことができる。  
※通常のfindは改行で区切られるため、改行を含むファイル名は2行と扱われる

* 参考
  * [Linux でファイル名に改行コードを入れる | PaePoi](http://palepoli.skr.jp/wp/2015/03/10/linux_filename_in_0x0a/)

```
$ touch "aaa
> bbb"
$ find . -type f | xargs ls
ls: cannot access ./aaa: そのようなファイルやディレクトリはありません
ls: cannot access bbb: そのようなファイルやディレクトリはありません
./conf.d/acl.conf  ./conf.d/filter.conf  ./conf.d/wls.conf  ./conf/httpd.conf  ./conf/magic
$
$ find . -type f -print0 | xargs -0 ls
./aaa?bbb  ./conf.d/acl.conf  ./conf.d/filter.conf  ./conf.d/wls.conf  ./conf/httpd.conf  ./conf/magic
```
