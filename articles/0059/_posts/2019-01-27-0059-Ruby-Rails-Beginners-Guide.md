---
layout: post
title: RubyとRailsの学習ガイド2019年版
short_title: RubyとRailsの学習ガイド2019年版
tags: 0059
post_author: igaiga
created_on: 2019/01/27
updated_on: 2019/09/02
---
{% include base.html %}

## この記事は
RubyそしてRailsをこれから勉強したい方に、どんな技術を勉強すればいいかと、それらの技術全体のガイドマップを図示します。そしてそれを学ぶための資料（書籍、Web記事ほか）を紹介していきます。この記事は、頭の中に技術全体の地図を描き、イメージしてもらうのが狙いです。

Railsアプリを作るときに必要になたくさんの技術について説明していきますが、本当にたくさんの技術が出てきます。まだ学んでいない、分からない言葉が出てくると思いますが、全体を把握するために、ひとまずは「そういう技術があるのだな」くらいで捉えてもらえればと思います。将来、その言葉が出てきたときに「どこかで聞いたような？」と思えたら儲けものです。

勉強方法のお勧めは、1つの知識を徹底的にやるよりも、まずは全体を通して勉強し、そのあとで勉強したいところに戻って積み重ねて学んでいく方が、挫折しづらいのでお勧めです。

追記(2021年7月13日): 本稿公開後、加筆改訂版を2019年4月の技術書典6にて出版し、その後、改訂版を2021年7月の技術書典11で出版しました。技術全体図をイラストで描き、トピックを増やし、本稿公開後にいただいたフィードバックや追調査した結果で本記事の内容も改訂しています。2021年改訂版は [BOOTH](https://igaigarb.booth.pm/items/3089926) にて電子書籍版を購入できます。

## Rails技術全体図

Railsアプリで使う知識を図示すると次のようになります。

![ruby_rails_guide_map.png]({{base}}{{site.baseurl}}/images/0059-Ruby-Rails-Beginners-Guide/ruby_rails_guide_map.png)

こうして見ると、Railsアプリ（Webアプリ）というものはたくさんの技術を積み重ねてできていることが分かります。それぞれについて順に見ていきましょう。

## 筆者について
この記事は「Ruby超入門」と「Railsの教科書」の著者である五十嵐が書いています。（なので、その2つについての記述が厚くなっていると思います。）RubyはtDiaryという日記システムで使い始めて約15年、自分でプログラムを書くようになって約12年、Railsを書き始めて約9年になります。大学の非常勤講師、企業の新人研修、また島根県Ruby合宿、Rails寺子屋、RailsGirlsといった場所で講師を約7年やっています。現在はフリーランスでRailsアプリ開発の仕事をしています。

## Ruby
Rubyはプログラミング言語です。Railsから始めた人は意外に思うかもしれませんが、Rubyだけでもプログラムを書くことができます。執筆時2019年1月10日現在のRuby最新バージョンはRuby2.6.0です。

Rubyを学ぶ方法を、レベル感を「初めてのプログラミングをRubyで学ぶ」「（他言語を学んでいた方が）Rubyを学ぶ」「Rubyの技術を伸ばす」の3つに分けて説明していきます。

### プログラミングをRubyで始める

これからプログラミングを学ぶという方へRubyをお勧めする点として、プログラムが短く読みやすいこと、いわゆる「おまじない」と呼ばれる説明が困難な事柄が学習の過程であまり出てこないことなどがあります。

拙著 [ゼロからわかるRuby超入門](https://gihyo.jp/book/2018/978-4-297-10123-7) を紹介させてください。この本はプログラミングが初めての方へ向けた、Rubyプログラミングの入門書です。キーボードでプログラミングでしか使わないキーの位置の説明から始まり、プログラミングの基礎部品である条件分岐、繰り返しの概念や、整数、文字列、配列、ハッシュと言ったよく使うオブジェクトの仕組みを説明しています。また、エラー時の対処方法、リファレンスマニュアルの調べ方、かんたんなデバッグ方法も学べるので、自分で調べる力をつけることができます。Railsで必要となる知識を学ぶことをゴールにしているので、メソッドやクラス、モジュールの作り方、使い方など、最低限必要となる知識を効率良く学べます。また、ライブラリ（公開されている便利な共有部品）であるGemやBundlerの使い方を学べます。応用としてかんたんに扱えるWebフレームワークSinatraでWebサーバを作り、Ruby標準添付ライブラリであるNet::HTTPを使ってそれにアクセスするHTTPの基礎を動かして学ぶことができます。HTTPについては後述します。

Ruby超入門の共著者である松岡さんがこの本で学習するRubyの知識について地図のように図示しているのが [こちら](https://speakerdeck.com/machu/zerokarawakarurubychao-ru-men-falsebu-kifang?slide=7) です。1冊の本で学ぶ知識を分類して1枚の図で示すとこのようになります。（今、書いていて気づいたのですが、これを書籍の前書きに載せたら分かりやすかったですね…。）

![machu.png]({{base}}{{site.baseurl}}/images/0059-Ruby-Rails-Beginners-Guide/machu.png)

このレベルの他の本としては [3ステップでしっかり学ぶRuby入門](https://gihyo.jp/book/2018/978-4-7741-9502-5) があります。Windowsのコマンドプロンプトでの実行と結果のスクリーンショットが載っているので、照らし合わせながら実行できます。

### （他言語を学んでいた方が）Rubyを学ぶ

前述の初めてプログラミングを学ぶ本は丁寧な説明にページを割くため、知識を選んで最短距離で学ぶことになります。一方で、他の言語で基礎知識を既に持っている人にとっては、網羅的に知識を学びたい、Rubyならではの実践的な技術知識を学びたいといった要求が出ます。

網羅的な知識を学ぶ本として、 [たのしいRuby](https://www.sbcr.jp/products/4797386295.html) があります。Rubyプログラミングに必要な知識を網羅的に説明していて、この本でカバーしていないRuby知識を探すのが難しいほどです。15年以上に渡って改訂されて読み継がれている名著です。

もう1冊、網羅的に解説している本に [かんたんRuby](https://gihyo.jp/book/2018/978-4-7741-9861-3) があります。こちらは網羅的なRubyの知識の説明と、分かりやすい説明を両立させた本です。

3冊目は実践技術を学ぶ本、[プロを目指す人のためのRuby入門](https://gihyo.jp/book/2017/978-4-7741-9397-7) （通称：チェリー本）です。「いくつかの書き方があるが、このケースではこう書くと良い」という著者のお勧めに沿ってRubyでの自然な書き方を学べます。また、後述するテストの書き方、デバッグ方法の説明も手厚く書いてあります。読者に寄り添った文体で人気の入門書です。

もう1冊、少し違うアプローチを採用している [Ruby逆引きハンドブック](http://www.c-r.com/book/detail/1266) 。見出しが「文字列を大文字・小文字変換する」「配列を反転する」といった形で並び、やりたいことからそれに対応するコードを調べることができます。これで調べてそれらをつなげることで自分の書きたいプログラムを組み立てることができます。1つの機能に対していくつかのサンプルプログラムで説明されるので、プログラムも短くなり、読みやすく理解しやすいです。私がRubyを勉強したスタイルでもあり、お勧めの方法です。

### Rubyの技術を伸ばす

Rubyの基礎を網羅的に学んだ後のステップとして、発展的な技術を身につけるための書籍を2つ紹介します。

1冊目は [パーフェクトRuby](https://gihyo.jp/book/2017/978-4-7741-8977-2) 。Rubyの一歩進んだ使い方がたくさん書かれています。特にgemの作り方、公開方法については多くのページが割かれて丁寧に説明があります。実践的に使う応用的な技がたくさん書かれています。

2冊目は [メタプログラミングRuby](https://www.oreilly.co.jp/books/9784873117430/) 。メタプログラミングとは「プログラミングコードを記述するコードを記述すること」で、その際に有用な数々の技を、著者パオロさんのユーモアあふれる解説で学べます。それ以外にも、Rubyの仕組みについても多く解説されていて、同名メソッドがあった場合の呼び出されるメソッドの決定方法やクラスメソッドの実現方法などが書かれています。私はこの本を読んで、特にブロックの見方が変わり、Rubyプログラムの読み方が一変したことを今でも鮮明に覚えています。

## Rails
RailsはWebアプリ（ブラウザからアクセスするアプリ）をかんたんに作るための様々な便利な道具を集めたフレームワークです。また、Railsアプリ中ではRubyのプログラムを書いていきますが、その際にRailsが用意するさまざまな便利な道具を使うことができます。そのためRubyとRailsの知識の両方を使って書いていくことになります。Rubyとして学んだ知識もここで使うことができます。

執筆時2019年1月10日現在のRails最新バージョンはRails5.2.2です。今年の4月末にRails6.0.0が出る予定だと公表されています。

### 環境構築
Rubyの場合と比べて、RailsではたくさんのGem（特にCで書かれていてインストール時にコンパイルが必要なNative extension）を使うため、環境構築がRubyだけで使うよりも難しいという問題があります。

macOSでRailsを使うときにはrbenv、Command line tools、homebrewを使ってRubyをインストールする方法がお勧めです。Rails Girls ガイドの [インストール](http://railsgirls.jp/install#setup_for_macos) のページが分かりやすく、また最新のインストール方法で更新されているのでお勧めです。

WindowsでRailsを使うときには、WSL(Windows Subsystem for Linux)を使ってUbuntu(Linux OS)をセットアップし、その上で開発するのがお勧めです。後述する書籍「現場Rails」に手順が説明されています。また、Rails Girls ガイドの [インストール](http://railsgirls.jp/install#setup_for_windows) のページにも2019年1月中に記載予定で作業中です。別の選択肢としては、[RubyInstaller](https://rubyinstaller.org/) を使ったセットアップがあり、Rails Girls ガイドや書籍「Ruby超入門」に手順が載っています。こちらの方がインストーラーだけでセットアップできるのでインストールは簡単です。しかし、RailsアプリをAWS(後述)などで運用しようとすると、利用するOSはほぼLinuxになります。手元の環境もLinux(WSL)やmacOS(BSDと呼ばれるUnixの1種)にしておいた方がスムースです。

### Railsでまずは簡単なアプリを作る

まずはRailsでの開発を体験してみたいときには、簡単なアプリ作りに挑戦してみましょう。資料としてRailsGirlsガイドの [はじめてのRailsアプリ](http://railsgirls.jp/app) がお勧めです。写真を投稿するアプリづくりの手順が丁寧に書かれています。半日から1日で手軽にRailsアプリづくりを体験できます。

この資料では作ったRailsアプリがどうやって動いているかについての説明はあまりありません。この部分の説明については拙著 [Railsの教科書](https://tatsu-zine.com/books/rails-textbook) で図解しています。Railsの教科書ではRailsアプリを最小単位で作り、それがどのような仕組みで動いているかを説明しています。バージョンはRails5.1対応です。（Rails6.0が出たところで最新版に合わせて改訂の予定です。）

### Railsの基礎技術を学ぶ

Railsの教科書ではごく限定した範囲の説明だけにとどまっています。Railsの基礎知識を広く学ぶには、 [現場で使える Ruby on Rails 5速習実践ガイド](https://book.mynavi.jp/ec/products/detail/id=93905) 、通称「現場Rails」がお勧めです。Railsの基礎を網羅的に説明しています。Railsの受託開発や開発支援を長年つづけている株式会社万葉のみなさんが執筆されているので、タイトル通り現場で使える技術に即した内容になっています。Rails5.2対応。

また、定番のWeb資料として、 [Railsガイド](https://railsguides.jp/) と [Railsチュートリアル](https://railstutorial.jp/) があります。

Railsガイドがリファレンス的にRailsのほぼ全ての範囲の機能について説明が書かれています。時間はかかりますが、一通り読んでおくことでRailsアプリ開発に使える道具を広く身につけることができるでしょう。また、RailsガイドはRailsの開発とセットで書かれているため、最新バージョンのRailsに対応した記事をいつも読むことができます。たまに読み返すと、新機能が追記されていて「こんな書き方もできるのか」と勉強になります。

Railsチュートリアルはアプリづくりを通じてRailsアプリ開発を学んでいきます。分からないことが出てきたときは、前述のRailsガイドや現場Rails、またはRailsの教科書などを活用して調べながら進められるようになれば、実際の開発時の進め方の学びにもなります。

### Railsの技術を伸ばす

Railsの技術を伸ばしていくときにお勧めなのが [パーフェクト Ruby on Rails](https://gihyo.jp/book/2014/978-4-7741-6516-5) です。Rails4.1対応と少し古くなってきましたが、今でも役立つ内容がたくさん書かれています。Rack Middleware、Railtie、Engineといった、よりRailsを使いこなす内容も説明されています。

また、英語で書かれた書籍ですが、 [Crafting Rails 4 Applications](https://pragprog.com/book/jvrails2/crafting-rails-4-applications) はRailsの知識をより深めるのにとても良い本です。Rails4.0対応と少し古くなりましたが、Railsについて一歩深く踏み込む方法を学ぶ意味でもお勧めです。

### HTTP

rails new してRailsアプリを作り、rails serverを起動しただけで既にWebアプリとして動作します。

rails serverを起動するとき、Pumaという名の「Webサーバ」プログラムを利用します。Webサーバは、Webアプリを動作させるための基礎部分を担当し、さまざまなWebアプリで共通で使うことで、毎回同じプログラムを書く必要なく、Webアプリを作ることができます。

Webで利用されている通信プロトコル（取り決められているルール）にHTTPがあります。HTTPほかWebに関する技術は [Webを支える技術](http://gihyo.jp/magazine/wdpress/plus/978-4-7741-4204-3) がHTTPプロトコルだけでなくRESTなどの技術を網羅的に説明する良書です。

また、WebではHTTPS通信などで暗号技術を多く利用しています。この分野は [暗号技術入門](http://www.hyuki.com/cr/) に秘密鍵暗号、公開鍵暗号、暗号アルゴリズム、証明書、一方向ハッシュ関数など、一通りの技術が丁寧に説明されていて、初学者にも読みやすく書かれていてお勧めです。

### OS

rails serverを起動するときにはshell環境を使います。ターミナルやコンソール（Windowsではコマンドプロンプト）の中の世界です。「黒い画面」と呼ばれたりもします。Linux/Unix OSへ指示をするためのshellという技術を学んでおくと、マシンを操ることができます。shellについては [Webデザイナーの為の「本当は怖くない」“黒い画面”入門](http://fjord.jp/articles/548.html) が必要な内容を厳選して分かりやすく解説しています。

Unix OSの知識を深めるための本として [なるほどUNIXプロセス](https://tatsu-zine.com/books/naruhounix) があります。プロセス、シグナル、システムコールといったUnix OSの基礎となっている技術を、Rubyプログラムを例文として説明しています。また、Pumaと並んで使われているWebサーバプログラムunicornを例にしたネットワークプログラミングの分かりやすい解説があります。RubyのプログラムがOSの中でどのように動いているかを理解する基礎となる知識を得られます。

Railsプロセス（動いているプログラム）をOSで管理するときには、プロセスをコンテナとして便利に扱えるDocker、そしてそれを束ねて使うKubernetesといった技術を使うことも最近は実戦投入されつつあります。Dockerでプロセスを扱うようにしておくと、UnixのほかMacやWindows上でも同様に起動できるため、開発環境構築にDockerを使う機会もあるかもしれません。Dockerについては [マンガでわかるDocker](https://booth.pm/ja/items/825879) が概念と使い方を分かりやすく説明しています。

### HTML/CSS

HTTPでブラウザとWebアプリでやりとりするときに、ブラウザで表示するページを記述するのにはHTMLとCSSと呼ばれる言語を使います。

HTMLとCSSを学習する資料としては、 [サルワカのWebデザイン入門](https://saruwakakun.com/html-css/basic) や、少し古めのものですが [ごく簡単なHTMLの説明](https://www.kanzaki.com/docs/htminfo.html) などがあります。

ここから先はデザインの領域へ続いていますので、この方向へ進みたい方はデザイナーさん向けの育成ルートを探してみるのも良いかもしれません。

### DB

Railsそれ自身はたくさんの部品でできています。主要な部品の1つはデータを保存するDBとのやりとりです。DBではSQLという言語を使ってデータをやりとりします。RailsではこれをActiveRecordという部品を使って、Rubyで扱い易い形でDBとやりとりすることができます。そこで、（Railsを使わずに）SQLを使ってDBでどのようなことができるかを学んでおくと、「SQLでできたこれはActiveRecordではどう書くのだろう？」と調べるきっかけを作ることができます。SQLについての勉強には [SQL書き方ドリル](https://gihyo.jp/book/2016/978-4-7741-8066-3) がお勧めです。加えて、インデックスやトランザクション、正規化といったDBで使う技術を説明した入門書として [SQLはじめの一歩](http://gihyo.jp/magazine/wdpress/plus/978-4-7741-7182-1) をお勧めします。

また、基礎的な関連やポリモフィック関連、STIなどRailsで使える技術については、Railsガイドの [関連付け](https://railsguides.jp/association_basics.html) の記事に基礎的なことがまとめられ、分かりやすく説明されているのでお勧めです。

### Git, GitHub

RubyコミュニティではGitと呼ばれるソース管理システムを使って書いたプログラムを管理することが一般的です。Gitを使うとプログラムを昔のある時点まで遡ったり、他の人が書いたプログラムを自分の手元に取得することができます。Gitを便利に扱うプラットホームとしてGitHubも、コードレビューやチーム開発によく利用されています。 [わかばちゃんと学ぶ Git使い方入門](http://www.c-r.com/book/detail/1108) はマンガと分かりやすい説明を組み合わせてGitとGitHubで必要な知識と使い方を説明しています。

### AWS

作ったRailsアプリを実行するために、AWS(Amazon Web Services)やGCP(Google Cloud Platform)といったクラウドサービスがよく使われます。自分でサーバマシンを用意する必要がなく、時間課金で多種のサーバを利用できるサービスです。AWSについては[AWSをはじめよう](https://booth.pm/ja/items/1032590)がゼロから丁寧に説明する良書です。希望するドメイン（URL）を自分で取得するときは姉妹書である[DNSをはじめよう](https://mochikoastech.booth.pm/items/812516)に手順が丁寧に書かれています。

また、アプリケーションをGitで配置するだけで動くHerokuなどのPaaSと呼ばれるサービスもあり、より手軽に使うのにはこちらが便利です。資料としては[RailsGirlsガイド](http://railsgirls.jp/heroku)が参考になります。

また、ソースコードをこれらのサーバへ配置することをデプロイと言います。デプロイにはCapistranoというツールがよく使われます。

そして、AWSなどのクラウドサービス上のサーバマシンのセットアップ作業もプログラムで書くことができます。ここではItamaeなどのツールがよく使われているようです。

### デバッグ

意図通り動かないところをみつけて直す作業はプログラミングで大きな比重を占める作業です。

基本となるエラーメッセージの読み方はたとえば [ゼロからわかるRuby超入門](https://gihyo.jp/book/2018/978-4-297-10123-7) でも説明しています。エラーが出ると悲しくなりますが、エラーメッセージはそんな私たちを励ますRubyから私たちへの思いやりあふれるヒント集です。

[プロを目指す人のためのRuby入門](https://gihyo.jp/book/2017/978-4-7741-9397-7) は1章を割いてエラーメッセージの読み方、エラーの種類、バックトレースの読み方、プログラムの途中の状態を把握する方法、デバッガbyebugを使ってプログラムを一時停止しステップ実行を行う手などが説明されています。

[Ruby逆引きハンドブック](http://www.c-r.com/book/detail/1266) にはデバッガbyebugの使い方やプロファイラstackprofを使って遅い場所を探す方法など便利な技法が説明されています。

また、Ruby2.4からはgemを使わなくてもirbを使ってプログラムを一時停止することができます。次のプログラムを一時停止したい場所に書きます。（Ruby2.4では `require "irb"` を `binding.irb` の前に書く必要があります。）

```ruby
binding.irb
```


### テスト

これから書くプログラムが意図通り動くかどうか確かめるための道具がテストです。テスト自身もプログラムで書くことができるので、テストを書けば実行はコンピュータがやってくれます。

前述の [チェリー本](https://gihyo.jp/book/2017/978-4-7741-9397-7) では1章を割いてテストについて 説明しています。FizzBuzzプログラムに対してテストを書くことから説明しているので、易しいところからテストを学ぶことができます。テストフレームワークにはMinitestを利用しています。

同じくMinitestを使った資料として、Railsガイドにも [テスト](https://railsguides.jp/testing.html) について書いたページがあります。Railsの各部品のテストについて丁寧に説明があります。

また、Railsのテストフレームワークとしてよく使われているRspecについては [Everyday Rails - RSpecによるRailsテスト入門](https://leanpub.com/everydayrailsrspec-jp) に網羅的にまとまっています。目次を見ると分かるように、Railsの各部品ごとに書くことができる各種テストを説明しています。

### セキュリティ

世の中には脆弱性（セキュリティに不備がある状態）があると、サービス運用不可能になるだけでなく、データ改ざんや情報漏洩につながります。Railsで注意すべきセキュリティのポイントについて、Railsガイドの [セキュリティ](https://railsguides.jp/security.html) のページ にまとまっています。必読のページです。

### 触れなかった内容

本記事では書籍、Webを中心に参考資料を紹介したため、Web教材、動画資料は触れられていません。現在では多くの方に利用されているものなので、それらについても調べて加筆できるといいなと思っています。（4月にある技術書典6で加筆改訂版を出せたらと考えていますが、詳細は未定です。）

本記事では触れなかった技術を、概要だけ1行で紹介します。

- asset pipeline: javascriptやCSSを上手に配信するための一連の仕組み
- CDN(Content Delivery Network): Web配信を高速化する仕組み
- OpenAPI, Swagger: APIを作るときにやり取りする仕様を定義する仕組み
- IDE: エディタと付随する開発支援環境。RubyにはRubyMineという有償のIDEがあり、コード入力、コードリーディングやデバッグを支援してくれます。

追記(2019年8月29日): 2019年4月に行われた技術書典6で加筆改訂版を出すことができました。電子書籍版を [BOOTH 「RubyとRailsの学習ガイド 2019 技術書典6 拡大版」](https://igaigarb.booth.pm/items/1312664) から購入可能です。技術全体図をイラストで描き、「OpenAPI, Swagger」、「IDE」について加筆したほか、本稿の内容も公開後にいただいたフィードバックや追調査した結果で更新しています。

### 参考文献

本文章は以下の記事を参考にしています。

- [ゼロからわかるRuby超入門の歩き方](https://speakerdeck.com/machu/zerokarawakarurubychao-ru-men-falsebu-kifang)
- [勉強してる技術の位置付け](http://docs.komagata.org/5533)
- [未経験からRuby on Railsを学んで仕事につなげるまでの1000時間メニュー](https://qiita.com/saboyutaka/items/1a8c40e105e93ac6856a)

また、上記の記事を書いている、「ゼロからわかるRuby超入門」共著者の松岡さん、プログラマー学習・就職支援  [フィヨルドブートキャンプ](https://bootcamp.fjord.jp/) を運営している駒形さんそして町田さん、沖縄でプログラミング教室 [CODE BASE](https://www.protosolution.co.jp/codebase/program-school/index.html) の講師をされているsaboyutakaさんには本記事のレビューもしていただきました。ありがとうございました。

## 筆者紹介

- 五十嵐邦明
- twitter: [@igaiga555](https://twitter.com/igaiga555)

フリーランスでRailsアプリ開発を行っている。大学の非常勤講師、企業の新人研修、また島根県Ruby合宿、Rails寺子屋、RailsGirlsなどで講師を務める。著書に [ゼロからわかるRuby超入門](https://gihyo.jp/book/2018/978-4-297-10123-7) 、 [Railsの教科書](https://tatsu-zine.com/books/rails-textbook)。
