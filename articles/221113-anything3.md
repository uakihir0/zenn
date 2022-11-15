---
title: "NovelAI よりも凄い？ AnythingV3 を Google Colab で試す！"
emoji: "🖼️"
type: "tech"
topics: ["Anything", "AI", "StableDiffusion"]
published: true
---

# はじめに

https://twitter.com/jaguring1/status/1591609069244383232

**NovelAI よりも凄い AI イラスト生成モデルがどうやら出たらしいぞ！！**

しかも `huggingface` にモデルが上がっているようで、`StableDiffusion` を使ってモデルを差し替えれば動くらしいということを知って、試してみたので、その過程を記載しておきます。この記事は基本的に `StableDiffusion` をモデルを差し替えて試してみるのとほぼ同等で、[Google Colab で はじめる Trinart Stable Diffusion](https://note.com/npaka/n/nc8c428c1bd01) の記事を参考にさせていただきました。大変ありがとうございます。

こんな感じで、自分も試してみたのですが、呪文が適当でも結構いい感じの画像が出てきます。しかも、Google Colab の無料の実行ランタイムで動くので、ちょっと重いですがお金もかかりません。

https://twitter.com/uakihir0/status/1591727813606150147

## 注意 (追記)

AnythingV3 のモデルについては出自については怪しく、NovelAI からリークしたものに手を加えられたもの、との疑惑もあります。また、一部のモデルファイルにおいて、ウイルススキャンした結果、トロイの木馬が検出されたという話もあります。今回が、Google Colab 上で実行する方法を紹介するため、手元の環境にファイルを落としてくる訳ではないので、ウイルスの心配はないと思いますが、**ご利用は慎重に、かつ自己責任でお願いします。**

https://twitter.com/jaguring1/status/1591618162864566272

## 手順

### Colab ノートブックの作成

[Google Colab](https://colab.research.google.com/?hl=ja) にアクセスし、新規の Colab のノートブックを作成し、「編集 → ノートブックの設定」からハードウェアアクセラレータを「GPU」に選択。その後、以下のコードセルを追加して実行し、GPU が動いているかを確認します。

```python
# GPUの確認
!nvidia-smi
```

![](/images/anything/check_gpu.png)

### 実行環境の整備

次に、コードセルを追加し、以下のパッケージをインストールします。

```python
# パッケージのインストール
!pip install -e git+https://github.com/CompVis/taming-transformers.git@master#egg=taming-transformers
!pip install pytorch_lightning tensorboard==2.9.1 omegaconf einops taming-transformers==0.0.1 clip transformers kornia test-tube
!pip install diffusers invisible-watermark
```

この過程において、ランタイムの再起動を要求されるので、[ランタイム → ランタイムを再起動] で再起動をしておくことをオススメします。その後、`StableDiffusion` をインストールします。画像生成の実行は、`stable-diffusion` のフォルダ内で実行します。

```python
# StableDiffusion のインストール
!git clone https://github.com/CompVis/stable-diffusion.git
%cd stable-diffusion
!pip install -e .
%mkdir outputs
```

次に、`AnythingV3.0` のモデルデータをダウンロードします。モデルデータは [Linaqruf/anything-v3.0](https://huggingface.co/Linaqruf/anything-v3.0/tree/main) に複数パターン存在するのですが、Google Colab の RAM の都合上、フルのモデルデータ (7.7GB) ではなく、選定されたモデルデータ (3.8GB) のものを利用します。

```python
# Anythingv3.0 のモデルデータを取得
!wget https://huggingface.co/Linaqruf/anything-v3.0/resolve/main/Anything-V3.0-pruned.ckpt
```

これで実行に必要なデータは一通り揃いました。

### 実行

テキストから画像生成するには、コードセルを追加し、例として、以下のように実行します。`--prompt "cute cat ear maid"` で指定している部分がプロンプトと呼ばれる、どのような画像を生成して欲しいかを指定する呪文の箇所になります。

```python
# テキストからの画像生成
!python scripts/txt2img.py \
    --plms \
    --ckpt ./Anything-V3.0-pruned.ckpt \
    --skip_grid \
    --n_samples 1  \
    --n_iter 1 \
    --outdir outputs \
    --ddim_steps 100 \
    --prompt "cute cat ear maid"
```

その後、左メニューのファイルから、`/content/stable-diffusion/outputs` 以下に作成されたファイルが確認できます。ダウンロードもできるので、好きなように使ってみてください。SNS に上げるときはぜひプロンプトと共に共有してくれると、呪文の解析が進みみんなハッピーになれます。

![](/images/anything/generated_image.png)

作業効率を上げるためにも、作成した画像を Google Drive にマウントしたフォルダに上げるなど、工夫をしてみるのもいいかもしれません。

### 補足 (追記)

####　 セーフフィルタの除去

Anything での画像出力は、NSFW (不適切画像) と判定される画像が出力されやすい傾向が個人的にあると感じています。実際に見てみても、特に NSFW と感じる内容のものはほぼ無く、あまりこのフィルターが意味を成しているようには見えません。そのため、作業の邪魔であるセーフフィルタを `txt2img.py` を変更することによって除去します。以下に自分が作成したセーフフィルタを除去した `txt2img.py` をダウンロードして、場所に展開します。

```python
# セーフフィルタのチェックをスキップした txt2img.py を txt2img2.py として保存
!wget https://gist.githubusercontent.com/uakihir0/bbb1bd8a4480e2bab07726ca0e744f91/raw/3e6f43fa5c112e512006cc9e33e932f6c6539d94/txt2img.py -O scripts/txt2img2.py
```

このファイルを用いるには以下のように、python の実行ファイル名を変更して実行します。オプションの内容は特に変更することなく使用することができます。

```python
# テキストからの画像生成
!python scripts/txt2img2.py \
# ... (略) ...
```

#### Stable Diffusion web UI 対応

また、Web UI 上で生成する画像に対してのプロンプトやステップ数などの設定ができる [Stable Diffusion web UI](https://github.com/AUTOMATIC1111/stable-diffusion-webui) に対応し、便利にしてくれた方がいましたので、ここで紹介させてもらえればと思います。ありがとうございます。

https://fls.hatenablog.com/entry/2022/11/14/202941

## まとめ

意外と簡単に試すことができるので、AI イラストってどうなの？ とか初心者の方にも是非試していただきたいです！
