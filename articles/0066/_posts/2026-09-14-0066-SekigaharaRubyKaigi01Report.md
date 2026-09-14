---
layout: post
title: RegionalRubyKaigi レポート (NUM) 関ケ原 Ruby 会議 01
short_title: RegionalRubyKaigi レポート (NUM) 関ケ原 Ruby 会議 01
tags: 0066 SekigaharaRubyKaigi01Report regionalRubyKaigi
post_author: うさみ
created_on: 2026-09-14
---
{% include base.html %}

## はじめに

時に令和八年<ruby><rb>皐月</rb><rp>(</rp><rt>さつき</rt><rp>)</rp></ruby>三十日、日辰は<ruby><rb>甲辰</rb><rp>(</rp><rt>きのえたつ</rt><rp>)</rp></ruby>。  
天下分け目の古戦場たる<ruby><rb>濃州</rb><rp>(</rp><rt>のうしゅう</rt><rp>)</rp></ruby>関ケ原の地において、『関ケ原 Ruby 会議零一』の陣ぞ敷かれける。  
某、此度の戦に馳せ参じたる顛末をば、<ruby><rb>茲</rb><rp>(</rp><rt>ここ</rt><rp>)</rp></ruby>に<ruby><rb>言上</rb><rp>(</rp><rt>ごんじょう</rt><rp>)</rp></ruby><ruby><rb>仕</rb><rp>(</rp><rt>つかまつ</rt><rp>)</rp></ruby>らん。

これより先は、令和の世の言葉にてお目通し願うこと、平にご容赦願い奉る。

*(2026 年 5 月 30 日土曜日に岐阜県関ケ原町で開催された『関ケ原 Ruby 会議 01』のレポートをお届けいたします。以後は現代語で失礼いたします)*

### 関ケ原 Ruby 会議 01 とは

[関ケ原 Ruby 会議 01](https://regional.rubykaigi.org/sekigahara01/) 公式サイトでは以下のように説明されています。

> **天下分け目の地域 Ruby 会議に候**
>
> 関ケ原 Ruby 会議 01 は、天下分け目の地「関ケ原」に本陣を構え、Ruby を語らう諸将参集の場に候。
>
> いにしえの兵どもが夢の跡にて、いざ新たなる Ruby の軍記を刻まん。

地域 Ruby 会議は各地の Ruby コミュニティ主導で開催されることが基本ですが、このイベントは関西 Ruby 会議の ydah さん、東京 Ruby 会議 12 の osyoyu さん、nagara.rb の corocn さんらによる<ruby><rb>奉行衆</rb><rp>(</rp><rt>オーガナイザー</rt><rp>)</rp></ruby>によって企画されました。

> 関西 Ruby 会議 08 のオフィシャルパーティにて、osyoyu さんと話をしていたときのことでした。「関西と関東の地域 Ruby コミュニティ間の交流ができる場があったら面白いよね。というか東西戦やろう！！」という話になりました。
>
> *([関ケ原 Ruby 会議 01 の開催に寄せて](https://note.com/sekigahara01/n/nb2a18bc713eb) より引用)*

そのほかオーガナイザー陣の思いは、インタビューでも語られています。

 - [関ケ原 Ruby 会議 01 運営インタビュー ── 【前編】 Ruby の衆、何ゆえ“天下分け目の地”へ馳せ参ずるや - SmartHR Tech Blog](https://tech.smarthr.jp/entry/2026/05/14/153000)
 - [関ケ原 Ruby 会議 01 運営インタビュー ── 【後編】 RubyKaja、東西の武将、合戦、そして宴へ - SmartHR Tech Blog](https://tech.smarthr.jp/entry/2026/05/21/153000)

Ruby コミッタの笹田耕一さんと前田修吾さんを東西両軍の大将と戴き、発表者募集の段階から東軍と西軍のどちらに属するかを決める仕組みになっていました。

筆者は最近 Ruby を書いていなかったのですが、昨年の『[北陸 Ruby 会議 01](/articles/0066/0066-HokurikuRubyKaigi01Report.html)』の場でイベントのコンセプトを聞いて、直ちに参加を決めました。

### 開催概要

 - 開催日： <time datetime="2026-05-30">2026 年 5 月 30 日 (土)</time>
 - 会場： 岐阜県不破郡関ケ原町 / 関ケ原ふれあいセンター
 - 公式タグ： `#sekigahara01`

## 合戦本陣

### 諸将参陣

当日の関ケ原は快晴でした。関ケ原ふれあいセンターは絶好の合戦日和です ⚔️

参陣した参加者 (大名・家臣) は受付で東軍・西軍・中央のいずれかの名札を選択して記名する仕組みになっていました。東西

![ホワイトボードに貼られた日本地図に「お主、いずこから参られた？」「付札に御名をしたため、張り付けてくだされ」と書かれています]({{base}}{{site.baseurl}}/images/0066-SekigaharaRubyKaigi01Report/map.jpg){:width="600px"}

会場には日本地図が掲示されており、

### 本陣 開戦の儀 (オープニング)

オープニングでは奉行によって会についての案内があったあと、両軍の武将 (発表者) が入場しました。

![東軍と西軍の武将たちが対峙して、刀を構えて一触即発の状態です]({{base}}{{site.baseurl}}/images/0066-SekigaharaRubyKaigi01Report/busho.jpg){:width="600px"}

両軍の武将が対峙した後は Ruby 作者のまつもとゆきひろさん (Matz) からの開戦の儀がありました。

![まつもとゆきひろさん (Matz) が拳を突き上げて、背景に「開戦」の文字が映し出されています]({{base}}{{site.baseurl}}/images/0066-SekigaharaRubyKaigi01Report/matz_kaisen.jpg){:width="600px"}

### [東軍 先鋒] Sorbet の型が Rails の MVC 全てを貫通するまで

kazzix14

### [西軍 先鋒] 拙者、『型は欲しいが型は書きたくない』者たちとの和睦を結び、るびぃにおける型の領地安堵を実現せんと欲す者也

森塚三矢大阪守真年

### [東軍 次鋒] 気づいたら Ruby で 100 作品 ー クリエイティブコーディングが生活の一部になるまで

chobishiba

### [西軍 次鋒] Termfront: Ruby 標準ライブラリだけで作る FPS

S.H.

### ピクニック

![青空の下、前田修吾さんが演説をしている横で東軍大将の笹田耕一さん、奉行衆の osyoyu さんを始めとする多くの兵どもが弁当を食べています]({{base}}{{site.baseurl}}/images/0066-SekigaharaRubyKaigi01Report/picnic.jpg){:width="600px"}

### スポンサー LT

### RubyKaja

### [東軍 中堅] Play Music on Ruby ── PicoRuby で作る MIDI オーケストレーションツール

Toshio Maki

### [西軍 中堅] New "Type" system on PicoRuby

Masataka Pocke Kuwabara

### [東軍 副将] Job 戦国時代

kinoppyd

### [西軍 副将] PicoRuby に於ける Refinements の再解釈

hasumikin

### 合戦

