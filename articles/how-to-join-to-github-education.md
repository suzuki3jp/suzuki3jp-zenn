---
title: "学生証だけですぐ Github Education に通った話"
emoji: "😧"
type: "idea" # tech: 技術記事 / idea: アイデア
topics: ["Github"]
published: true
---

# はじめに
数年前から独学でプログラミングを学び、最近は個人開発をしている[鈴木](https://github.com/suzuki3jp)です。

今回は [Github Education](https://education.github.com) に**学生証のみ**で一瞬で通った話とそのやり方を記します。

# 前提
- 学校発行の Email は使用しない（学校に言えば発行してもらえたのかもしれないですが、面倒だったので学生証のみで申請しました。）
- github.com の公開情報に個人情報を載せたくない
  - 今回申請したアカウントはネット名でのアカウントなので本名や学校などの個人情報は載せたくありませんでした。
  - なので非公開情報である [`Settings` > `Billing and plans` > `Payment information`（請求情報）](https://github.com/settings/billing/payment_information) のみに個人情報を記載しました
- 通信制高校に通っているので、学校の位置情報と現在の位置情報はだいぶ違う

# 請求情報の設定
[`Settings` > `Billing and plans` > `Payment information`](https://github.com/settings/billing/payment_information) で自分の名前、自宅の住所などの情報を記入しましょう
:::message alert
後々学生証を翻訳して添付するので、ここの住所や名前は日本語ではなく**英語にしてください**
:::

# 学生証の翻訳
まずは学生証の写真をとり、それを適当な画像編集アプリで編集します。（私は Photoshop で行いました。）
ChatGPT などに適当に投げればそれっぽい翻訳を返してくれるので、学生証の各項目の隣にテキストを入力して下さい。
**年号表記になっているところは西暦に直して下さい**
以下は翻訳の例です: 
| 原文 | 翻訳 |
| ---- | ---- |
| 入学 | Enrolled in YYYY |
| 学籍番号 | Student ID number |
| 生徒氏名 | Student name |
| 生年月日 | Date of birth |
| 住所 | Address |
| 発行 | Issued on MM DD, YYYY |

# 申請
1. [申請ページ](https://education.github.com/discount_requests/application) に行って対象のアカウントでログイン。
2. 学校名を入力するとデータベースにある学校名を予測で出してくれます。（ここの英語学校名と学生証の翻訳の学校名同じにしてある方がいいかも？）
3. Continue
4. 翻訳した学生証をモニターに表示するか印刷し、それを撮影。（[背面カメラはうまく動作しない](https://github.com/orgs/community/discussions/49360) ので頑張ってインカメで撮りましょう😇）
5. Submit

# 申請が承認されるまで
Submit すると即時～数分で承認されたか拒否されたかを確認できる状態になります。
再度 [申請ページ](https://education.github.com/discount_requests/application) にアクセスすると申請がどうなっている確認することができます。
緑のApproved が承認されたが特典はまだ利用可能ではない状態、紫のApproved が特典も利用可能な状態。Rejected がリクエストが拒否された状態。
![](/images/how-to-join-to-github-education/request-status.png)
:::message alert
緑Approved から 紫Approved になるまでには数日かかるので気長に待ちましょう。
自分は 11/29 に 緑Approved、12/02 に 紫Approved になりました。
特典が利用可能になると Github からこんなメールが届きます。
![](/images/how-to-join-to-github-education/welcome-to-github-education.png)
:::

# [余談] Github Copilot を使う
自分の場合は特典が利用可能になったというメールを受け取った直後 Copilot を使用しようと思ったら使えなかったので、調べたら別途申請する必要があるみたいです。（これは一瞬です）
[Github Copilot](https://github.com/features/copilot/) > `Get started with Github Copilot` > `Copilot Individual` > `Start a free trial`

# 参考
https://zenn.dev/pipipipipi/articles/5d234d58246715
https://qiita.com/girlfellfromsky/items/735c7ed53f7cb8c63788
https://qiita.com/SNQ-2001/items/796dc5e794ac3f57a945

# さいごに
学校発行の Email を用いた申請の記事はたくさんあったのですが、学生証のみはあまりなかったので、今回の記事が誰かの参考になれば幸いです。
現在 `PlaylistManager` というWebアプリケーションを開発しているので、よかったらリポジトリを覗いてみてください:)
https://github.com/suzuki3jp/PlaylistManager
https://zenn.dev/suzuki3jp/articles/nextauth-approuter-google-20241025
https://zenn.dev/suzuki3jp/articles/nextjs-i18n-20241115