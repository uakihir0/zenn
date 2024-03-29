---
title: "Bluesky はエンジニアにとって最高の遊び場"
emoji: "⛲"
type: "idea"
topics: ["sns", "Bluesky"]
published: true
---

# はじめに

**みなさん、最近遊び場に飢えていませんか？** Twitter API も有料化され、遊び場としての敷居が非常に高くなってしまった現在、気軽に色々なアイデアを実現することができる遊び場に飢えているエンジニアもきっと多いんじゃないかと思います。そんなみなさんに朗報です。 **Bluesky、最高にエンジニアリングの遊び場として楽しいですよ？ みなさんもここで何かアイデアを形にしてみませんか？** というお誘いです。

# Bluesky とは？

Bluesky とは 今流行りの分散型 SNS の一つで、Mastodon の親戚のようなものです。Twitter 創業者 ジャック・ドーシー氏が出資しているということでも有名です。分散型 SNS というのがどういうものか？ については今のところ分散するシステム自体が完成していないので、遊び場としての性質に関係はなく、他の方の解説を見ていただきたいのですが、 **今の Bluesky を簡単に言えば「絶賛立ち上がっているオープンな SNS」** です。一週間前 (2023/5/30) にユーザー数が 10 万人を突破し、招待制ではありますが、徐々に一般に浸透しつつあります。

# Bluesky が何故遊び場としていいのか？

Bluesky が何故エンジニアの遊び場としていいのか、についてですが簡単に言えば以下の二点があります。個々にどういうことかを説明していきます。

- **技術がオープンでユーザー誰にでも門戸が開かれていること**
- **ユーザーに制作物を触ってもらえる環境にあること**

## 技術がオープンでユーザー誰にでも門戸が開かれていること

Bluesky は、ATProtocol というプロトコルで実現可能なアプリケーションの例として実装されているという側面があります。 ATProtocol は誤解を恐れずにざっくり表現すると、ユーザーとデータの持ち方と、その共有の方法を定義したもので、Lexicon というドキュメント等で定義され公開されています。

https://atproto.com/docs

そのため、ATProtocol、その上に実装されている Bluesky の仕様もすぐに確認することができます。 (上記のドキュメントはちょっと反映が遅いので、最新のを追いたい場合は、GitHub の [bluesky-social/atproto](https://github.com/bluesky-social/atproto) を追いかけるといいと思います) なので、 **Bluesky 上で何かを作りたいと思ったら、それを実現可能な資料はそこに一通り揃っています。** また、Bluesky の[公式 Web クライアント](https://bsky.app/)もその仕様を元に作成されているので、どう実装していいか分からない場合、公式 Web クライアントから開発者ツールを開いて通信を確認すれば、このときにはこれを叩けばいいのか、というのがすぐに分かります。

また、Bluesky/ATProtocol ライブラリも順調に増えてきていて、各言語で Bluesky/ATProtocol の各機能を呼び出すのに障壁がなくなってきています。要するに下回りを自分で用意せずとも、すぐに作りたいものの作成にとりかかれる環境が整備されつつあるということです。 **既に先人が開発した、コミュニティーのライブラリや、各種クライアントアプリケーションが公式のコミュニティーページにまとまっています。**

https://atproto.com/community/projects

上記を見ていただければわかりますが、JavaScript (公式ライブラリ), Python, Ruby, PHP, Go, Rust, Java は既にクライアントライブラリが存在します (6/3 現在)。これら言語を使ってないよ、という場合は、ある意味逆にチャンスです！ **自分は Java のライブラリがなかったので作成し、このコミュニティーのページに掲載していただきました！**

### カスタムフィード

**また、Bluesky の機能自体にもエンジニアが関われる機能があります。それがカスタムフィードです。** カスタムフィードは簡単に言えば「フィード(タイムライン)を独自に定義できるもの」で、エンジニアの腕の見せ所の一つであると思います。 **例えば、猫好きな人のために猫の画像しか流れてこないフィードを作成したり、例えば、短歌として成立する投稿だけを集めたフィードを作成したり、** 自分のアイデアと技術力次第で、自由なフィードを作り公開することができます。単純に、SNS の機能としても画期的な仕組みだと個人的に思います。

https://github.com/bluesky-social/feed-generator

## ユーザーに制作物を触ってもらえる環境にあること

Bluesky はまだ全然人がいません。日本人に限った場合、恐らく 5000 人をちょっと超えたぐらいの人数規模だと想像しています。 (とはいえ、既にエンジニアの方が多く参加されているので、恐らく Twitter の知り合いでやっている人がいるのではないかと思います) とはいえ、そこに参加してコミュニケーションを活発にされている人達は、 **新しいものに飛びつくようなアーリーアダプターの人達** で、作ったものに対して積極的に触ってもらえて、フィードバックを貰える環境にあります。これは非常に楽しいことで、ものづくりの醍醐味の一つです。

宣伝になるのですが、自分も [SocialHub](https://www.uakihir0.com/socialhub/) という複数の SNS を見て投稿できるアプリを開発していて、そこに Bluesky も閲覧できるように機能を追加したのですが、そのアップデートの告知に対しても非常に多くの反応を貰いました。アクティブな人数に対してあまりにも大きい反響を頂いたので、すごく恐縮しています。ありがとうございました。

![](/images/bluesky/socialhub_bluesky.png)

### 活発なユーザーコミュニティー

Bluesky では日本人開発者コミュニティーが活発に勉強会などを開催していて、そこで制作物の発表なども行われています。先日 (6/2) も Bluesky/ATProtocol 勉強会 #1.5 があり、そこで先程挙げた自分の制作物の発表をさせていただきました。比較的緩い感じの勉強会になっているので、発表に慣れていない人でも入りやすいのかなと思います。作った制作物については、Bluesky に関連していれば基本的になんでも問題なく、真面目なものを紹介してもいいし、ネタ的なものでも大丈夫です。資料等が以下に掲載されているので、雰囲気を知りたい場合は是非参照してみてください。

https://428lab.connpass.com/event/284777/

# まとめ

Bluesky は遊び場に飢えているエンジニアにとって、 **これ以上にない楽しい活動の場** だと自分は感じています。単純に SNS として見ても Twitter の黎明期を感じさせる、 **自由でカオスな雰囲気** がとても楽しいです。現在は private beta で招待制なのでハードルがありますが、Twitter で参加してる友人エンジニア捕まえてきて誘ってもらいましょう。そして、適当に **アクティブそうな日本人を 100 人ぐらいフォロー** しましょう。そうして交流していくうちに、きっと何か作りたくなってくると思います！

**ではみなさん、Bluesky で会いましょう！**
