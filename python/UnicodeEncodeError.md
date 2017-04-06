# UnicodeEncodeError について

## 事象

これの原因を学ぶ

```
# python issue-check.py
Traceback (most recent call last):
  File "issue-check.py", line 23, in <module>
    params =urllib.urlencode({'body':u'@' + username + u'さん \nご確認いただき、問題が無ければ当issueのクローズをお願いします'})
  File "/usr/lib64/python2.7/urllib.py", line 1326, in urlencode
    v = quote_plus(str(v))
UnicodeEncodeError: 'ascii' codec can't encode characters in position 17-18: ordinal not in range(128)
```

## 原因

* unicode型とstr型を結合しようとしている
* unicode型が求められているのに、str型を渡している

```
例：
username = 'test' ⇦ stg型
body = u'名前：' + username.decode('utf_8')

unicode型に対して文字結合を実施するため、str型のusernameをdecodeする
```

## 学習

[PythonのUnicodeEncodeErrorを知る - HDEラボ](http://lab.hde.co.jp/2008/08/pythonunicodeencodeerror.html) を読み解く

>str型は、代入された時点で、（ソースコードのエンコーディングがUTF-8の場合）UTF-8の文字コード列であるため、s1は、UTF-8の文字コード列としてそのまま出力することが可能です（pythonから見ると、特に意味の無いバイト列になります）。

> 一方、「unicode型」とは、「Unicodeを表現可能な、とある内部形式」であって、上記のu1は、どの文字コード列（エンコーディング）でもありません。

## Link
  * [UnicodeDecodeError - Python Wiki](https://wiki.python.org/moin/UnicodeDecodeError)
  * [PythonのUnicodeEncodeErrorを知る - HDEラボ](http://lab.hde.co.jp/2008/08/pythonunicodeencodeerror.html)
  * [Unicodeとは？ その歴史と進化、開発者向け基礎知識 - Build Insider](http://www.buildinsider.net/language/csharpunicode/01)

> The UnicodeDecodeError normally happens when decoding an str string from a certain coding. Since codings map only a limited number of str strings to unicode characters, an illegal sequence of str characters will cause the coding-specific decode() to fail

UnicodeDecodeErrorは、str型の文字列をdecodeした時に発生することが多い。コーディングは制限された数のstr文字列をunicode文字に対応付けるので、不正なstr型の文字列シーケンスはdecodeの失敗を引き起こす。

