---
title: "ChatGPT-CodeReview×GitHub Actionsで記事のレビューをしてみた"
emoji: "🌟"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: []
published: false
---

## はじめに
少し前からAIを使ったコードレビューに関するサービスが増えてきているかと思います。
特に有名なのはCodeRabbitかと思いますが、個人利用の範囲だと使える範囲が限られているかと思います。
今回はもう少しお手軽にレビューできるよう、ChatGPT-CodeReviewを使ってみました。

## ChatGPT-CodeReviewとは
ChatGPT-CodeReviewは、OpenAIのChatGPTを使ったコードレビューサービスです。
以下の3つの利用方法があり、本記事はGithub Actionsを使った方法を紹介します。
- Github App
- Github Actions
- Self-hosting


## ChatGPT-CodeReviewを使ってみる