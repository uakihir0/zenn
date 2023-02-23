---
title: "画像生成 AI StableDiffusion を AppleSillicon Mac 簡単に使う３つの方法"
emoji: "🖼️"
type: "tech"
topics: ["AI", "StableDiffusion", "AppleSillicon"]
published: true
---

# はじめに

**昨今、画像生成 AI の進化が凄まじい。** StableDiffusion 登場からの、NovelAI などの様々なモデルの発明、そこから様々なチームが様々なモデルを発明し、どんどんと画像生成 AI の制度が向上しています。特に、最近の主流として、StableDiffusion モデルを色々混ぜ合わせて、独自のモデルを作り、それを [Civital](https://civitai.com/) 等で公開し評価し、更にモデルをブラッシュアップしていくという、進化のスパイラルが起きています。今まで、過去の記事においてクラウド上で、StableDiffusion を実行して AI イラストを作成する方法を紹介 ([参考](https://zenn.dev/uakihir0/articles/230115-anything4)) してきましたが、ここまで様々なモデルが存在すると、様々なモデルをダウンロードしてきて、切り替えて使用する、というケースも増えてくるかも知れません。ここでは、Windows の環境ではなく、AppleSillicon (M1,M2) 搭載の Mac において、比較的簡単技術的知識が浅い人でも StableDiffusion が実行できる３つの方法について記載していきます。

## 留意事項

StableDiffusion や画像イラスト生成の全般的な知識については、この記事では解説しません。以前自分が記載した過去記事を参考に、ある程度クラウド上で試した後に、ローカルでも試してみたいという方は、本記事を参考にしてみてください。過去記事については以下になります。

https://zenn.dev/uakihir0/articles/230115-anything4

## 方法 1: Draw Things を利用する

https://drawthings.ai/

Draw Things は iOS, Mac OS 向けの StableDiffusion 実行アプリです。StableDiffusion モデルを自分で読み込ませることもでき (Mac OS 版のみ？)、使用方法についても、比較的わかりやすい部類です。アプリのダウンロードは [AppStore](https://apps.apple.com/jp/app/draw-things-ai-generation/id6444050820) から可能です。以下は Mac OS アプリケーションを起動し、デフォルトのモデルをダウンロードしてきた後に、更に追加で [Civital: ChilloutMix (要ログイン)](https://civitai.com/models/6424/chilloutmix) というモデルをダウンロードして読み込ませた内容です。

![](/images/sdonmac/drawthings.png)

プロンプト・ネガティブプロンプトの設定や。Step 数など、見ての通り一通りの機能が揃っており、このアプリだけで完結します。また、一般的に画像生成 AI は高画質の画像を生成しようとすると、非常に時間が必要で、特に AppleSillicon Mac では辛いのですが、アップスケーラーも内蔵しており、解像度を手軽に 4 倍等に拡張することができます。このように、All in One のアプリとして非常に優秀なので、手軽にローカルで試したい場合に重宝するんじゃないかと思います。実行速度については期待しないでください。使用するマシンのスペックに依存しますが、クラウドの方が高速な場合が多いです。

## 方法 2: DiffusionBee を利用する

https://diffusionbee.com/

Draw Things と同様で、こちらは Mac OS 向けのみの StableDiffusion 実行アプリとして提供されており、上記のサイトからダウンロードして使用することができます。こちらもモデルを自分で用意して読み込ませることができます。使用方法についてはこちらの方が画面がシンプルでわかりやすいと思いますが、設定についてはやや簡素で、Draw Things の方が設定できる箇所は多いです。以下は、アプリケーションを起動してデフォルトのモデルをダウンロードした後に、更に追加で [HuggingFace: AnythingV4](https://huggingface.co/andite/anything-v4.0) というモデルをダウンロードして読み込ませた内容です。

![](/images/sdonmac/diffusionbee.png)

DiffusionBee の UI はシンプルでわかりやすく、デフォルト設定ではネガティブプロンプトの入力欄もありません。Option から、設定をすることが可能になります。DiffusionBee については、日本語で使い方を解説したサイトが幾つもあるので、検索してみると使い方が分かると思います。[こちら](https://original-game.com/how-to-use-diffusionbee-on-mac/) は細かく解説されており、分かりやすいです。

## 方法 3: StableDiffusion WebUI を利用する

こちらの方法は比較的簡単ではありますが、Python と Git のインストールが必須になります。クラウドで利用する場合は主にこの方法を利用するので、慣れた UI で触ることできるのが利点です。一方で、ローカルで利用する場合は、まずは Python と Git を以下のようにインストールします。Git って何？という方にはあまりこの方法はオススメできません。[こちらの記事](https://mac-ra.com/mac-automatic1111-stable-diffusion-webui/) の方が詳しく解説されているので、サクッと以下を読んだら、この記事を参照されることをオススメします。

```shell
# 先に homebrew をインストールしてから以下を実行します (https://brew.sh/index_ja)
# Python のバージョンは以下で固定しています。固定しない場合はエラーが出る場合があります。
brew install cmake protobuf rust python@3.10 git wget
```

次に、適当な場所に新しいフォルダを作り、その中で以下のコマンドをターミナルから実行します。StableDiffusion WebUI を Clone してきて、その中のフォルダに潜ります。

```shell
git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui.git
cd stable-diffusion-webui
```

適当にモデルデータをダウンロードしてきて、`stable-diffusion-webui > models > Stable-diffusion` に配置します。以下の例では、[Civital: ChilloutMix (要ログイン)](https://civitai.com/models/6424/chilloutmix) というモデルをダウンロードしてきて、配置しています。

![](/images/sdonmac/path.png)

その後、以下のコマンドを実行します。このコマンドは先程移動した `stable-diffusion-webui` フォルダ内で実行します。

```shell
sh webui.sh
```

すると、暫くダウンロードやセットアップが行われた後に、最後に以下のように出力され、最後の方 URL が表示されます。

```
DiffusionWrapper has 859.52 M params.
Applying cross attention optimization (InvokeAI).
Textual inversion embeddings loaded(0):
Model loaded in 15.4s (load weights from disk: 7.6s, create model: 1.3s, apply weights to model: 4.6s, apply half(): 1.2s, move model to device: 0.5s).
Running on local URL:  http://127.0.0.1:7860
```

その URL にアクセスすると、見慣れた StableDiffusion WebUI が表示されます。ここから、画像生成することができます。操作については[過去記事](https://zenn.dev/uakihir0/articles/230115-anything4) でも解説しているので割愛します。存分に画像生成を行った後、終了するときは、そのままターミナルを消してしまうか、`Ctrl+C` を押して実行を強制停止します。

![](/images/sdonmac/webui.png)

# まとめ

**Draw Things や DiffusionBee についてはお好みで選ぶと良いと思います。** DiffusionBee は操作は簡単ですが、サンプラーの設定が選べなかったりとやや不自由さがあります。一方で、Draw Things は画面がやや複雑で把握しにくい部分があります。**StableDiffusion WebUI は業界標準**なので、操作は慣れている人が多いかと思います。一方でセットアップがやや大変なので、手順を読んで理解できない場合は無理して入れる必要は無いと思います。
