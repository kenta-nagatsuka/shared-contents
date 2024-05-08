https://api.python.langchain.com/en/latest/_modules/langchain_core/document_loaders/base.html#BaseLoader.load
これを継承して、
https://api.python.langchain.com/en/latest/_modules/langchain_community/document_loaders/cube_semantic.html#CubeSemanticLoader
これを作っていて、
loadのところではlazy_loadが呼ばれている。（base classでそういう実装になっている）
でlazy_loadではmeasure(dbtで言うところのmeasure+metrics)とdimentionをまとめてカラムと呼び、このカラムごとにpage_contentとmetadataを作り、それをリストとして持っている。

で
https://github.com/cube-js/cube/blob/master/examples/langchain/streamlit_app.py
この実装では、カラムごとのその情報をベクトルDBにつっこんだものに対して、questionとの類似度が1番高いものを取り出して、そのカラム情報がメタデータとして持っているテーブルを取得、
さらにそのテーブルをメタデータに持つものというフィルターかけた上で、”All available columns”という文字列との類似度でカラム情報を15個取ってきて、（ここが何がしたいのかよくわからない。そのテーブルに紐づく全てのカラムが欲しいなら類似度検索するまでもなく単に取ってくればいいだけじゃないのか？）（少しでも関係しているカラムを検索したいなら、元々のユーザーのquestionを突っ込めばいいのではと思うのだが）
そのカラム情報（カラムではない。カラムごとにメタデータを含んでいる）をプロンプトに渡した上で、sqlを生成させている。

そもそもCubeの実装がよくわからないが、これだと1つのテーブルからのmeasureなりdimension なりしか登場できないんじゃないか？



でこの生成したsqlをsemantic layerに投げているのだろうか？そこまでは追えていないが、なんかデータベースに直接投げていそうな雰囲気のプロンプトでもある。


ふむ

streamlit_app.pyとかutilのプロンプトとか見たが、投げているのはcubeに対してだな。
で、cubeに対して投げるクエリが、かなり普通のSQLっぽく書けるみたいだなどうやら。カラムのところだけcubeで作成しているmeasureとかdimensionsとかの、ベクトル類似度検索で取ってきたものを指定するようにしている。
つまりCubeにクエリを投げる側からすると、ほとんど普通のSQLを書くような感じで書けるっぽい。これは普通にsemantic layerとして好ましい特徴だな。
だからプロンプトではカラムのメタデータだけ渡して、後は割と自由にSQLを生成させているわけか
当然複数のmeasureを組み合わせたクエリの生成も許していそうなので、これは1つのメトリクスの特定と利用ではなく、関連していそうなメトリクスとディメンションを取ってきた上で組み合わさせているな
そういう意味ではガバナンスという意味では一つのメトリクスの特定と利用をするように書き換えた方がわざわざsemantic layerを使う意味があると思うが

あとなんで1つのテーブル由来のカラムしか取ってないのかわからない。

プロンプトにはpay attention to which column is in which table.とあるので自分の勘違いならいいけど、、、
langchainのlazy_loadの実装見る限りはやっぱりカラムのメタ情報をベクトルDBに突っ込んでいそうだし、、、テーブルというのがもしかしたらcubeでは独特の意味を持つ、とも考えにくいしなあ、、、


ていうか
https://blog.langchain.dev/cube-x-langchain-building-ai-experiences-with-llms-and-the-semantic-layer/
Note that only views are loaded as they are considered the "facade" of the data model.

で、
https://cube.dev/docs/reference/data-model/view
Viewはこれで、なんかテーブル間の関係性を定義してそうだが、

結局テーブル間の関係性はCubeSemanticLoaderでは何もloadしなくないか？テーブル名ってのが普通にテーブル名な限りは。
つまりknowledge graph的な情報を何も使ってなくないか？
ViewとかのCubeの用語がよくわかってないし、langchainとのコラボの実装を細かくは追ってないから断言はできないけど、そう見える。

Knowledge graph的な情報をうまく使いたいなら、そしてガバナンスを効かせたいといつ需要に応えるならやはり以下の流れがいいように思える。
メトリクスのメタデータをつっこんだベクトルDBを用意ユーザーからの質問との類似度でメトリクスを検索特定したメトリクスが利用可能なdimensionsをsemantic layerのAPIをたたいて取得特定したメトリクスとdimensionsを使ってgroup byやwhereを含むSQLなり、それに相当するコマンドラインを作成APIリクエストを送る

この実装がCubeとかdbtでできるのかはわからないが、要は
* ユーザーからのクエスチョン1つに対して該当メトリクスを1つ特定する
* その後はknowledge graph的な情報をたどって利用可能なdimensionsを列挙する
* 最後にそれらをプロンプトに渡して（広義の）クエリを作成させる
という流れがいいんじゃないかというのが感想。


Cube Langchainの実装もわかるけどね。
メトリクスとメトリクスを組み合わせた情報が欲しいときもまああるとは思う。
でもそれを許してしまうと、結局データ提供側が想定していないクエリが生成されてしまう可能性があるわけなので、元々解決したかった課題に立ち戻ってしまうと思うんだよな。


まあdbtはクラウド版じゃないとsemantic layer使えないしcubeかな、、、


https://cube.dev/docs/reference/rest-api#v1meta
Cubeのmeta情報は取れそうだけど、テーブル間の関係性が定義されてそうなViewのmeta情報はどうやって取るんだ？
まあそれはそれこそlangchainの実装見ればいいのか？

https://api.python.langchain.com/en/latest/_modules/langchain_community/document_loaders/cube_semantic.html#CubeSemanticLoader
response = requests.get(f"{self.cube_api_url}/meta", headers=headers)
これ見る限り、cube（狭義。semantic modelみたいなやつ。）のメタ情報取ってそうだけどな。結局measureとかdimensionのメタ情報をベクトルDBに入れてるわけだし。


既存実装がどうなってるかはどうでもいいんだが、knowledge graph的な情報というか、テーブル間のrelationshipというか、あるメトリクス（広義）が利用可能なdimensionを取得したいんだよな
まあメトリクスさえ取れれば、そのメトリクスに使っているカラムが所属しているテーブルが持っている他のカラムとか、そのテーブルとrelationshipがある他のテーブルのカラムを頑張れば取ってこれるのかもしれないけども、、、
できればそういうのはsemantic layerから取得したい。
CubeのAPIでそういうものが無ければそこを自前で実装するしかないのか？

https://cube.dev/docs/product/data-modeling/concepts
Joins define the relationships between cubes, which then allows accessing and comparing properties from two or more cubes at the same time. In Cube, all joins are LEFT JOINs.
これ見る感じ、cube(semantic model)の中にテーブル間の関係も記述できそうではある。
てことはあの/metaってAPIでいいのか？じゃあviewってなんなのかよくわからないけど
ただこのJoinをしてもテーブル間の関係を記述するだけなんだよな
まあそれだけでもあるメトリクス（measure）の値を他のcubeのdimensionsでgroup byできるようになる、、、みたいなことなんかな
複数の生テーブル由来のカラムからなるメトリクスを作るにはどうするのかね
最初にcube（semantic model）作るときのsqlで生テーブルをjoinしたあとで、そのcubeでmeasureを作ればいいのかな
まああまりそういうユースケースなさそうではあるが。


https://cube.dev/docs/reference/rest-api#v1meta
よく見るとmeta取得のAPIのresponseにdrillMembersというのがある。

https://cube.dev/docs/reference/data-model/measures
Using the drill_members parameter, you can define a set of drill down fields for the measure. drill_members is defined as an array of dimensions. Cube automatically injects dimensions’ names and other cubes’ names with dimensions in the context, so you can reference these variables in the drill_members array. Learn more about how to define and use drill downs.


https://cube.dev/docs/guides/recipes/data-exploration/drilldowns
これはわかりやすい。cube(semantic model)のjoinで書いた他のcube？生テーブル？の中のカラム？ディメンション？を、各measureのdrillMembersに指定できるというわけだな。
でこの情報も当然cube(semantic model)に対するメタ取得のAPIで取れるわけだな。

あとは

https://cube.dev/docs/reference/data-model/measures
Cube automatically injects dimensions’ names and other cubes’ names with dimensions in the context, so you can reference these variables in the drill_members array.
これを是非やってほしいものなんだが、どうやってやるのかが辿れない。
ま、この程度なら自前スクリプトでちゃちゃっとやっちゃってもいいし、全カラム使えるとしないのであれば一つ一つ書き加えるのでもいい。


後はmetaが自由に設定できそうなのもいいね。dbtも同じだけど。


https://cube.dev/blog/introducing-a-drill-down-table-api-in-cubejs
Resultset.drillDownってのは、フロントエンドを実装するときに使うっぽいか？


うーんCubeでもできそうな気はしてきたっちゃあしてきたが、、、
結局RAGに突っ込むためのメタデータの用意的な使い方をするのであれば、別にSemantic layer製品を通す必要はなくて、メトリクス的なもの、dimension的なもの、そしてsemantic model間の関係性を示すもの、があれば十分なんだよな
まあでもjoinする場合はjoin keyをテーブル間の関係性のメタデータから持ってきたりとかちょこちょこケアはしないといけないだろうけど、、、

まあ製品だとsemantic model間の関係の可視化なんかはしてくれるけど、、、あとはアクセスコントロールなんかもしてくれるらしいが

んーまあcubeで検証してみてもいい
結局どういう風にmetadataを持つべきかの一種のべスプラを提供してくれているわけだし、やろうと思えばmetaでいくらでもいじれるし

ただ今からCubeの機能をまず検証して、それからLLMとの部分を実装するとなると、ちょっと2か月でいけるのかなという気はする。。。体勢次第か？
