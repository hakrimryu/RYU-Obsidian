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
3. 사양및 로깅 데이터에 따른 API추가 및 수정

・cloud logging에ops agent를 사용해서 전송
1.   【AWS/게임서버】Google Cloud의 Ops Agent설치
2.   【AWS/게임서버】Ops Agent 설정 (google-cloud-ops-agent/config.yaml)
3.   【AWS/게임서버】로그 설정 (로그 경로 추가)
4.   【Google Cloud】서비스 계정 생성 및 Cloud Logging권한 부여 후 어카운트 키(Json) 생성
5.   【AWS/게임서버】생성한 어카운트키 저장 및 환경 변수(경로) 작성
6.   【AWS/게임서버】Ops Agent재시작
7.   【Google Cloud】sinks를 이용하여 Cloud Logging에서 BigQuery로 로그 데이터를 전송
8.   【Google Cloud】sinks 필터 설정
9.   【AWS/게임서버】Ops Agent를 이용한 로깅 관련 스크립트 작성
	장점 : 
	Google Cloud의 표준 로깅 도구를 사용하므로 안정성과 호환성이 보장
	Cloud Logging과 BigQuery 간의 연동이 원활
	단점 : 초기 설정이 다소 복잡하며, AWS와 Google Cloud 간의 연동이 필요하다는 점에서 추가 작업이 발생

・BigQuery PHP Library를 사용
1.   【Google Cloud】서비스 계정 생성 및 BigQuery권한 부여 후 어카운트 키(Json) 생성  
2.   【AWS/게임서버】Composer를 이용한 BigQuery PHP Client Library를 설치
3.   【AWS/게임서버】생성한 어카운트키 저장 및 환경 변수(경로) 작성
4.   【AWS/게임서버】BigQuery Library를 이용하여 로깅 관련 스크립트 작성
	장점 : 직접적으로 BigQuery에 데이터를 삽입할 수 있어, 중간 과정이나 다른 서비스를 거치지 않기 때문에 단순하고 직관적
	단점 : 오류 핸들링, 재시도 로직 등을 직접 구현이 필요.

・BigQuery REST API 사용
1.   【Google Cloud】서비스 계정 생성 및 BigQuery권한 부여 후 OAuth 2.0 클라이언트 ID를 생성
2.   【AWS/게임서버】Google API PHP 클라이언트 라이브러리를 설치
3.   【AWS/게임서버】OAuth 2.0 인증을 사용하여 액세스 토큰을 취득
4.   【AWS/게임서버】REST API를 이용하여 로깅 관련 스크립트 작성
	장점 : 직접적으로 BigQuery에 데이터를 삽입할 수 있어, 중간 과정이나 다른 서비스를 거치지 않기 때문에 단순하고 직관적
	단점 : API 호출과 관련된 에러 핸들링, 토큰 관리, 재시도 로직 등을 구현이 필요.

・Firebase Analytics를 이용한 로깅 및 BigQuery 전송
(Client측에서 Firebase Analytics와 BigQuery 연동 작업이 되었다는 것을 전제로 함)
1.   【Unity】클라이언트애서 로깅 대상 api를 두드린다.
2.   【AWS/게임서버】필요하에 서버측에서 Log용 데이터를 작성한다.
3.   【Unity】response로 성공 값이 돌아 오면 FirebaseAnalytics를 이용하여 로깅한다.
	장점 : 클라이언트에서 이미 사용 중인 서비스를 활용하기 때문에 추가적인 작업이 적다.
	단점 : 클라이언트 로깅의 경우 조작 가능성이 있으나, 서버 검증 후 로깅을 수행하면 이 위험성은 크게 줄어든다.

링크
(KPI 이벤트 리스트)
https://docs.google.com/spreadsheets/d/1tLgsojpTqspgkFo1fxYsQ1IgvLBk4s8kQRCzcyDBrUw/edit?pli=1#gid=1336877271
(BQ Sandbox_Nobollel 테스트환경)
https://console.cloud.google.com/bigquery?project=bq-sandbox-402401&supportedpurview=project