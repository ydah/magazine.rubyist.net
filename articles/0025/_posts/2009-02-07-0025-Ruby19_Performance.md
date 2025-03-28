---
layout: post
title: Ruby 1.9 で Web アプリを想定したベンチマークをとってみた
short_title: Ruby 1.9 で Web アプリを想定したベンチマークをとってみた
created_on: 2009-02-07
tags: 0025 Ruby19_Performance
---
{% include base.html %}


著者: 桑田 誠

* Table of content
{:toc}


## はじめに

以前から Ruby の動作速度は遅いといわれていました。
しかし、アプリケーションの動作速度にはデータベースやライブラリやアルゴリズムなどが大きく影響するため、言語の速度がアプリケーションの速度を決定するわけではありません。

本稿では以下のような複数の Ruby 実装でベンチマークをとることで、__言語の速度がそのままアプリケーションの速度になるわけではない__ことを具体的に説明します。

* Ruby 1.8.7
* Ruby 1.9.1
* Rubinius (2009-01-18 版)


なお本稿で想定するアプリケーションは、一般的な Web アプリケーションです。
これは、現在 Ruby がいちばんよく使われている分野が Web アプリケーションであることが理由です。

### ベンチマーク環境

今回は、Mac OS X 環境と Linux 環境とでベンチマークを行ないました。
各環境のマシンは次の通りです。

* Mac OS X
  * Version: 10.5.6 (Darwin 9.6.0)
  * CPU: Intel Core 2 Duo 2.8 GHz
  * Memory: 2 GB 800 MHz DDR2 SDRAM
* Linux
  * Version: Redhat 5.2 (2.6.18-53.1.14.el5 SMP i386 GNU/Linux)
  * CPU: Intel Xeon 3065 2.33GHz (stepping 11)
  * Memory: 1 GB


また各 Ruby 実装のバージョンは以下の通りです。

{% highlight text %}
{% raw %}
### Mac OS X
$ ruby187 -v
ruby 1.8.7 (2008-08-11 patchlevel 72) [i686-darwin9.6.0]
$ ruby191 -v
ruby 1.9.1 (2008-12-30 patchlevel-0 revision 21203) [i386-darwin9.6.0]
$ rbx -v
rubinius 0.10.0 (ruby 1.8.6) (6de41e35a 12/31/2009) [i686-apple-darwin9.6.0]
{% endraw %}
{% endhighlight %}


{% highlight text %}
{% raw %}
### Linux
$ ruby187 -v
ruby 1.8.7 (2008-08-11 patchlevel 72) [i686-linux]
$ ruby191 -v
ruby 1.9.1 (2008-12-30 patchlevel-0 revision 21203) [i686-linux]
$ rbx -v
rubinius 0.10.0 (ruby 1.8.6) (6de41e35a 12/31/2009) [i686-pc-linux-gnu]
{% endraw %}
{% endhighlight %}


### ベンチマークスクリプト

ベンチマークスクリプトはすべて[githubから入手できます](http://github.com/kwatch/shelves/tree/HEAD/rubyist-magazine/0025-appbench)。

## フィボナッチ数列のベンチマーク

まず、ベンチマークとしてはポピュラーなフィボナッチ数列を試してみましょう。

fib-bench.rb:

{% highlight text %}
{% raw %}
def fib(n)
  n <= 1 ? 1 : fib(n-1) + fib(n-2)
end

n = (ENV['N'] || $N || 35).to_i
p fib(n)
{% endraw %}
{% endhighlight %}


実行結果 (Mac OS X):

{% highlight text %}
{% raw %}
[macosx]$ /usr/bin/time ruby187 -s fib-bench.rb -N=35
14930352
       11.78 real        11.72 user         0.01 sys

[macosx]$ /usr/bin/time ruby191 -s fib-bench.rb -N=35
14930352
        3.05 real         3.03 user         0.01 sys

[macosx]$ N=35 /usr/bin/time rbx fib-bench.rb
14930352
        5.85 real         5.78 user         0.03 sys
{% endraw %}
{% endhighlight %}


実行結果 (Linux):

{% highlight text %}
{% raw %}
[linux]$ /usr/bin/time ruby187 -s fib-bench.rb -N=35
14930352
13.54user 0.00system 0:13.55elapsed 99%CPU (0avgtext+0avgdata 0maxresident)k
0inputs+0outputs (0major+433minor)pagefaults 0swaps

[linux]$ /usr/bin/time ruby191 -s fib-bench.rb -N=35
14930352
2.78user 0.00system 0:02.78elapsed 99%CPU (0avgtext+0avgdata 0maxresident)k
0inputs+0outputs (0major+690minor)pagefaults 0swaps

[linux]$ N=35 /usr/bin/time rbx  fib-bench.rb 
14930352
6.22user 0.01system 0:06.24elapsed 99%CPU (0avgtext+0avgdata 0maxresident)k
0inputs+0outputs (0major+4995minor)pagefaults 0swaps
{% endraw %}
{% endhighlight %}


実行結果をみると、

* Ruby 1.9.1 は Ruby 1.8.7 の約 4 倍から 5 倍高速
* Rubinius は Ruby 1.8.7 の約 2 倍高速


という結果になりました。
特に Ruby 1.9.1 は、(いい意味で) とても同じ Ruby とは思えないぐらいの実行結果を叩きだしています。

ただ Web アプリケーションでフィボナッチ数列が必要となることはまずありません。
そこで今度は、より Web アプリケーションよりのベンチマークをとってみることにします。

## eRuby によるベンチマーク

Web アプリケーション寄りのベンチマークで、かつ手軽に試せるものとして、eRuby を使ったベンチマークを用意しました ([こちら](http://github.com/kwatch/shelves/tree/HEAD/rubyist-magazine/0025-appbench)に置いてあります)。
このベンチマークは、eRuby を使って約 330 行の HTML ページを生成するベンチマークです。

なお eRuby 処理系としては [Erubis](http://www.kuwata-lab.com/erubis/) を使用しています。
これは、ERB だと Ruby のバージョンによって大きく速度が異なるため、今回のベンチマークにはふさわしくないと判断したためです。

eruby-bench.rb:

{% highlight text %}
{% raw %}
## オプションの解析 
$N = ENV['N'] unless defined?($N)       # 繰り返し回数
$e = ENV['E'] unless defined?($e)       # HTML エスケープする/しない
$cgiext = nil unless defined?($cgiext)  # require 'cgiext' する/しない

## eRuby ファイルをコンパイルしてメソッドに変換
require 'rubygems'
require 'erubis'
filename = "view.rhtml"
str = File.read(filename)
escape = $e ? true : false
eruby = Erubis::FastEruby.new(str, :escape=>escape)
src = eruby.src
eval "def render(list); #{src}; end", binding(), filename

## テストデータの読み込み
class StockInfo
  attr_accessor :name, :url, :symbol      # string
  attr_accessor :price, :change, :ratio   # float
end
require "yaml"
data = YAML.load_file("data.yaml")
list = []
data["list"].each do |hash|
  item = StockInfo.new
  item.name   = hash['name']
  item.url    = hash['url']
  item.symbol = hash['symbol']
  item.price  = hash['price']
  item.change = hash['change']
  item.ratio  = hash['ratio']
  list << item
end

## その他の処理
require "cgiext" if $cgiext
$N = ($N || 100000).to_i
if $N == 1
  print render(list)
  exit
end

## ベンチマークを実行
require "benchmark"
Benchmark.bm do |r|
  r.report do
    $N.times { render(list) }
  end
end
{% endraw %}
{% endhighlight %}


実行結果 (Mac OS X):

{% highlight text %}
{% raw %}
[macosx]$ ruby187 -s eruby-bench.rb -N=100000
      user     system      total        real
 23.250000   0.060000  23.310000 ( 23.798767)

[macosx]$ ruby191 -s eruby-bench.rb -N=100000
      user     system      total        real
 26.040000   0.090000  26.130000 ( 26.243537)

[macosx]$ N=100000 rbx bench2.rb
      user     system      total        real
442.980005   0.000000 442.980005 (442.980004)
{% endraw %}
{% endhighlight %}


実行結果 (Linux):

{% highlight text %}
{% raw %}
[linux]$ ruby187 -s eruby-bench.rb -N=100000
      user     system      total        real
 22.520000   0.000000  22.520000 ( 22.520964)

[linux]$ ruby191 -s eruby-bench.rb -N=100000
      user     system      total        real
 23.460000   0.090000  23.550000 ( 23.558899)

[linux]$ N=100000 /usr/bin/time rbx eruby-bench.rb 
      user     system      total        real
475.965906   0.000000 475.965906 (475.965864)
{% endraw %}
{% endhighlight %}


この結果を見ると、

* Ruby 1.9.1 は Ruby 1.8.7 より約 5 % から 10 % 遅い
* Rubinius は Ruby 1.8.7 より約 20 倍遅い


という結果になりました。
フィボナッチ数列のベンチマークとはまるで違います。
特に Rubinius は、(悪い意味で) とても同じ Ruby とは思えないほどの結果となってしまいました。

Ruby 1.9.1 が 1.8.7 より遅いのはおそらく、文字列の多言語化 (M18N) によって文字列処理のコストが増大したせいだと思われます。
また Rubinius が極端に遅いのは、Rubinius の String クラスが C ではなく Ruby で実装されていることが原因でしょう。
つまり、文字列処理に関しては Ruby 1.8.7 がいちばん高速であり、それが eRuby のベンチマーク結果に反映されたということです。

Web アプリケーションでは、数値計算よりも文字列処理のほうがずっと多いです。
そのため、フィボナッチ数列のようなベンチマークをとったところで Web アプリケーションの速度は分かりません。
特に今回のようなベンチマークでは、処理系エンジンの性能よりも、組み込み関数やメソッドの実行速度のほうが重要になります。
Ruby 1.8.7 では文字列に関する組み込みメソッドが C 言語で実装されているため十分高速であり、Rubinius のように C で実装されていない場合は極端に遅くなることがあるわけです。

今回のベンチマーク結果を見ると、__アプリケーションの速度には処理系の速度よりも組み込み関数やメソッドの性能のほうが重要__であることがわかります。

## HTML エスケープに関するベンチマーク

先ほどの eRuby ベンチマークでは、HTML エスケープ処理をしていませんでした。
実は HTML エスケープ処理も含めてベンチマークをとると、興味深い結果が得られます。
ここではそれを紹介してみます。

HTML エスケープ処理を含めてベンチマークをするには、先ほどのベンチマークスクリプトに「-e」オプションをつけるか、環境変数「$E」を設定します。

実行結果 (Mac OS X):

{% highlight text %}
{% raw %}
[macosx]$ ruby187 -s eruby-bench.rb -N=100000 -e
      user     system      total        real
 49.010000   0.180000  49.190000 ( 49.313947)

[macosx]$ ruby191 -s eruby-bench.rb -N=100000 -e
      user     system      total        real
 65.310000   0.170000  65.480000 ( 65.673570)

[macosx]$ N=100000 E=true rbx eruby-bench.rb
      user     system      total        real
672.767837   0.000000 672.767837 (672.767842)
{% endraw %}
{% endhighlight %}


実行結果 (Linux):

{% highlight text %}
{% raw %}
[linux]$ ruby187 -s eruby-bench.rb -N=100000 -e
      user     system      total        real
 47.410000   0.050000  47.460000 ( 47.468278)

[linux]$ ruby191 -s eruby-bench.rb -N=100000 -e
      user     system      total        real
 58.130000   0.010000  58.140000 ( 58.146540)

[linux]$ N=100000 E=true rbx eruby-bench.rb
      user     system      total        real
709.440972   0.000000 709.440972 (709.440978)
{% endraw %}
{% endhighlight %}


この結果を見ると、次のことがわかります。

* Ruby 1.8.7 では、HTML エスケープ "あり" は "なし" より 2 倍以上遅い
* Ruby 1.9.1 では、HTML エスケープ "あり" は "なし" より約 2.5 倍遅い
* Rubinius では、HTML エスケープ "あり" は "なし" より約 1.5 倍遅い


つまり、__eRuby による HTML 生成コストより HTML エスケープ処理のコストのほうが大きい__というわけです。

今回のベンチマークで使ったデータには、HTML エスケープが必要な文字 (「&amp;&lt;&gt;"」) は一切含まれていません。
にも関わらず、たかが HTML エスケープ処理の動作コストが eRuby のコストより大きいという事実には驚かされます。

この理由ですが、まず Erubis では正規表現を使って HTML エスケープ処理を行なっています。
そのため、正規表現の動作コストが文字列処理のコストよりずっと大きいということが言えると思います。
また Ruby 1.8.7 より Ruby 1.9.1 のほうが速度低下の割合が遅くなっているのは、Ruby 1.9.1 では正規表現ライブラリを高機能なものに変更したことが原因と思われます。

それでは、もし HTML エスケープ処理を C 言語で実装したらどのくらい速度が向上するでしょうか。

それを確かめるために、CGIExt を使ってみましょう。
[CGIExt](http://cgialt.rubyforge.org/) は cgi.rb での関数の一部を C 言語による拡張モジュールとして実装したライブラリであり、require するだけで HTML エスケープ用関数 (CGI.escapeHTML() や ERB::Util#h() や Erubis::XmlHelper.escape_xml()) を C 実装に置き換えます。
今のところ Ruby 1.8.x にしか対応してないので、Ruby 1.9.1 や Rubinius では使えないことに注意してください。

なお先ほどのベンチマークスクリプトに「-cgialt」オプションをつけると、CGIAlt を使ったベンチマークができます。

実行結果 (Mac OS X):

{% highlight text %}
{% raw %}
[macosx]$ ruby187 -s eruby-bench.rb -N=100000 -e -cgialt
      user     system      total        real
 26.750000   0.030000  26.780000 ( 26.852012)
{% endraw %}
{% endhighlight %}


実行結果 (Linux):

{% highlight text %}
{% raw %}
[linux]$ ruby187 -s eruby-bench.rb -N=100000 -e -cgialt
      user     system      total        real
 27.080000   0.000000  27.080000 ( 27.096098)
{% endraw %}
{% endhighlight %}


この結果を見ると、次のことがわかります。

* CGIExt を使えば、HTML エスケープ処理があった場合でもない場合と比べて約 15 % から 20 % の速度低下で済む。


先ほど、「アプリケーションの速度には処理系の速度より組み込みの関数やメソッドの速度のほうが重要」と書きました。
今回の結果はこの主張を裏付けるものになっています。
つまり、自分のアプリケーションで必要とする機能が組み込みや拡張モジュールとして提供されていれば、たとえ処理系の速度が遅くても、最終的なアプリケーション速度は速くなるわけです。

これは Java など静的な言語だけをやっていた人が勘違いしやすい点なので注意してください:
Ruby などスクリプト言語の処理系では、組み込み関数やメソッドは大抵 C/C++ 言語で実装されているため、それらを呼び出すだけならスクリプト言語でも十分高速です。
また必要なライブラリが C 言語による拡張モジュールで提供されている場合も同様に高速です。
そして、それらがどれだけ用意されているかでアプリケーションの速度は大きく変わります。
言葉を変えていえば、スクリプト言語では__組み込み関数やライブラリの品揃えでアプリケーション速度は大きく変わる__といえます。

余談になりますが、これは Web アプリケーションの分野において PHP が高速である理由のひとつでもあります。
各種言語でベンチマークをとると、PHP は Perl や Ruby より遅いです。
しかし実際に Web アプリケーションを使ってみると、PHP は他の言語より軽快に動作しているように感じます。
これは、HTML エスケープやリクエストパラメータ解析のような Web アプリケーションでよく使われる処理を、PHP では C 言語による組み込みの関数として提供していることが理由のひとつであると思われます。

## データベースを使ったベンチマーク

ベンチマークの最後として、データベース処理を含めたベンチマークを実行してみます。
内容はコメントつきの Blog アプリケーションであり、使用したデータベースとデータ件数は以下の通りです。

* MySQL バージョン: 5.0.67
* データ件数: ユーザ 10 人 + ブログエントリ 5000 件 + コメント 10000 件


ベンチマークスクリプトの詳細は[こちら](http://github.com/kwatch/shelves/tree/HEAD/rubyist-magazine/0025-appbench)にある blog-bench.* を参照してください。
今回は環境の都合により、Mac OS X 上で Ruby 1.8.7 と 1.9.1 を使ったときの結果のみ紹介します。

実行結果 (Mac OS X):

{% highlight text %}
{% raw %}
[macosx]$ ruby187 -s blog-bench.rb -N=500
      user     system      total        real
  3.440000   0.580000   4.020000 ( 31.501939)
[macosx]$ ruby191 -s blog-bench.rb -N=500
      user     system      total        real
  3.380000   0.580000   3.960000 ( 31.460440)
{% endraw %}
{% endhighlight %}


この結果から、次のようなことがわかります。

* Ruby による消費時間を比べると、Ruby 1.8.7 より 1.9.1 のほうが約 1 % ほど速い。
* 全体の実行時間のうち、Ruby による消費時間はわずか数パーセントにすぎない。


このベンチマークでは、Ruby 1.8.7 より 1.9.1 のほうが若干ではありますが高速でした。
先の eRuby ベンチマークでは 1.8.7 のほうが高速だったことを考えると、これはモデル操作の部分で Ruby 1.9.1 が逆転したと考えてよいでしょう。
つまり、MVC モデルにおける V は 1.8.7 のほうが速いが M は 1.9.1 のほうが速い、あるいは、__モデル操作が多い複雑なアプリケーションほど Ruby 1.9.1 のほうが速い__と言えます。

ただし、全体の時間が約 31.5 秒もかかっているのに、そのうち Ruby によって消費された時間はわずか 3.5 秒程度です。
このことから、全体の実行時間のうちほとんどはデータベースによって消費されていることがわかります[^1]。

この結果を見ると、言語の速度を競うのがいかにばからしいことかと思い知らされます。
もちろん言語の速度が必要なアプリケーションも数多くあるでしょう。
しかしデータベースを扱うようなアプリケーションであれば、いちばんのボトルネックはデータベース周りであり、そこを改善せずに言語まわりを改善したところで大して効果はありません。

そして、この傾向は__大規模システムであればあるほど顕著になる__ことに注意してください。
今回のベンチマークでは__わずか__ 1.5 万件程度のデータしか扱っていませんが、実際の大規模システムではこの 100 倍や 1000 倍あるのは珍しくありません。
そして、そのようにデータ量が大量になればなるほどボトルネックになるのはデータベースであり[^2]、言語処理系の速度はあまり問題にならなくなります[^3]。

ちなみに、このベンチマークで使ったテーブルと SQL にはボトルネックがあります。
該当箇所の create table 文と select 文は以下の通りですが、これらのうちどの部分がボトルネックになっているか、みなさんお分かりになるでしょうか。

create table 文:

{% highlight text %}
{% raw %}
create table blog_entries(
  id          integer         primary key auto_increment,
  user_id     integer         not null references blog_users(id),
  title       varchar(200)    not null,
  body        text(4000)      not null,
  updated_at  timestamp       not null,
  created_at  timestamp       not null,
  deleted_at  timestamp       null default null,
  index blog_entries_user_id(user_id)
);
{% endraw %}
{% endhighlight %}


select 文:

{% highlight text %}
{% raw %}
ENTRIES_SQL = <<END
select blog_entries.* from blog_entries, blog_users
where blog_entries.user_id = blog_users.id
  and blog_users.name = ?
order by blog_entries.id desc
limit 0, 20
END
{% endraw %}
{% endhighlight %}


さて、どこがボトルネックでしょうか。
みなさん考えてみてください。

答えは 30 秒後、CM のあとで!

　

　

----

◇◆◇ CM中 ◇◆◇

{% isbn_image('4873113946', '') %}
__『プログラミング言語 Ruby』__

David Flanagan, まつもと ゆきひろによる Ruby の解説書が出版されました。
Ruby の父が 10 年ぶりに書いた待望の書籍、絶賛発売中！

* David Flanagan, まつもと ゆきひろ 著、卜部 昌平 監訳、長尾 高弘 訳
* 2009 年 01 月 24 日 発売
* 472 ページ
* 定価 3,990 円
* ISBN: 978-4-87311-394-4
* 原書: The Ruby Programming Language


----

　

　

みなさん、わかりましたでしょうか。
筆者はデータベースが得意ではないのですが、調べた限りでは blog_entries テーブルにつけたインデックス (blog_entries_user_id) が原因でした。
このインデックスがついていると、select 文の結果をソートする際に PRIMARY インデックスが使われないため、遅くなるようです[^4]。
この問題の対策としては、次の 2 つがあります。

1. インデックスを削除する (alter table blog_entries drop index blog_entries_user_id)
1. PRIMARY インデックスを使うことを明示的する (select * from blog_entries use index (PRIMARY), ...)


今回は後者を試してみましょう。
ベンチマークスクリプトに -useindex オプションをつけると、「from blog_entries」を「from blog_entries use index (PRIMARY)」に変更してくれます。
筆者の環境で試してみると、31.5 秒かかっていたのが 12 秒にまで縮まりました。
ActiveRecord などの O/R マッパーでは use index のような構文をサポートしてないことを考えると、データベースに詳しい人ほど O/R マッパーを嫌がるのもわかります。

実行結果 (Mac OS X, -useindex オプションつき):

{% highlight text %}
{% raw %}
[macosx]$ ruby187 -s blog-bench.rb -N=500 -useindex
      user     system      total        real
  3.180000   0.420000   3.600000 ( 12.085999)
[macosx]$ ruby191 -s blog-bench.rb -N=500 -useindex
      user     system      total        real
  3.120000   0.410000   3.530000 ( 11.948881)
{% endraw %}
{% endhighlight %}


ただ、それでもまだデータベースがボトルネックになっていることにかわりはありません。
今回の場合ですと、Ruby の部分をチューニングしても最大で 3 秒程度しか縮まりませんが、SQL をチューニングすると簡単に約 20 秒も縮まり、またあと 12 秒ほどチューニングの余地があります。
つまり、Ruby をより速い言語に変えて 3 秒を縮めるよりも、テーブルを分割したり SQL の実行結果をキャッシュすることで 12 秒を縮めることを考えるべきだということがわかります。

今回の SQL はごく簡単だったので解決方法もわかりやすかったですが、実際のアプリケーションで使われる SQL はもっと複雑になるため、何がボトルネックでどう解決すればいいのかはそう簡単には分かりません。
ここでも、「大規模で複雑なシステムほど SQL のチューニングがより重要になるため、相対的に言語の速さは重要でなくなる」ことがわかります。

このように、データベースを使うアプリケーションではデータベース周りこそがボトルネックです。
そしてこの傾向は、大規模システムになるほど顕著になります。
Ruby が遅いと文句をいう前に、Ruby による部分が本当にボトルネックになっているかを確認してから、速い言語を求めましょう。
少なくとも、前述の SQL におけるボトルネックが分からなかったプログラマーや、MySQL を使っていながら use index を知らないようなプログラマーに、言語の速さがどうのこうのという資格はないでしょう[^5] 。

## 考察

ここまでお読みになっていただき、ありがとうございます。
せっかくの機会ですので、筆者の戯れ言にもうしばらくお付き合い下さい。

### 言語の速度は手段でしかない

冒頭部分の繰り返しになりますが、本稿での主張は「言語の速度がそのままアプリケーションの速度になるわけではない」あるいは「言語の速度とアプリケーションの速度は別モノ」ということです。

この主張には、ある重大な前提が隠されています。
それは、我々が最終的に必要としているのは「アプリケーションの速さ」であって「言語の速さ」ではないということです。
我々にとっては__「アプリケーションの速さ」こそが目的であり、「言語の速さ」はそのための手段でしかありません__[^6]。

勘違いしないでいただきたいのですが、言語処理系の性能に無頓着でいいと主張しているわけではありません。
言語の性能は大事です。ただ、同じくらい大事な (あるいはそれより大事な) 要因が他にも多くあるという、ごくごく当たり前のことを言っているまでです。

アプリケーションの速度には実にさまざまな要因が絡んできます。
それはデータベースだったり、アーキテクチャだったり、フレームワークやライブラリだったり、ネットワークや I/O の性能だったり、アルゴリズムやデータ構造だったり、プログラム上のアイデアだったりします。
そして言語処理系の速度というのは、いくつもある要因のうちの 1 つにすぎません。
「速い言語 + 遅いライブラリ」よりも「遅い言語 + 速いライブラリ」のほうが最終的なアプリケーションの動作は速い、なんてことはよくあることです。
しかしそのことに気づかず、言語処理系の速度ばかりが偏重される傾向にあるのは残念なことです。

筆者は、「1.9 になればアプリケーションが 3 倍速くなる!」とか、「1.8 とは違うのだよ、1.8 とは!」のように、Ruby 1.9 に対して過剰で能天気な期待をしている人の存在に危惧しています。
過剰な期待は過剰な失望を生み出しかねません。
Ruby 1.9 の高速化が世間で適切に評価されるためには、過剰な期待はマイナスにしかならないでしょう。
それよりも、Ruby が高速化された今こそ、言語の速度とアプリケーションの速度は別モノであると世の中に知らしめる好機ではないでしょうか。

### 個人的に Ruby 1.9 に期待すること

現在、Ruby がいちばんよく使われているのは Web アプリケーション関連ですが、それらは Ruby 1.9 にしたところで大して速くはならないと筆者は考えています。
また今回のベンチマーク結果はそれを裏付けています。

それでは Ruby 1.9 は要らない子なのでしょうか?

そんなことはありません。
筆者は、Ruby 1.9 は既存の Web アプリケーションを高速化するものではなく、Web アプリケーション以外での Ruby の利用を促してくれるもの、いわば __Ruby の可能性を広げるもの__だと考えています。

Ruby が Web アプリケーション分野でよく使われているということは、裏返していうと、今までの Ruby が遅かったせいで Web アプリケーション以外の分野ではあまり使われていなかった、と捉えることもできます。
文字列処理の多い Web アプリケーションでは Ruby でも問題ないでしょうが、そうではない分野では Ruby の遅さがネックになっていることもあるでしょう。
Ruby が高速化されたおかげで、そういった分野にも Ruby が進出できるようになります。

たとえば、現在の Web アプリケーションではデータエントリ (データの入力と出力) が中心ですが、今後 Ruby が本気でエンタープライズ分野に進出するのであれば、統計処理やビジネスインテリジェンスといった、データの集計を行なうアプリケーションも視野にいれるべきでしょう。
そして、こういったアプリケーションでは文字列処理よりも数値計算やループの性能が重要になるため、Ruby 1.8 ではつらかったとしても 1.9 なら適用できる可能性があります。

また Ruby 1.9 では [Fiber](http://doc.loveruby.net/refm/api/view/class/Fiber) が利用可能となっています。
これは Ruby で利用可能なアーキテクチャを大きく拡張するものであり、個人的に大変期待しています。
特に Fiber を使った非同期 I/O はデータベースまわりのボトルネックを大幅に解消できる可能性があり、実際に [NeverBlock](http://www.espace.com.eg/neverblock) というライブラリでは MySQL へのクエリーを非同期化することで[性能を大幅に高めている](http://www.espace.com.eg/neverblock/benchmarks)そうです。
聞いたところによると、ささだ先生としては Fiber をまだ積極的には使ってほしくないそうですが、データベース周りが高速化できると聞かされれば期待するなというほうが無理でしょう。
少なくとも今の筆者は過剰なくらい期待しています :)

## おわりに

本稿では Web アプリケーションを想定したベンチマークをとることで、「言語の速度がそのままアプリケーションの速度になるわけではない」あるいは「言語の速さとアプリケーションの速さは別モノである」ことを説明しました。

また各ベンチマーク結果から以下のことがわかりました。

* MVC モデルにおける Model の部分が複雑で操作が多いほど Ruby 1.9 のほうが速い。
  * 文字列処理が多い View の部分は Ruby 1.8 のほうが速い。
* 組み込み関数やメソッドの品揃えは重要。拡張モジュールも大変有効。
  * たとえば eRuby による HTML 生成よりも HTML エスケープ処理のほうが重いので、ここを拡張モジュールにすると効果が高い。
* データベースを使うとそこが大きなボトルネックになるため、言語の速度は大きな問題にならない。
  * (筆者を含め) SQL が苦手なプログラマーに速度を語る資格なし。
  * 新しいアーキテクチャでデータベース周りが高速化できるといいなあ。


本稿ではアプリケーションのボトルネックとしてデータベースを挙げましたが、Web アプリケーションではフレームワークやライブラリも大きなボトルネックになります。
たとえば今回のベンチマークでは HTTP パラメータの解析やセッション処理などを含んでいません。
本来であればこれらも含めてベンチマークを実施することが望ましいので、機会があれば紹介したいと思います。

なお当然ではありますが、ベンチマーク結果は環境や使用するデータに大きく影響されます。
本稿の結果を鵜呑みにすることなく、自分の手と目で確かめるようにしてください。

本稿が、Ruby 1.9 への過剰な期待を薄めてくれるきっかけになることを願っています。

(編集：くげ)

----

[^1]: これには HTTP パラメータの解析など Web アプリケーションで必要ないくつかの処理が省略されていることに注意してください。実際の Web アプリケーションではこの割合も変わります。ただし、データベースがボトルネックであることは変わらないでしょう。
[^2]: またデータベースサーバはスケールアウトしにくいことも、大規模システムでデータベースがボトルネックになる理由のひとつです。
[^3]: ところが世の中の認識はこれとは逆で、大規模システムほど速い言語が必要という人ばかりです。そのくせ、そういう人ほど SQL のチューニングや O/R マッパーの速度には無頓着だったりします。なんとも不思議な現象です。
[^4]: これは MySQL のバージョンに依存する話であり、今回使用した MySQL 5.0.67 ではこのような結果になりましたが、MySQL 5.1 では blog_entries_user_id インデックスを削除しなくても select 文において PRIMARY インデックスが使われました。また MySQL 5.0.27 では逆に PRIMARY インデックスすらも使われなくなったためさらに遅くなりました。
[^5]: まさに筆者のことです。
[^6]: ただし世の中には「言語の速さ」自体が目的である場合もあるので、その場合はこの限りではありません。
