### 확정된 작업
・기본 KPI 데이터 취득
	- Firebase Analytics와monolith(BigQuery)의연계
・과금 및 유상젬 소비 시 KPI 데이터 취득
	- 게임서버를 통한 、monolith(BigQuery)의 이용

개발 협의 (작업 방식에 따라)
1. 작업 리스트에 따라 Aiming사와 Nobollel측 작업을 분류할 필요가 있다.
	ex) 인프라측 툴 설치및 설정(Aiming)
		로깅시 필요한 백엔드 작업 (Nobollel)

사전 작업
구매와 소비관련의 중요도가 높을 경우 게임서버를 통한 로깅 작업을 실시 하기로 하였기 때문에
이에 따른 BQ의 데이터셋의 데이터(CSV)가 필요하다 필요하다.
Nobollel의 플래너측에서 작성할지 Aiming에서 작성할지?
ex) 
user_id (string) : f80c947f71b14e0eb9879d8631373fc0
product_id (string) : com.aiming.pzlessdev.googleplay.0001
price (int) : 100 
store (string) : google_play


게임서버에서 BigQuery에 로깅 예상 작업 검토(4가지 방법)
・공통작업
1. 로깅 대상의 API조사
2. BQ의 데이터셋 설계 및 작성(csv)및 데이터셋 생성
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

・BigQuery PHP Library를 사용
1.   【Google Cloud】서비스 계정 생성 및 BigQuery권한 부여 후 어카운트 키(Json) 생성  
2.   【AWS/게임서버】Composer를 이용한 BigQuery PHP Client Library를 설치
3.   【AWS/게임서버】생성한 어카운트키 저장 및 환경 변수(경로) 작성
4.   【AWS/게임서버】BigQuery Library를 이용하여 로깅 관련 스크립트 작성

・BigQuery REST API 사용
1.   【Google Cloud】서비스 계정 생성 및 BigQuery권한 부여 후 OAuth 2.0 클라이언트 ID를 생성
2.   【AWS/게임서버】Google API PHP 클라이언트 라이브러리를 설치
3.   【AWS/게임서버】OAuth 2.0 인증을 사용하여 액세스 토큰을 취득
4.   【AWS/게임서버】REST API를 이용하여 로깅 관련 스크립트 작성

・Firebase Analytics를 이용한 로깅 및 BigQuery 전송
(Client측에서 Firebase Analytics와 BigQuery 연동 작업이 되었다는 것을 전제로 함)
1.   【Unity】클라이언트애서 로깅 대상 api를 두드린다.
2.   【AWS/게임서버】필요하에 서버측에서 Log용 데이터를 작성한다.
3.   【Unity】response로 성공 값이 돌아 오면 FirebaseAnalytics를 이용하여 로깅한다.


##링크
(KPI 이벤트 리스트)
https://docs.google.com/spreadsheets/d/1tLgsojpTqspgkFo1fxYsQ1IgvLBk4s8kQRCzcyDBrUw/edit?pli=1#gid=1336877271
(BQ Sandbox_Nobollel 테스트환경)
https://console.cloud.google.com/bigquery?project=bq-sandbox-402401&supportedpurview=project