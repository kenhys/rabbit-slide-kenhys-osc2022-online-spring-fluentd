# OSSを継続的に\\nメンテナンスしていく\\n仕組みづくり

subtitle
:  Fluentdの事例

author
:  Kentaro Hayashi

content-source
:  OSC2022 Online/Spring

allotted-time
:  20m

theme
:  .

# 発表資料について

![](images/presentation.png){:relative-height="80"}

{::note}https://slide.rabbit-shocker.org/authors/kenhys/osc2022-online-spring-fluentd/{:/note}

# 今日お話すること

* OSS(Fluentd)を継続的にメンテナンスしていく仕組みづくり
  * リリースするための仕組みを改善したこと
  * プラグインを引き取る仕組みがあること
  * よいフィードバックをもらうために工夫したこと

# Fluentdとは

![](images/fluentd.png){:relative-height="90"}

{::note}https://docs.fluentd.org/quickstart{:/note}


# 最新のリリース

* v1.14.5 (2022/02/09)
  * `in_http`で`application/x-ndjson`をサポート
  * `out_forward`がハングする不具合修正

# 定期的にリリース

* Fluentd
  * おおよそ2ヶ月ごとにメンテナンスリリース
  * 修正をはやくユーザーに届ける
    * リリースしないと使われない
    * masterではなおってます、というのを少なく

# リリース作業

* Fluentd gemの公開
* ブログ記事公開
  * {::note}https://www.fluentd.org/blog/{:/note}
* Dockerイメージの公開
* TD Agentの公開(随時)

# 最新パッケージ版

* TD Agent 4.3.0 (2021-12-13)
  * Fluentd+プラグイン集
  * 各種プラットフォーム向け(deb,rpm,msiなど)
  * おおよそ4ヶ月ごとリリース
* Calyptia Fluentd

# 従来のパッケージの課題

* パッケージを簡単につくれる・維持できる仕組みが必要

# TD Agent v3

* Omnibusベースのビルドシステム
  * バンドルしたOpenSSLを使う
  * 環境を整えるまでが大変😟
  * 特定の人だけがビルドできる状態😟
    * 属人化したリリース🤔

# TD Agent v4

* fluent-package-builder
  * Dockerベースのビルドに刷新
    * 環境構築や属人化の問題を解決
  * arm64のサポート
  * GitHub ActionsによるCI
{::note}https://github.com/fluent/fluent-package-builder{:/note}


# 注意喚起

* TD Agent v3はすでにEOLです
  * {::note}https://www.fluentd.org/blog/schedule-for-td-agent-3-eol{:/note}
  * v4への移行手順も案内しています
    * {::note}https://www.fluentd.org/blog/upgrade-td-agent-v3-to-v4{:/note}

# 今日お話すること

* Fluentdを継続的にメンテナンスしたい
  * ~~リリースするための仕組みを改善したこと~~
  * プラグインを引き取るしくみがあること
  * よいフィードバックをもらうために工夫したこと

# Fluentdとプラグイン

* 1000以上の3rdパーティープラグインがある
  * {::note}https://www.fluentd.org/plugins{:/note}
* なかにはメンテナンスを継続できないケースも
  * 開発者が使わなくなった...etc


# fluent-plugins-nursery

* メンテナンスを継続するためのプロジェクト
* リポジトリを移行してメンテナンスを継続
* もちろん配下で開発し続けてもよい

{::note}https://github.com/fluent-plugins-nursery/{:/note}

# メンテナンスを移行するには

* contactリポジトリで移行の相談
  {::note}https://github.com/fluent-plugins-nursery/contact{:/note}
* issueでownershipの移転のための手続きを実施
  * 最近の事例: fluent-plugin-remote_syslog

# 今日お話すること

* OSS(Fluentd)を継続的にメンテナンスしていく仕組みづくり
  * ~~リリースするための仕組みを改善したこと~~
  * ~~プラグインを引き取るしくみがあること~~
  * よいフィードバックをもらうために工夫したこと


# コミュニティーサポート

* GitHub Issues
  * {::note}https://github.com/fluent/fluentd/issues{:/note}
* Discource
  * {::note}https://discuss.fluentd.org/{:/note}
  * GitHub Discussionsへ移行
    * {::note}https://github.com/fluent/fluentd/discussions{:/note}
* Slack 
  {::note}https://fluent-all.slack.com/{:/note}


# 従来のGitHub Issueの課題

* {::note}https://github.com/fluent/fluentd/issues{:/note}

![](images/fluentd-issues.png)

# ありがちなこと(1)

* Fluentdと直接関係ないプラグインの質問がくる
 
![](images/non-fluentd-issue.png)

# ありがちなこと(2)

* 再現に必要な情報が不足

![](images/missing-required-information.png)


# 各種ガイドラインを\\n読まない……ことも

![](images/issue-template.png)

# なぜこうなるのか？

* それなりに使われているソフトウェアでありがちなこと
  * 背景もばらばらな人がフィードバックしてくれる
  * 報告する人がわかっていることは省略する傾向にある
* 再現に必要な情報が不足している

# やりたいこと

* フィードバックを改善につなげられるようにしたい
  * 本体の不具合や機能に関する要望など
  * 不具合であれば**再現可能な情報**が含まれている

# どうするか？

* GitHubのissueの役割を**仕組みで**しぼる
  * 不具合報告 or 機能の要望に誘導する
  * 単なる使い方の質問のissueで埋もれさせない
  * プラグインに関するissueに埋もれさせない

# テンプレート選択画面を設定する

![](images/list-issue-forms.png)

{::note}https://github.com/fluent/fluentd/issues/new/choose{:/note}

# 使い方を知りたい人向け

* GitHubのissueからdiscuss(Discource)への誘導
  * 課題: 開発者以外で回答する人が不足😞
  * 開発者側でトラッキングがしにくい😞
  * 結果: あまりうまくいかなかった

{::note}https://discuss.fluentd.org/{:/note}

# 従来のIssueテンプレート

![](images/issue-template.png)

# 従来のIssueテンプレート

* 入力項目が別れていない
* 必須項目を指定できない
* テンプレートを消して報告してくる強者も

# 再現させるために必要な情報

* バグの内容
* 動作環境
* 再現方法
* 期待する挙動
* エラーログ
* 設定内容

# GitHub\\nIssue Formsを活用

![](images/issue-forms.png)

{::note}https://github.com/fluent/fluentd/issues/new?assignees=&labels=&template=bug_report.yaml{:/note}


# Issueのメンテナンス

* 反応がないIssueというのもある
  * 放置されたIssueはGitHub Actionで閉じる

{::note}https://github.com/actions/stale{:/note}

# 工夫したことまとめ

* Issueの役割を明確にする(不具合・要望)
* Issue Formsで必要な情報を集める
* 古いIssueをGitHub Actionsで閉じる

# よりオープンな開発方針

* GitHub Project
  * 実験的にプロジェクトメンバーで採用
  * 次のバージョンに入れるものとかを相談

{::note}https://github.com/orgs/fluent/projects/4{:/note}

# さいごに

* フィードバックを歓迎しています
  * バグに遭遇したらGitHubのIssueとして報告する
  * 既存のissueにコメントする(助け合いの精神で！)
* 開発に関しては次の発表で！
