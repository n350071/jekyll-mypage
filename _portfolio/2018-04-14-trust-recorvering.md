---
layout: single
title:  "顧客の信頼を失ったプロジェクトの立て直し"
header:
  teaser : /assets/images/akusyu_businessman.png
  show_overlay_excerpt: false
  overlay_color: "#333"

---

# 病院などの受付業務支援サービス

## 概要

### 実績

[アジャイル風の低品質プロジェクトの改革]({{ site.baseurl }}{% link _portfolio/2018-04-15-what-is-agile.md %})
が落ち着いて一段落したあと「顧客からクレームが入って次の契約が続かなさそうなプロジェクトを生き返らせて欲しい」と要請を受け、原因追求および対策を行い、２ヶ月で信頼回復しました。

### 立場

他のプロジェクトと並行してのプロジェクトマネージャー

## 詳細

### 全体把握

マネージャーから聞いた話では「現場のプログラマと、お客様のプログラマで認識齟齬が起きている。詳しい話は僕もわからないからメンバから聞いて。」と説明を受けました。メンバに話を聞いてみると、たしかに認識齟齬が何回か起きている様子でした。

まずは全体を把握するため、体制図や顧客とやりとりするRedmineやチケットの状況、次のリリースまでのチケット一覧、以前に問題のあったチケットなどを見せてもらったり、メンバだけではわからないことは、前任のPMに教えてもらうことにしました。

### 原因追求

#### マネジメントの問題はマネジメントにあり
最初の違和感は、前任のPMが体制図を把握していないこと、そして、私が「体制を顧客にも確認します」と言った時に「それはしなくてもいい」と嫌がる様子を見せたことでした。自分が把握できていなかったことが明るみになることを恐れている様子でした。

前任PMがマネジメントできていなかった線も可能性として見ていくことにして、まずは認識齟齬があった件について、原因と対策案を検討し、顧客との週一の定例会および、現場のプログラマ同士のミーティングで報告しました。

#### アジャイルとはいえ、作業フローが整理されていなくて現場が混乱していた

認識齟齬については、それぞれのプログラマが疑問を持った時点で確認をすれば済んでいた話で、それができなかったのは、チケットの状態ごとのワークフローが決まっていなくて、いまボールがどちらにあるか？宙ぶらりんになったまま放置されていたり、お客様も忙しいのでそこに配慮して質問できなかったなど、「顧客を含めたチーム全体の速度を最大にするにはどうするべきか？」という視点でのワークフロー整備ができていないことが原因でした。

#### じつは認識齟齬は大きな問題ではなかった

認識齟齬について解決し、最初の信頼回復はできましたが、顧客とよくよく話をしてみるとそこが原因ではありませんでした。プログラマ同士は仲がよく、メンバはよくやってくれているという評価でした。「メンバがよくやってくれている」というのは裏を返すと「メンバでないところが、よくやっていない」ということです。顧客に報告している資料やこれまでの経緯で変なところがなかったか？注意しながら、時間が許す限りプロジェクト内のフローを整理しました。

#### 過小報告

顧客に今月の稼働を報告する資料を確認してみると、フォーマットがわかりにくいだけでなく、事実に対して少なすぎる時間で報告をしていることがわかりました。理由をメンバに確認したところ「前任PMからの指示で少なめにしていました」とのことで、顧客に報告したところ、不信感の原因はここにありました。「夜遅くまで作業してくれているのに、時間数的には下限値ギリギリになっている様子を見て、専任で契約しているメンバが実は２つのプロジェクトを掛け持ちしているのではないか？と不審に思っていました」とのことでした。

### 対策

顧客の不満は、プログラマではなく、前任PMおよび営業に向いていたことがわかりました。顧客-営業で会話することがあっても、営業-現場で話をすることがなく、３者間で意見の食い違いがあり、それがねじれを生んでいました。まず、不信感の原因になっていた報告の仕方についてすぐに修正し、次の１ヶ月で営業も含め、顧客が本当にしたかった契約文言に変更し、社内でも営業-現場間で定期的に会話をする約束をしました。

### 結果

契約内容は、どちらにとっても不利になる内容ではなく、クリーンになったため、これまでのむやみな残業が減り、プロジェクトの利益率が少し上昇しました。その後、会社を離れてしまいましたが、プロジェクトは１年経ったあとも無事に続いているそうです。
