---
title: "LINE の LLM を iPhone で動かした話"
emoji: "📲"
type: "tech"
topics: ["ChatGPT", "llm", "ml"]
published: true
---

# はじめに

**昨今 ChatGPT をはじめとする生成 AI についても進化が凄まじく**、今まで簡単にはできなかったこと、人手が必要だったことがどんどん AI によって自動化されています。そんな中 **エッジ (オンデバイス) での AI 処理が流行る気配を見せつつあります**。最近発表された Google の Pixel8 では、端末のパワーを用いて AI の技術で写真を書き換えて、より良いものに変更していったりなど、エッジでの AI 処理の有用性を示してくれました。今回、自分はその中のチャレンジとして、iPhone の端末上だけで LLM (大規模言語モデル) を動作させて何かを作れないかを考えてみようと思い、調査も兼ねて実際に動作させるところまで試したので、その内容を共有します。

また、この記事を書いている本人は、この分野についての知識はほぼないので、色々間違っている部分があるかもしれません。その場合は、コメント等で指摘していただけると幸いです。

## 結論

先に結論を述べておくと、**現状における LLM のオンデバイス動作はまだまだ発展途上で、有用な活用法が検討できるレベルではなさそう** というのが結論です。これは日本語の話なので英語だともう少し異なってくる可能性もあるのかなと思います。

https://twitter.com/uakihir0/status/1711010276869570902

## LINE の LLM

今回 iPhone で動作させることを目標にしたモデルが LINE が公開している LLM です。

https://engineering.linecorp.com/ja/blog/3.6-billion-parameter-japanese-language-model

このモデルの詳細については上記リンクから見ていただきたいですが、とても日本語言語モデルとしては有用そうに見えます。一方で 36 億パラメータというのはちょっと心細く、ChatGPT は 100 兆パラメータと言われているのでその規模の違いが見て取れます。このパラメータ数の差異は主に、エッジで動かすか、クラウドで動かすのかの差があり、この LINE の LLM は一般ユーザーの保持する端末でも動かすことが可能なレベルのものとなっています。

## 先行事例

この記事は以下のページにもっとフォーカス当てて欲しいという側面があります。この方は、LINE のモデルが出る以前に出ていた rinna のモデルについて同様に iPhone で動かしています。この記事の内容は本当に参考になりました。

- [rinna-3.6b を mlc-llm を使って Mac で動かす](https://zenn.dev/kkatsuyoshi/articles/916ae1db24c9ec)
- [rinna-3.6b を iPhone でも動かす](https://zenn.dev/kkatsuyoshi/articles/b29bf21081278a)

## モデル

https://huggingface.co/uakihir0/japanese-large-lm-3.6b-instruction-sft-q4f16_0

**先に iPhone 向けに LINE の LLM を mlc-llm によって変換した (量子化した) モデルデータを置いておきます。** このモデルを mlc-llm の iOS プロジェクトに組み込むと動作確認ができると思います。モデルデータの作成方法については、先行事例を参考にしたので、合わせて見ていただくと分かりやすいと思います。断片的に書いているので、実行する環境のよって変えてもらえればと思います。

### 環境整備

```shell
// Install tools
brew install python cmake rustup-init git git-lfs

// Git clone mmlc-llm
git clone --recursive https://github.com/mlc-ai/mlc-llm.git
cd mlc-llm

// Setup python environment
python3 -m venv myenv
source ./myenv/bin/activate
pip install torch transformers sentencepiece protobuf
pip install -I mlc_ai_nightly -f https://mlc.ai/wheels

// Setyp rust environment
rustup-init
source "$HOME/.cargo/env"
```

### コードの変更

```diff python:./mlc_llm/relax_model/gpt_neox.py
         ffn_out_dtype = "float16"
     elif model.lower().startswith("redpajama-"):
         stop_tokens = [0]
+    elif model.startswith("japanese-large-"):
+        stop_tokens = [3, 0]
+        ffn_out_dtype = "float16"
     else:
         raise ValueError(f"Unsupported model {model}")
```

```diff python:./mlc_llm/utils.py
        "stablelm-": "stablelm",
        "redpajama-": "redpajama_chat",
+       "japanese-large-": "line_llm",
        "minigpt": "minigpt",
        "moss-moon-003-sft": "moss",
```

```diff cpp:./mlc-llm/cpp/conv_templates.cc
+ Conversation LineLLM() {
+   Conversation conv;
+   conv.name = "line_llm";
+   conv.system = "";
+   conv.roles = {"ユーザー", "システム"};
+   conv.messages = {};
+   conv.separator_style = SeparatorStyle::kSepRoleMsg;
+   conv.offset = 0;
+   conv.seps = {"<NL>"};
+   conv.role_msg_sep = ": ";
+   conv.role_empty_sep = ":";
+   conv.stop_str = "</s>";
+   // TODO(mlc-team): add eos to mlc-chat-config
+   // and remove eos from stop token setting.
+   conv.stop_tokens = {3, 0};
+   conv.add_bos = false;
+   return conv;
+ }

...

      {"conv_one_shot", ConvOneShot},
      {"redpajama_chat", RedPajamaChat},
+     {"line_llm", LineLLM},
      {"rwkv_world", RWKVWorld},
      {"rwkv", RWKV},
```

### モデルの量子化

先に Mac 向けに量子化ができるかどうかを確認します。

```shell
python build.py          \
    --quantization q0f16 \
    --hf-path=line-corporation/japanese-large-lm-3.6b-instruction-sft
```

```shell
python3 build.py                                    \
    --model japanese-large-lm-3.6b-instruction-sft  \
    --quantization q4f16_0                          \
    --target iphone                                 \
    --max-seq-len 768
```

### トークンの処理

`tokenizer.json` を作成します。

```
python3 -c "import transformers; tokenizer=transformers.AutoTokenizer.from_pretrained('line-corporation/japanese-large-lm-3.6b-instruction-sft'); tokenizer.save_pretrained('tokenizer', legacy_format=False)"
cp tokenizer/tokenizer.json dist/japanese-large-lm-3.6b-instruction-sft-q0f16/params/
```

### iOS 向けにモデルを配置

iOS アプリが読み込むモデルリストを更新します。

```diff json:./ios/MLCChat/app-config.json
 {
     "model_libs": [
-        "vicuna-v1-7b-q3f16_0",
-        "RedPajama-INCITE-Chat-3B-v1-q4f16_0"
+        "japanese-large-lm-3.6b-instruction-sft-q4f16_0"
     ],
     "model_list": [
         {
-            "model_url": "https://huggingface.co/mlc-ai/mlc-chat-vicuna-v1-7b-q3f16_0/resolve/main/",
-            "local_id": "vicuna-v1-7b-q3f16_0"
+            "model_url": "",
+            "local_id": "japanese-large-lm-3.6b-instruction-sft-q4f16_0"
         }
     ],
     "add_model_samples": [
-        {
-            "model_url": "https://huggingface.co/mlc-ai/mlc-chat-RedPajama-INCITE-Chat-3B-v1-q4f16_0/resolve/main/",
-            "local_id": "RedPajama-INCITE-Chat-3B-v1-q4f16_0"
-        }
     ]
 }
```

```diff sh:./ios/prepare_params.sh
 declare -a builtin_list=(
-    "RedPajama-INCITE-Chat-3B-v1-q4f16_0"
+    "japanese-large-lm-3.6b-instruction-sft-q4f16_0"
+    # "RedPajama-INCITE-Chat-3B-v1-q4f16_0"
     # "vicuna-v1-7b-q3f16_0"
```

以下のコマンドで、ライブラリのビルドとモデルのコピーを行います。

```shell
cd ios
./prepare_libs.sh
./prepare_params.sh
```

`tokenizer.json` のコピーを行います。

```shell
cp ../tokenizer/tokenizer.json dist/japanese-large-lm-3.6b-instruction-sft-q4f16_0
```

ここまでで作成されたモデルが、 `./ios/dist/japanese-large-lm-3.6b-instruction-sft-q4f16_0` 以下に配置されています。これを上げたものが [このレポジトリ](https://huggingface.co/uakihir0/japanese-large-lm-3.6b-instruction-sft-q4f16_0) になります。ここまで実行できれば、あとは `./ios` に含まれている iOS のプロジェクトを立ち上げて、諸々設定した上で実行すれば動作すると思います。

## 結論

このように、とりあえず、iPhone で動かすことはできましたが、量子化の過程の中で精度が落ちているのか、かなり受け答えには難がある形になりました。改めて、自分はこの分野についての知識はあまりないので、これが今の最善かと問われたら「分からない」というのが回答になります。もっといい方法がある等あれば是非教えていただければと思うので、よろしくお願いします。

https://twitter.com/uakihir0/status/1711010276869570902
