---
title: "ChatGPTとGitHub Actionsを使った記事の自動レビュー"
emoji: "🐤"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["ChatGPT", "OpenAI", "生成AI", "GitHubActions"]
published: true
---

## はじめに

CodeRabbit など AI を使ったコードレビューに関するサービスが増えてきているかと思います。
せっかく(?)、Zenn の記事を github で管理しているので、
今回は記事のレビューを ChatGPT-CodeReview を使ってお任せしてみました。

## ChatGPT-CodeReview とは

https://github.com/anc95/ChatGPT-CodeReview

ChatGPT-CodeReview は、OpenAI の ChatGPT を使ったコードレビューサービスです。
以下の 3 つの利用方法があり、今回は Github Actions を使ってみました。

- Github App
- Github Actions
- Self-hosting

思いっきり CodeReview って書いてますやん、と思うかもしれません。正解です。書いてます。
GithubActions でプロンプト入力して使う方法があったので、記事のレビューに使ってみました。

## Github Actions でプロンプトを入れて使ってみる

.github/workflows 配下に以下のような yml ファイルを作成します。

```yml
name: GPT Review

permissions:
  contents: read
  pull-requests: write

on:
  pull_request:
    types: [opened, reopened, synchronize]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: anc95/ChatGPT-CodeReview@main
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
          OPENAI_API_KEY: ${{ secrets.OPENAI_API_KEY }}
          LANGUAGE: Japanese
          OPENAI_API_ENDPOINT: https://api.openai.com/v1
          MODEL: gpt-4
          PROMPT: |
            あなたは技術記事を書くプロのライターです。回答は日本語でお願いします。
            渡された技術情報について、わかりやすく、読みやすい記事になるようレビューしてください。
            特に以下の点に注意してください:
              - 誤字脱字や文法ミスを指摘すること
              - 読者が初心者から中級者であることを想定して、専門用語の説明を含めること
              - 技術的なコンセプトやプロセスをステップバイステップで説明すること
              - 実例やコードサンプルを使って具体的に説明すること
              - 視覚的に分かりやすい図やイラストを追加する提案をすること
              - 重要なポイントを強調し、要点をまとめること
              - 個人的な見解や感想は、客観的事実と異なると分かるようにすること

            誰にとっても理解しやすい技術記事になるよう、積極的に工夫してください。
          top_p: 1
          temperature: 1
          max_tokens: 4096
          MAX_PATCH_LENGTH: 4096
```

補足をすると、

- `LANGUAGE`：使用言語
  - ENGLISH | 简体中文 | 繁體中文 | 한국어 | 日本語 が対応
- `MODEL`：使用モデル
  - `gpt-4` を指定しました（README には gpt-3.5-turbo で記載されていました）
- `PROMPT` ：プロンプト
  - ここでは、技術記事を書くプロのライターに対してのレビューをお願いしています
  - コードレビューであれば、ここのプロンプトを変更するだけで使えそうです
- `top_p` ：テキスト生成においてサンプリングの多様性を制御するパラメータ
  - 0 から 1 の間の値で、1 に近いほどランダム性が高くなります
- `temperature` ：テキスト生成において出力の多様性を制御するパラメータ
  - 0 から 1 の間の値で、1 に近いほどランダム性が高くなります
- `max_tokens` は生成するトークンの最大数
  - 生成されるテキストの最大を指定
- `MAX_PATCH_LENGTH` はパッチの最大長
  - レビュー対象となるコードの差分（パッチ）の最大長さを指定し、この長さを超える差分はレビューされない

※temperature と top_p については、下記ページご参考ください。

- top_p

https://platform.openai.com/docs/api-reference/chat/create#chat-create-top_p

- temperature

https://platform.openai.com/docs/api-reference/audio/createTranscription#audio-createtranscription-temperature

## 実際に使ってみた

実際に本記事をレビューしてもらいました。
PR を作成すると、Github Actions が実行されます。
![](/images/codereview3.png)

実際のレビュー結果がこちらです。

![](/images/codereview2.png)

途中で送ったのもあり、少し強め?のレビューがされましたが、
誤字脱字などはしっかり指摘してくれています。

#### 修正が入った箇所

誤字あるのと、URL 重複しています。ちゃんと指摘してくれています。

![](/images/codereview1.png)

## まとめ

今回、ChatGPT-CodeReview を使った GitHub Actions による記事のレビューをしてみました。
何度か試した結果、レビューの内容は毎回異なり、人間味?があって面白かったです。
レビュー内容は大きくズレはなさそうですが、もう少しプロンプトで指定できると良さそうです。
ある意味での不確定性があるレビューが静的解析ツールとは違うなと感じました。
コードレビューにも取り入れても良さそうだなと思いました。

---

最後に公開のために commit すると、優しめレビューがされました。ありがとうございます。
![](/images/codereview4.png)
