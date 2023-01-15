---
title: "画像生成 AI AnythingV4 を Google Colab で便利に使う！"
emoji: "🖼️"
type: "tech"
topics: ["Anything", "AI", "StableDiffusion"]
published: true
---

# はじめに

昨今、画像生成 AI の進化が凄まじい。 StableDiffusion 登場からの、NovelAI などの様々なモデルの発明、そこから様々なチームが様々なモデルを発明し、どんどんと画像生成 AI の制度が向上しています。最近 Anything の最新バージョンである AnythingV4 (V4.5) がリリースされ、更に一歩画像生成の制度とバリエーションが増えました。この記事は [AnythingV4](https://huggingface.co/andite/anything-v4.0) を [Google Colab](https://colab.research.google.com/?hl=ja) で動かし [StableDiffusion WebUI](https://github.com/AUTOMATIC1111/stable-diffusion-webui) で利用して、便利に画像生成を行う手順を紹介します。ベースとしては、以前 AnythingV3 を紹介した[過去記事](https://zenn.dev/uakihir0/articles/221113-anything3) を参照してください。

## 注意

AnythingV4 のモデルについては出自については AnythingV3 同様にやや怪しく、ウイルスが混入している可能性も否定できません。今回が、Google Colab 上で実行する方法を紹介するため、手元の環境にファイルを落としてくる訳ではないので、ウイルスの心配はないと思いますが、**ご利用は慎重に、かつ自己責任でお願いします。**

## 手順

### Colab ノートブックの作成

[Google Colab](https://colab.research.google.com/?hl=ja) にアクセスし、新規の Colab のノートブックを作成し、「編集 → ノートブックの設定」からハードウェアアクセラレータを「GPU」に選択。その後、以下のコードセルを追加して実行し、GPU が動いているかを確認します。

```python
# GPUの確認
!nvidia-smi
```

![](/images/anything/check_gpu.png)

### 実行環境の整備

次に、コードセルを追加し、以下の StableDiffusion WebUI をインストールし、加えて、AnythingV4 のモデルをダウンロードしてきます。

```python
!git clone https://github.com/AUTOMATIC1111/stable-diffusion-webui
%cd stable-diffusion-webui
#@title AnythingV4
!wget https://huggingface.co/andite/anything-v4.0/resolve/main/anything-v4.0-pruned.ckpt -O /content/stable-diffusion-webui/model.ckpt
!wget https://huggingface.co/andite/anything-v4.0/resolve/main/anything-v4.0.vae.pt -O /content/stable-diffusion-webui/model.vae.pt
```

この後に、`StableDiffusion WebUI` を起動します。その際に [gradio](https://gradio.app/) が使用され、出力結果の最後の方に URL が出力されます。その URL にアクセスすると、`StableDiffusion WebUI` を扱うことができます。

```python
!COMMANDLINE_ARGS="--share --gradio-debug" REQS_FILE="requirements.txt" python launch.py
```

![](/images/anything/gradio_link.png)

### 実行

StableDiffusion WebUI の画面は以下のようになっています。プロンプトと呼ばれる、どのような画像を生成して欲しいかを指定する呪文を入力し、任意でネガティブプロンプトと呼ばれる、どのような画像を生成してほしくないかを指定する呪文を入力し、生成ボタンを推すととりあえず画像が生成されます。[この画像](https://majinai.art/i/AHZzOpi) のプロンプト等をお借りして試しました。

![](/images/anything/sdwebui.png)

保存したら、画像を取得することができます。細かいですが、保存した画像にはメタ情報として、どんなプロンプトで生成したか等の情報が含まれており、StableDiffusion WebUI から復元することが可能です。`PNG Info` タブから、先程保存した画像を選択すると、どのようなプロンプトで生成された画像かを知ることができます。そこから `Send to txt2img` をタップして画像生成をさらにブラッシュアップしていくことも可能です。

![](/images/anything/sdwebuipng.png)

## まとめ

AnythingV3 を紹介した時から、かなり単純な形に落とし込むことができました。UI を使って画像生成をしたり、画像を保存してプロンプトを記憶したりするなど、より集中してプロンプトを選択できる環境になったのではないかと思います。 AnythingV4 のより進化したモデルを使って、想像力を働かせて様々なイラストを作成してみてください！
