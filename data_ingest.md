# denodo

# matillion

Matillion Pricing - Cost of our Data Integration Tools
https://www.matillion.com/pricing

従量課金

> Enterprise
> $2.70/credit

Credit Consumption Terms
https://www.matillion.com/legal/credit-consumption-terms

> Matillion ETL
> Matillion Data Loader

どっちや


Matillion Docs
Matillion ETL Docs
https://docs.matillion.com/?metl=true#batch-pipelines

Matillion Docs
Data Productivity Cloud Docs
https://docs.matillion.com/#batch-pipelines

まずこの2つが大きなタイプっぽい


でドキュメントの階層的にはData Productivity Cloudの中に

Batch Loads Introduction - Matillion Docs
https://docs.matillion.com/data-productivity-cloud/batch/docs/using-documentation/

があるっぽい。
でも料金表を見る限り、Data Productivity Cloudとは独立してMatillion Data Loaderだけをsubscribeすることもできるっぽいね

> Data Loader is an application for the bulk import or export of data using two functionalities: Batch pipelines and Change Data Capture (CDC) pipelines.

とあるのでデータ入れるだけならMatillion ETLよりこのMatillion Data Loaderなんかな？



Matillion ETL product overview - Matillion Docs
https://docs.matillion.com/metl/docs/1975061/

こっちはなんか色々な処理を書くためのものっぽいな

> Unlocks the power of your data warehouse: Matillion ETL pushes down data transformations to your cloud data warehouse, and processes millions of rows in seconds, with real-time feedback.

> Push-down ELT technology uses the power of your data warehouse to process complex joins over millions of rows in seconds.


とあるからELTに対応してるのは間違いなさそうだが、ETL処理もできるんだろうか

Instance sizes - Matillion Docs
https://docs.matillion.com/metl/docs/hub-instance-sizes/

ここら辺見る限りなんかETLのインスタンスサイズとか気にしてそうではあるけど。。。

アーキテクチャ図を見せろよわかりにくいなあ

Matillion ETL Architecture | Matillion Academy | Matillion Academy
https://academy.matillion.com/certifications/7cca930c-512e-11ed-9a78-02d47b69d3fd

これに書いてありそう。
コース形式なのかったるいが


Architecture overview - Matillion Docs
https://docs.matillion.com/data-productivity-cloud/architecture-overview/

data-productivity-cloudの方はこっちにちょっと書いてあったが、よくわからん



まあたぶん最低限のシナリオだとData Loaderってやつなんかな？


Credit Consumption Terms
https://www.matillion.com/legal/credit-consumption-terms

そうするとCreditごとに処理できるrowsが決まってるっぽい
で処理対象の行の数が増えるとめっちゃ割引が利くってことか？この表は

> As customer volumes pass through the consumption bands, any rows that are loaded within that band are charged at the burn rate for that band. This means incremental increases in usage become cheaper for Customer.

まあそうっぽいな
計算めんどそうだが



Batch Loads Introduction - Matillion Docs
https://docs.matillion.com/data-productivity-cloud/batch/docs/using-documentation/

> Data Loader is an application for the bulk import or export of data using two functionalities: Batch pipelines and Change Data Capture (CDC) pipelines.

CDCってなんだっけ

モダンデータスタック カテゴリ紹介 #24 『Change Data Capture(変更データキャプチャ)』 – Modern Data Stack Categories Overview Advent Calendar 2023 | DevelopersIO
https://dev.classmethod.jp/articles/modern-data-stack-categories-overview-advent-calendar-24-change-data-capture/

> CDCでは、データは一括ロードやバッチウィンドウではなく、リアルタイムで小刻みに転送されます。この機能により、CDCはリアルタイムのデータ移動で、より迅速かつ正確な意思決定を支援します。

CDC (Change Data Capture) について調べた
https://blog.foresta.me/posts/research-cdc-platform/

ふむ
まあリアルタイム連携ってことね

Batch Pipeline UI - Matillion Docs
https://docs.matillion.com/data-productivity-cloud/batch/docs/3140976/

batchでappendとかoverwriteとか選べないんか？
airbyteはあるっぽいが



# fivetran

Pricing | Fivetran
https://www.fivetran.com/pricing

従量課金やね

> Only pay for monthly active rows (MAR), which are rows inserted, updated or deleted by our connectors — not total rows. Each MAR is only counted once per month, no matter how many changes.
> The more unique data you sync, the less the unit cost — your cost per row declines automatically.
> For low data volumes, you can use Fivetran at no cost on our Free Plan.

これも行に課金
大量行で割引も利くっぽい


Streamlit
Fivetran Pricing Estimator
https://fivetranpricingestimator.streamlit.app/

使い方よくわからん
まあTotal MARと料金はわかる


なんとなくmatillionのdata LoaderとFivetranとで料金比較してみたけどあんまり変わらないような気もする


Fivetranが自動データ変換のためdbtと統合しました。 – FBP Partners Tech Blog
https://tech.fbpp.jp/archives/1020

へえ

(1980) Fivetranはじめの一歩 #DevelopersIO | クラスメソッド - YouTube
https://www.youtube.com/watch?v=BOyHOOGXLBo

まあイメージは湧いた
data ingestってほんとマッピングとかやらないんかね
censusはけっこう大変だったイメージだけど


# trocco

料金プラン｜TROCCO®︎(トロッコ) - データ基盤の総合支援サービス
https://trocco.io/lp/plan.html

プランごとに定額で一定の処理時間枠があって、

> 累積処理時間が処理時間枠を超過した場合は、追加課金が発生いたします。20時間単位での追加料金となります。

らしい。


これは処理時間で課金
ふ～ん


Blog | Rounda
https://rounda.co.jp/blog/same_data_airbyte_trocco/

最後の方よくわからんねむくて



# airbyte

Snowflake と Airbyte ではじめるモダンデータスタックへの道
https://www.awarefy.dev/blog/snowflake-with-airbyte/

Pricing | Airbyte - Open-source data integration
https://airbyte.com/pricing#pricing-calc

従量課金やね

> API source: $15 per million rows synced, or 6 credits
> Database, warehouse and file source: $10 per GB synced, or 4 credits
> Custom source: $15 per million rows synced, or 6 credits

これはデータソースごとに違うけど、
データベースがソースならデータ量で課金



とりあえずairbyteのOSSで雰囲気確かめて機能要件合ってるかやってみて、okならデータ量とロードマップと相談しながらそのままOSSで行くかCloud版に移行するかやってみたらいいんでは
まあトライアル版使えれば別に他のSaaS試すのでもいいかもしれないが。。。


Migrate from Airbyte OSS to Airbyte Cloud Questions - Q&A - Airbyte
https://discuss.airbyte.io/t/migrate-from-airbyte-oss-to-airbyte-cloud-questions/130

> Unfortunately you cannot import your source/destination/configs from your OSS to Cloud right now.

Migrate Airbyte config from one deployment to another (OSS) - Q&A - Airbyte
https://discuss.airbyte.io/t/migrate-airbyte-config-from-one-deployment-to-another-oss/370

> you can export → import the complete configuration from one instance to another. The only requirement is both instance must have the same version:


ふーむ
最新の情報が知りたいな
ここの移行ができるといいんだけど。。。
OSS版の設定をexportしてCloud版にimportとかできそうではあるけどな


まあでもOSS版てさ、dbtみたいにただSQL飛ばすだけなら別にいいけど、実際にデータを処理する製品をOSSでセルフホストで回すのって可用性ちゃんと持たせたり大変そうだけどな


自前でデータパイプラインをサクッと構築できる「Airbyte」を試してみた | DevelopersIO
https://dev.classmethod.jp/articles/dive-deep-into-modern-data-saas-about-airbyte/

まあなんかよさそう
内部ではdbtが動いてるらしい

へ～

わからんけどairbyteでいいんじゃない




# 観点

Censusの検証とかでやったみたいなことをまあやればよいような気もするけど、大変だよな

とりあえず課金体系とかSaaSなのかOSSなのかとかだけ見てみようかな。。。

あとはまあ最低限のシナリオで求められる機能の比較とか？

* 接続
  * データソース
  * データ出力先
* データ転送
  * ジョブ？
  * トリガー？定期実行？
  * CDC？
* スキーマ、カラム、型の対応づけ
  * 自動？
  * 手動？
* 設定のコード管理
  * git連携
  * インポート、エクスポート
* ELT
  * できるか
  * dbtとの連携など
* セキュリティ
  * region
  * VPC内のホスト
  * private link
  * VPN
* サポート
  * 日本法人
  * NTTDとのパートナー
    * Cutting Edge資料見る



airbyteはOSSで～とか言っちゃったから聞かれるよなあ
めんど

差あんのか機能に？


個人的にはこんなdata ingestというかmanaged etlの製品なんざ何年か経てばいくらでも勢力変わってるだろうから、いかに

* 作り込まずに使えるか
* 設定をエクスポートできるか
  * 他製品にも連携可能な形で。できるのか知らんけど
  * まあ最悪データソースとデータ転送先とのスキーマとか含めたマッピングとか型とか転送トリガーとかそういうのがドキュメントとかyamlとかでエクスポートできればよいような気もする。そしたらLLMで未来の製品の設定ファイルへの変換とかできるんじゃないの

を一番重視したいところではある。

料金なんかまあどれも似たり寄ったりじゃないの。
カラム数が少なくて行が多いから行課金よりデータ量課金の方が嬉しいみたいな明らかな特性があればまた違うかもしれないけど、まあテーブル一つじゃないんだしわからんだろ


まあでも他製品向けの設定のエクスポートなんてできないよなあ
てなるとまあ作り込まずに使うところかなあ。手間かけないために。

まあいずれにしてもそこら辺の機能要件かな
てなると上の箇条書きで書いた通りの感じかな

でもここら辺の製品比較やりたいなら案件にしてくれって感じだし、案件になっても一人じゃできないな
誰かBPさんつけてほしい


話変わるけどIcebergみたいなオープンなテーブルフォーマットがもっと普及していけばここら辺のIngestも不要になるというか、変換する必要ないんだからただ単にデータ持ってくるだけで済むようになるのかもね

そうだよな、まだそっちの方が将来性あるよね
いちいちこんなingestのためにちょこちょこ変換やってらんねえわな

今例えばcsvとかparquetを持ってくるみたいな感覚でicebergファイルの移動だけで済むようになるのが一番簡単だもんな

だからingest系の製品はいずれ淘汰されていくのかな

そう考えたらまあやっぱり一番手間かけずにingestできるやつならなんでもいいかな
そんな5年も10年も使うような製品にはならないでしょたぶん


1. 料金試算
   1. 大幅に安くなるものがあればここで絞る
2. セキュリティ
3. 機能


て感じか


reverse ETLはマッピングめんどかったけど、data ingestはそこまで大変じゃないんかな


# 要件

* matillionを検討してたようだけど、なぜそれではだめなのか？
  * まあ金額だろうと思うけどｓ
* データを入れるだけ？
* DWHに入れた後の変換はどうするの？
  * つまりELTは考えてる？
* GUIにこだわりある？
* 予算は？
* ロードマップは？
  * 最終的に取り扱うデータ量とか。試算のために。





# できること

全然ここら辺の製品知らないから、調べて比較するの大変なんだけど
がっつりやる余力はないなあ
11/6まで講演準備もあるし




# dbt

ちゃんと見てないけどどれもdbtとの統合はありそう
わからんけど

airbyteはめっちゃ内部でdbt動いてるっぽいな

