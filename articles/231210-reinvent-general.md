---
title: "AWS re:Invent 2023 参加記録【全般編】"
emoji: "🎪"
type: "idea"
topics: ["aws", "ai"]
published: false
---

# はじめに

**AWS re:Invent 2023 に参加してきました。** AWS re:Invent は毎年開催されている AWS のカンファレンスで、今年も 11 月 27 日から 12 月 1 日までの 5 日間に渡って開催されました。この記事では自分がどのような体験をそのイベント内でしたかを中心に、備忘録として参加記録を書き残していきます。参加記録は４つに分かれており、キーノートなどイベントのメインとなる部分を集めた本ページである**全般編**と、自分の受けたセッションやワークショップについて記録を残した**体験編**と、re:Invent の様々なトピックを巡る**あれこれ編**、最後に観光などラスベガスの過ごし方を記録した**観光編**に分かれています。

https://zenn.dev/uakihir0/articles/231210-reinvent-experience

https://zenn.dev/uakihir0/articles/231210-reinvent-arekore

# 筆者について

**筆者は AWS をインフラとして利用している会社でバックエンドのアプリケーションエンジニアをしています。** インフラを直接管理することはあまり多くはなく、基本的には SRE のチームに任せており、アプリケーションに合わせた細かい調整などをアプリケーションに合わせて調整するということをしてきました。他の参加者を見ても、あまり日本人のアプリケーションエンジニアが参加している例はみないので、結構稀なタイプの参加者であると自覚しています。（そもそも re:Invent の参加費が 30 万円程するので、ラスベガスへの旅費を合わせても相当な費用になるので、SRE ロールが中心の参加になるのは当然だとは思う）今回、re:Invent 参加するにあたって、AWS を改めて学び直すという意味合いで、AWS 認定ソリューションアーキテクトアソシエイト (SAA-C03) を取得してイベントに臨みました。試験の取得については別記事にまとめています。

https://zenn.dev/uakihir0/articles/231107-saa-c03

# re:Invent 2023 とは？

![](/images/reinvent2023/banner.png)

**毎年ネバタ州ラスベガスで開催される AWS のカンファレンスです。** ラスベガス・ストリップと呼ばれるラスベガスの中心の大通り沿いに聳え立つ複数のホテルのコンベンションセンター (カンファレンスホール) を使って、期間中、千を超えるセッションやワークショップが開催されます。最終日以外毎日行われるキーノートでは AWS の顔といえる人物が前に立ちの新サービスなどが発表されます。参加者の数はラスベガス現地で約 5 万人で、期間中はラスベガス中心街がイベントムード一色になり、どこにいってもイベント参加者に遭遇します。オンラインでも参加可能で re:Invent で行われるセッションの一部は YouTube にも動画がアップロードされ、後で確認することができます。

# 全体を通して

**今年の re:Invent は全体的に Generative AI (生成 AI) についてのセッションが多く**、どのようなユースケースで生成 AI を用いるのか？ どのようにして生成 AI を用いるのか？ そして自分のビジネスに対してどのように生成 AI をチューニングするのか？ という生成 AI を使うための幅広い知識を得ることができました。生成 AI については今年の技術トレンドでもあり、AWS においても無視できない存在であることが背景にあります。(Google Cloud 等においても同様の傾向です)

# Keynote

**re:Invent 2023 において 5 つのキーノートが行われました。** キーノートは re:Invent において新サービス等が発表されるまさにキーとなるセッションで、非常に力の入れようが大きいものになっています。パートナーキーノート以外を見たので、それについて記載していきます。文中のリンクは、AWS の公式だったり、[DevelopersIO](https://dev.classmethod.jp/) など分かりやすい解説サイトに飛ぶようになっています。

## Monday Night Live Keynote with Peter DeSantis

https://www.youtube.com/watch?v=pJG6nmR7XxI

### 概要

![](/images/reinvent2023/key1_1.png)

コンピューティング部門での Senior Vice President である Peter DeSantis によるキーノートです。このキーノートでは、サーバーレスへの道というテーマで、シナリオ仕立てでサーバーレスの歴史や、サーバーレスの現状、そしてサーバーレスの未来について語られました。**[`Amazon Aurora Limitless Database`](https://dev.classmethod.jp/articles/report-achieving-scale-with-amazon-aurora-limitless-database/) や [`Amazon Elasticache Serverless`](https://dev.classmethod.jp/articles/amazon-elasticache-serverless-ops-made-easy/) が発表** され、サーバーレスの新たな選択肢が提示され、よりサーバーレスの世界に踏み込めるようになったのが特徴です。また、キーノートの最後に、量子コンピューターのチップ開発を行っていることを発表し、実際のチップの現物を見せるというパフォーマンスもありました。

### 感想

![](/images/reinvent2023/key1_0.png)

**re:Invent についてすぐのキーノートだったので、非常にその規模の大きさに驚かされました。** キーノートが行われる前は、カバーバンドが会場でライブをしており、場を暖めていました。また、飲み物も配られており、一日目限定なのかお酒も中にはありました。（流石に日本からの移動で疲れていたので飲まなかったのですが）キーノートの内容としては、`Amazon Aurora Limitless Database` と `Amazon Elasticache Serverless` どちらも非常に興味深く、またすぐにでも実戦投入できそうだなという印象でした。Aurora の方はアプリケーションエンジニアとして気にすべき事が少し増えそうな感じはありますが、分散されたデータベース環境を使用することができるようになり、シャーディングのロジックなど、アプリケーションにおいてケアが必須だった非常に難しいデータのルーティングをある程度考えなくて済むようになるのは大きいアップデートだと感じています。Elasticache については運用が楽になるのは言うまでもなく嬉しいので、より Elasticache を選択する機会が増えるのではないかと思います。

## CEO Keynote with Adam Selipsky

https://www.youtube.com/watch?v=PMfn9_nTDbM

### 概要

![](/images/reinvent2023/key2_1.png)

AWS の SEO である Adam Selipsky によるキーノートです。このキーノートは re:Invent におけるメインのキーノートで、広範囲に渡る AWS の新サービスが発表されました。[`Amazon S3 Express One Zone`](https://dev.classmethod.jp/articles/amazon-s3-one-zone-storage-class-compare/) が発表され、非常に高頻度でアクセスされるファイルに対してのストレージとしての選択肢が増えました。また、Graviton プロセッサーの進化として [`Graviton4`](https://cloud.watch.impress.co.jp/docs/event/1550612.html) が発表されました。また、AI 学習用のアクセラレータ Trainium の最新製品として、`Trainum2` が発表され、AI 学習のさらなる高速化が可能になりました。また、生成 AI 関連では、`Amazon Bedrock` の FineTuning 等のカスタマイズ手法の提供であったり、また今回の目玉と言っていい **[`Amazon Q`](https://dev.classmethod.jp/articles/amazon-q-lineup/) が発表され、AWS を用いた様々なワークロードの中で、Amazon Q を用いた解決を行うことができるようになりました。**

### 感想

![](/images/reinvent2023/key2_0.png)

二日目の朝食後にこのキーノートに参加するために並び始めましたが、メインのキーノートということもあり、非常に多くの人は既に待っており、今か今かとキーノートが開始されるのを待ちわびていました。初日のキーノート同様にロックのカバーバンドが会場を予め温めていて、会場は開始前から盛り上がっていました。発表内容の半分程が生成 AI が占めており、その中でも `Amazon Q` の発表は多くの時間を使用していました。`Amazon Q` は AWS コンソールにおいて質問に答えてくれる機能だけではなく、AWS クライアントライブラリを用いたコード作成支援だったり、ビジネス上のナレッジツール等をまとめてビジネスに関する質問に答えてくれる機能であったりと、こういうの欲しいでしょ？ と言わんばかりの機能を提供してくれています。生成 AI で先に欲しいだろうという部分を先回りで提供してくれるあたり Amazon は流石だなと思わせられました。また、`Amazon Bedrock` については、PreTraining、FineTuning、RAG といった生成 AI を自分達のビジネスに合わせることのできる機能が追加され、いよいよ実用的になってきたと思いました。

## Keynote with Dr. Swami Sivasubramanian

https://www.youtube.com/watch?v=8clH7cbnIQw

### 概要

AWS の VP である Dr. Swami Sivasubramanian によるキーノートです。このキーノートでは、AWS の AI に関する取り組みについて語られました。Amazon Bedrock のアップデートとして、前日に公開された各種学習内容に加えて、`Anthropic Claude 2.1` などの新しく複数のモデルが使用可能になることがアナウンスされました。また、`Titan Image Generator` も使用可能になり、Stable Disffusion 以外の画像生成選択肢も使用可能になりました。生成 AI のモデルについて各種データをベクトルで扱うため、その格納先や検索として AWS の各種データベースサービスについて、ベクトルデータのサポートを発表した。`Amazon Aurora` `MongoDB` がサポート予定で、`Amazon DocumentDB` `Amazon DynamoDB` `Amazon OpenSearch Service Serverless` が更にベクトルデータ検索がサポートされる予定であることが発表された。また、**誰でも簡単に生成 AI を用いたアプリケーションを作成することができるプレイグラウンドである [PartyRock](https://partyrock.aws/) が発表されました。**

### 感想

![](/images/reinvent2023/key3_0.png)

このキーノートは、Venetian のキーノート会場ではなく、少し外れにあるホテルの Mandalay bay のサテライト会場である Content Hub から見ました。(みたいセッションがこのホテルだったのと、そのセッションとキーノートの時間が被っていたため) このキーノートを聞いたタイミングでは、あまり生成 AI に対しての理解が進んでおらず、発表された内容について、なんとなく嬉しいんだろうなぁ、と聞きながら微妙な感想を抱いていたんですが、他のセッションを聞いたりしていく中で、発表された内容の有り難みに気付くようになりました。`Anthropic Claude 2.1` は長いプロンプトでも読んでくれるため、RAG などでよりよい回答を出すことができる可能性が高まることや、各種ベクトルデータベースについても同様に RAG の過程においてよく使用されるため、より生成 AI のワークロードを AWS に容易に構築できるようになったという事です。PartyRock は単純に面白そう。無料で使えるので色々試してみようよ思います。

## Keynote with Dr. Werner Vogels

https://www.youtube.com/watch?v=UTRBVPvzt9w&t=16s

### 概要

![](/images/reinvent2023/key4_1.png)

AWS の CTO である Dr. Werner Vogels によるキーノートです。このキーノートでは、前半は **[The Frugal Architect](https://thefrugalarchitect.com/) というコストを考慮した持続的なアーキテクチャを作るにはどのようなことを意識すべき** か、を紹介しつつその中で、**システムのオブザーバビリティを高める [`AWS Management Console myApplications`](https://aws.amazon.com/jp/blogs/aws/new-myapplications-in-the-aws-management-console-simplifies-managing-your-application-resources/) と [`Amazon CloudWatch Application Signals`](https://aws.amazon.com/jp/blogs/aws/amazon-cloudwatch-application-signals-for-automatic-instrumentation-of-your-applications-preview/)** を発表しました。後半では、未来を予想すると題して、生成 AI についてその活用事例を実際に本人が行った取り組み等を通じて紹介されました。その中で [`Amazon SageMaker Studio Code Editor`](https://aws.amazon.com/jp/blogs/aws/amazon-sagemaker-studio-adds-web-based-interface-code-editor-flexible-workspaces-and-streamlines-user-onboarding/) など生成 AI のワークロードの作成を支援するためのツール群が幾つか発表されました。

### 感想

![](/images/reinvent2023/key4_0.png)

自分はあまり詳しくなかったので re:Invent に参加するまで知りませんでしたが、Dr. Werner Vogels さんは名物のおじさんらしく、**前日ホテルに帰る前に、近くを歩いていたクラスメソッドさんの参加者が「俺の推しだから早起きしてキーノート並ぶぜ」みたいなことを言っていました。** (クラスメソッドさんは [DevelopersIO](https://dev.classmethod.jp/) を運営している AWS 通の会社で、今回の re:Invent にも 50 人以上の方が参加したらしい) キーノートの内容も示唆に富んだ内容で、コストと持続可能なアーキテクチャを意識しようという内容で、やや耳の痛い話でした。もちろん会社によって優先すべきトレードオフの項目が異なるとは思いますが、コストはその中でも重要な位置付けとしてあると再認識しました。また、後半の話の中で生成 AI を使った取り組みについて「自分でもできたんだからあなた達でもできる」と話していたのは強く印象に残っています。本当にそうかどうかはさておき、一歩踏み出すことの大切さを改めて感じました。

## 全体の感想

**キーノートの内容は至る所でまとめられ、新サービスの情報を仕入れる分には、特にわざわざラスベガスにまで足を運んで見るものではありません。** 一方で、現地で見る意味として、**ユーザーがどのような熱量で AWS に期待しているのか？** などを知ることができます。キーノートにおける新サービスの占める内容は実は全然多くなく、動画を見てもらえば分かるとは思いますが、各々の方が結構自身の哲学的なことを語る時間が多いです。それは彼らが見てきたものであったり、目指しているものであったりと、ある種の旅路を追うようなものになっており、その内容には多くの気付きが含まれています。そういった彼らの語る重い言葉を生で聞き、その場にいる人達の反応を見ることで、自分達に何が必要で何を目指すべきなのか、少し未来という靄が晴れてくるような感じがします。ラスベガスは遠く、誰もが簡単に来れる場所ではないですが、参加することによって何らかの一助が得られることは間違いありません。
