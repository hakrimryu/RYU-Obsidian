### 確定された作業
・基本KPIデータの取得
	- Firebase Analyticsとmonolith(BigQuery)の連携
・課金及び有償アイテム消費時のKPIデータ取得
	- ゲームサーバを通じて、monolith(BigQuery)を利用

開発協議 (作業方法により)
	作業リストに基づき、Aiming社とNobollel側の作業を分類する必要がある。
	ex) インフラ側のツールのインストール及び設定(Aiming)
		ログ時に必要なバックエンド作業 (Nobollel)

事前作業
BigQueryのデータセットのデータ(CSV)が必要。
KPIイベントリストを参照して作成
後でBigQueryにデータセットインポートはNobollel? Aiming?
ex) 
user_id (string)
product_id (string)
price (int)
store (string)

ゲームサーバからBigQueryにロギング、予想作業検討(4つの方法)
・共通作業
1. ロギング対象のAPI調査
2. BQのデータセット設計及び作成(csv)およびデータセットの生成 (事前作業)
3. 仕様及びロギングデータに基づくAPIの追加及び修正

・cloud loggingにops agentを利用して転送
1.   【AWS/ゲームサーバー】Google CloudのOps Agentインストール
2.   【AWS/ゲームサーバー】Ops Agent設定 (google-cloud-ops-agent/config.yaml)
3.   【AWS/ゲームサーバー】ログ設定 (ログパス追加)
4.   【Google Cloud】​サービスアカウント作成およびCloud Logging権限付与、その後アカウントキー(Json)生成
5.   【AWS/ゲームサーバー】生成したアカウントキー保存及び環境変数(パス)作成
6.   【AWS/ゲームサーバー】​Ops Agent再起動
7.   【Google Cloud】sinksを利用してCloud LoggingからBigQueryへログデータを転送
8.   【Google Cloud】sinksフィルタ設定
9.   【AWS/ゲームサーバー】Ops Agentを利用したログ関連スクリプト作成
	メリット ： 
		Google Cloudの標準ロギングツールを使用するため、安定性と互換性が保証。
		Cloud LoggingとBigQuery間の連動がスムーズ。
	デメリット : 
		初期設定がやや複雑で、AWSとGoogle Cloud間の連動が必要という点で追加作業が発生

・BigQuery PHP Libraryを使用
1.   【Google Cloud】サービスアカウント作成及びBigQuery権限付与後、アカウントキー(Json)作成  
2.   【AWS/ゲームサーバー】Composerを利用したBigQuery PHP Client Libraryをインストール。
3.   【AWS/ゲームサーバー】生成したアカウントキー保存及び環境変数(パス)作成
4.   【AWS/ゲームサーバー】BigQuery Libraryを利用してロギング関連スクリプトを作成
	メリット ： 
		直接的にBigQueryにデータを挿入することができ、中間過程や他のサービスを通さないため、シンプルで直感的。
	デメリット : 
		エラー処理、再試行ロジックなどを直接実装する必要がある。

・BigQuery REST APIの使用
1.   【Google Cloud】サービスアカウント作成及びBigQuery権限付与後、OAuth 2.0クライアントIDを作成します。
2.   【AWS/ゲームサーバー】Google API PHPクライアントライブラリをインストールします。
3.   【AWS/ゲームサーバー】OAuth 2.0認証を使用してアクセストークンを取得する
4.   【AWS/ゲームサーバー】REST APIを利用してロギング関連スクリプトを作成します。
	メリット ：
	直接的にBigQueryにデータを挿入することができ、中間過程や他のサービスを通さないため、シンプルで直感的。
	デメリット : API呼び出しに関連するエラーハンドリング、トークン管理、再試行ロジックなどを実装する必要がある。

・Firebase Analyticsを利用したロギングおよびBigQueryの送信
(Client側でFirebase AnalyticsとBigQueryの連携が完了していることを前提とする)
1.   【Unity】クライアント側でロギング対象のAPIを叩く。
2.   【AWS/게임서버】必要に応じてサーバー側でLog用データを作成する。
3.   【Unity】responseで成功値が返ってきたら、FirebaseAnalyticsを利用してロギングする。
	メリット : 
	クライアントで既に使用中のサービスを活用するため、追加作業が少ない。
	デメリット : 
	クライアントのロギングの場合、操作の可能性があるが、サーバー検証後にロギングを実行すれば、このリスクは大幅に減少する。
	
リンク
(KPIイベントリスト)
https://docs.google.com/spreadsheets/d/1tLgsojpTqspgkFo1fxYsQ1IgvLBk4s8kQRCzcyDBrUw/edit?pli=1#gid=1336877271
(BQ Sandbox_Nobollel テスト環境)
https://console.cloud.google.com/bigquery?project=bq-sandbox-402401&supportedpurview=project