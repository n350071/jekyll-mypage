---
layout: single
title:  "Railsへのプルリクがマージされました"
categories:
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
---

## マージされたリクエスト


[#32572](https://github.com/rails/rails/pull/32572)


## 説明

[Railsガイド](https://railsguides.jp/association_basics.html#%E8%87%AA%E5%B7%B1%E7%B5%90%E5%90%88)に従って自己結合を定義し、レコードを追加しようとしたところ「親となるレコードがいない」旨のエラーがでて、調べてみたところ[stack over flow にて「```optional :true```」をつけたら直るよ！との指摘](https://stackoverflow.com/questions/45321131/can-a-self-join-association-be-done-allowing-the-reference-to-be-blank)があり、そりゃそうかと思いつつ直したところ、うまくいきました。

知識の確認のため、Railsガイド日本語版だけでなく、英語版も確認したところ、 ``｀, optional: true｀`` がついていませんでした。
そこで、「これは、ガイドが間違ってて ｀, optional: true｀ をいれるべきなのか、実は ｀, optional: true｀ を書かずにfixする方法があるのか、どうなんでしょう？」という質問をコミュニティのエンジニアと会話して、Rails5系からBelongs toに変更があった気がする.. というコメントをもらい、再び[Railsガイド英語版のリリースノート](http://edgeguides.rubyonrails.org/5_0_release_notes.html#active-record-notable-changes)を確認し、ガイドがこのリリースでの変更をマージできていないと判断し、プルリクエストを送信しました。

Railsにおいてのマージされるブランチ名は、[既存のもの](https://github.com/rails/rails/pull/32481/commits)を参考にして心配だったのでTwitterで確認し、作成しました。
<blockquote class="twitter-tweet" data-lang="en"><a href="https://twitter.com/naoki_ishigaki/status/985134893465141249"></a></blockquote>
<blockquote class="twitter-tweet" data-lang="en"><a href="https://twitter.com/naoki_ishigaki/status/985136861461934080"></a></blockquote>
<script async src="https://platform.twitter.com/widgets.js" charset="utf-8"></script>
