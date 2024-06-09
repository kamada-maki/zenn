---
title: "ChatGPT-CodeReview×GitHub Actionsで記事のレビューをしてみた"
emoji: "🌟"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: []
published: false
---

## はじめに

少し前から AI を使ったコードレビューに関するサービスが増えてきているかと思います。
お手軽にレビューできるよう、ChatGPT-CodeReview を使ってみました。
https://github.com/anc95/ChatGPT-CodeReview

## ChatGPT-CodeReview とは

ChatGPT-CodeReview は、OpenAI の ChatGPT を使ったコードレビューサービスです。
以下の 3 つの利用方法があり、本記事は Github Actions を使った方法を紹介します。

- Github App
- Github Actions
- Self-hosting

## Github Actions でプロンプトを入れて使ってみる
