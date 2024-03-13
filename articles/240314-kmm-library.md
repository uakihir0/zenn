---
title: "Kotlin Multiplatform で作るクラスプラットフォームライブラリ"
emoji: "📖"
type: "tech"
topics: ["kotlin", "kotlin multiplatform", "kmp", "library"], 
published: true
---

# はじめに

**みなさん、ライブラリ作ってますか？** 私はサービスを作る時に、ドメインを切り出せる時はできる限りライブラリとして取り出すのを意識しています。また、ライブラリが一般に有用な場合は OSS として公開したいと思っています。では、その時にどのような技術スタックを利用するのか？ そこで登場するのが [Kotlin Multiplatform] です。Kotlin Multiplatform は Kotlin を使って様々なプラットフォーム向けに動くものを作ることができるフレームワークです。モダンな言語である Kotlin でライブラリを書けば様々なプラットフォームで利用できるので、サーバーとフロントエンドでコードを共有できますし、OSS としても様々な言語で動くのは非常に魅力的なはずです。

## Kotlin Multiplatform

https://kotlinlang.org/docs/multiplatform.html

[Kotlin Multiplatform] には、主に三種類の実行環境にむけたビルドを行うことができます。

- Kotlin/JVM
  - Java, Kotlin 等の JVM 実行環境向け
- Kotlin/JS
  - ブラウザ等の JavaScript 実行環境向け
- Kotlin/Native
  - iOS, MacOS, Windows 等のネイティブ実行環境向け

このように、Kotlin Multiplatform でコードを記述すると、上記の環境で動くライブラリを作成することができます。とはいえ、Kotlin Multiplatform は Kotlin Multiplatform 向けに作られたライブラリでしか動作させることはできず、例えば Java で書かれたライブラリを Kotlin Multiplatform で利用することはできません。そのため、かなりライブラリを書く時に必要な機能を持った別のライブラリが存在せず、自分で作る必要があったりなど**実装が簡単にいかない部分が所々存在します。** ここではいくつかの実装のポイントを紹介していきます。(問題点が増えたら追記します)

## 実装の難点

### 欲しい機能ライブラリがない！

**どうしようもないです。** とはいえ、Kotlin の公式からライブラリが少し提供されているので、それを使って実現できる範囲は決して少なくないです。[AAkira/Kotlin-Multiplatform-Libraries](https://github.com/AAkira/Kotlin-Multiplatform-Libraries) に Kotlin Multiplatform で作成された有名なライブラリがいくつか存在しますが、特定の実行環境は対応していなかったりと、結構残念な気持ちになったりすることがあります。**が、それはチャンスと考えましょう！ 自分が実装して第一人者になりましょう！**

### ドキュメントが全然ないんだけど

本当にこれで、公式ドキュメント見てもあまりどう実装すればいいのか分からない。**いや、正確には実装はできるんだけど、どうデプロイすればいいか分からない。** とか、ビルドツールの Gradle の使い方が全然分からないというのが結構あるあるだと思います。どこを見ればいいか？ というと頑張って色々ググるしかないので、自分が作ったプロジェクトを貼るので是非参考にしてください。

https://github.com/uakihir0/kmisskey

このライブラリは、国産 SNS である [Misskey](https://misskey-hub.net/ja/) のクライアントライブラリであり、Misskey の API をこのライブラリを用いて簡単に叩くことができます。上記に貼ったのは Kotlin Multiplatform のコードで、そのまま Kotlin/JVM 環境で動作させることができます。Kotlin/Native の iOS や MacOS 向けには上記の [kmisskey] ライブラリからビルドして出力した [kmisskey-cocoapods](https://github.com/uakihir0/kmisskey-cocoapods) を利用して Cocoapods からインストールできます。また、Kotlin/JS の JavaScript 向けには [kmisskey.js](https://github.com/uakihir0/kmisskey.js) を利用して npm からインストールできます。各々のビルド方法は Github Actions に記載されているので、Gradle を合わせて確認してみてください。

#### (参考) どんなコードになるの？

実際に Kotlin Multiplatform で作成した [kmisskey] ライブラリは、以下のようなコードで実行することができます。このように、大体どの実行環境でも同じようなコードで実行することができるのは Kotlin Multiplatform の魅力の一つです。

##### Kotlin/JVM

```kotlin
import work.socialhub.kmisskey.KmisskeyFactory
import work.socialhub.kmisskey.api.request.i.IRequest
...

val misskey = KmisskeyFactory.instance(
    "https://misskey.io",
    "CLIENT_SECRET",
    "ACCESS_TOKEN",
)

val response = misskey.accounts().i(IRequest())
println(response.json)
```

##### Kotlin/Native (iOS/MacOS)

```swift
import kmisskey
...

let misskey = KmisskeyFactory().instance(
  uri: "https://misskey.io",
  clientSecret: "CLIENT_SECRET",
  userAccessToken: "ACCESS_TOKEN"
)

let response = misskey.accounts().i(request: CoreIRequest())
print(response.json)
```

##### Kotlin/JS (JavaScript)

```javascript
import kmisskey from "kmisskey-js";
import KmisskeyFactory = kmisskey.work.socialhub.kmisskey.KmisskeyFactory;
import IRequest = kmisskey.work.socialhub.kmisskey.api.request.i.IRequest;
...

    const factory = new KmisskeyFactory();
    const misskey = factory.instanceUserAccessToken(
      "https://misskey.io",
      "CLIENT_SECRET",
      "ACCESS_TOKEN",
    );
    misskey
      .accounts()
      .me(new IRequest())
      .then((res) => {
        console.log(res);
      });
```

### Kotlin/JS でのコルーチン動作

Kotlin/JS のブラウザ向けにおいてはコルーチンの動作について難しい部分があります。**一番の難点は `runBlocking` の関数が Kotlin/JS にブラウザ向けビルドでは利用できません。**`runBlocking` は雑に表現すると非同期実行のコードを同期実行に変換するもので、`runBlocking` 内では非同期実行のコード (susupend 関数) を実行することができます。この非同期実行を同期実行に変換するのは、シングルスレッドで動作するブラウザ環境向けに実行することはできず、コード内で `runBlocking` を記述するとエラーになります。

では、そのまま非同期実行の関数 (susupend 関数) をライブラリとして提供してしまえばいいのでは？ と思うかもしれませんが、この **suspend 関数は JavaScript で Export を明示的に行う `@JsExport` アノテーションに対応しておらず、その関数を JavaScript 上で表現することができません。** また、susupend 関数は Java で使う時にちょっと面倒な形で出力されるので、ライブラリとしてはブロッキングの形で提供する方がシンプルに使うことができてよいのではないかと思います。(これは個人の意見です)

**一方で、Kotlin/JS には非同期実行のための `Promise` というクラスが JavaScript の `Promise` と対応するように提供されており、このモデルを返すことによって非同期処理を同期的な関数で返すことができます。** この `Promise` クラスは JavaScript のものなので、いつものように then 関数などで続けて処理することができます。

とはいえ、同じコードで Kotlin/JS 以外ではデータそのものを返して、 Kotlin/JS では `Promise` が返るというのはどういうこと？ となると思います。 Kotlin/JS には特別に `dynamic` という型が存在しており、JavaScript 言語同様にどんな型でもエラーにならない型があります。**この dynamic 型を利用して `runBlocking` 相当のものを Kotlin/JS 向けに書き換えます。**

```kotlin
// commonMain
expect fun <T> runBlocking(block: suspend CoroutineScope.() -> T): T

// jsMain
actual fun <T> runBlocking(block: suspend CoroutineScope.() -> T): dynamic {
    @Suppress("OPT_IN_USAGE")
    return GlobalScope.promise { block() }
}
```

この時注意点として、当然 `dynamic` で返した `Promise` 型は本来の T 型ではないので、下手にその値を触ってしまうと、ランタイムエラーになります！ そのため、この `Promise` を返す関数は必ずライブラリで返す値そのものにするようにしてください。そして、Kotlin/JS において、TypeScript の型定義が含まれているので、その定義を `Promise` をラップした型に書き換えるようにする必要があります。

# おわりに

[Kotlin Multiplatform] でライブラリ書くのは結構楽しいし最高だぞ！ と言いたいです。 Kotlin Multiplatform というと、Kotlin Multiplatform Mobile とか iOS, Android アプリを一纏りにして作るユースケースばかり目立っていて、なかなか共有ライブラリを作るぞ、というユースケースは見受けられないので是非チャレンジしてみてください！ そして、そのライブラリを OSS として公開して僕に教えて下さい！

[Kotlin Multiplatform]: https://kotlinlang.org/docs/multiplatform.html
[kmisskey]: https://github.com/uakihir0/kmisskey
