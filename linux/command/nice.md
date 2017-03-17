# nice

## nice とは

```
nice [-n adjustment] [-adjustment] [--adjustment=adjustment] [command [arg...]]
```

```
オプション
       -n adjustment, -adjustment, --adjustment=adjustment
              command の優先度に加える値を、 10 ではなく adjustment にする。

       --help 標準出力に使用方法のメッセージを出力して正常終了する。

       --version
              標準出力にバージョン情報を出力して正常終了する。
```

* スケジュール優先度を変更してプログラムを実行するコマンド
* UNIXでは、すべてのプログラムに同じCPU時間を割りあてるのではなく、優先度というものを設けて、優先度の高いプログラムに多くのCPU時間を割りあてます。そして、niceコマンドはこの優先度を変更する際に使用します。
* 引数なしで実行すると、 nice は自身が継承したスケジューリング優先度を表示する。
* それ以外の場合には、 nice はスケジューリング優先度を調整してから与えられたcommand を実行する。
* nice によって調整できる優先度の範囲は -20 (優先度最高) から 19 (優先度最低) まで
* 負の値の優先度を指定できるのは、通常はスーパーユーザー (root) のみです。つまり、一般ユーザーは自分のプロセスの優先度を下げることしかできません。

## nice値の調べ方

* top
* ps -l でNI列を見る

```
Tasks: 206 total,   1 running, 205 sleeping,   0 stopped,   0 zombie
Cpu(s):  1.2%us,  1.0%sy,  0.0%ni, 97.7%id,  0.0%wa,  0.0%hi,  0.0%si,  0.0%st
Mem:  16334000k total,  2212248k used, 14121752k free,   132860k buffers
Swap:  4194300k total,        0k used,  4194300k free,   972488k cached

  PID USER      PR  NI  VIRT  RES  SHR S %CPU %MEM    TIME+  COMMAND
    1 root      20   0 19364 1620 1308 S  0.0  0.0   0:01.08 init
    2 root      20   0     0    0    0 S  0.0  0.0   0:00.00 kthreadd
    3 root      RT   0     0    0    0 S  0.0  0.0   0:01.64 migration/0
    4 root      20   0     0    0    0 S  0.0  0.0   0:00.14 ksoftirqd/0
```

## 注意点 

### 引数にコマンド・リストやパイプラインを使用できない


### シェル内の組み込みコマンドとは違う

```
nice [-n adjustment] [COMMAND [ARG...]] 
```
 
ほとんどのシェルには同名の組み込みコマンドがあるので、単に`nice`として実行すると、ここで記述されたものとは異なった機能のものが実行される。

> シェル組込みコマンドとしてのniceコマンドは、コマンド形式が異なり、優先度の前に+か-を付けて、指定します。そのため「-5」のような指定があり、この場合マイナス5を表します。つまり、「-」は「マイナス」であり「ハイフン」ではありません。

> cshなどを使用している場合は、明示的に「/bin/nice」のようなフルパスで指定する必要があります

* [CodeZine（コードジン）- nice](https://codezine.jp/unixdic/w/nice)

## おまけ

### renice
プロセスを開始した後に、別の優先度で実行しなければならないことに気付いた場合、プロセス開始後にその優先度を変更するコマンド。

## 参考

* [Linux の 101 試験対策: プロセス実行の優先度](https://www.ibm.com/developerworks/jp/linux/library/l-lpic1-v3-103-6/)
