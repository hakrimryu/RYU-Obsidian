사전 준비 및 검토 사항.
Aiming사와 본 자료를 바탕으로 사전 미팅이 필용 할 것으로 생각 된다.

확정된 작업
1. Firebase Analyticsとmonolith(BigQuery)の連携
・일반적인 사항
3. サーバーを通じた、monolith(BigQuery)の使用
・구매와 소비 관련의 중요도가 높은 경우
(KPI イベントリスト)
https://docs.google.com/spreadsheets/d/1tLgsojpTqspgkFo1fxYsQ1IgvLBk4s8kQRCzcyDBrUw/edit?pli=1#gid=1336877271

(테스트환경)
BQ Sandbox_Nobollel
https://console.cloud.google.com/bigquery?project=bq-sandbox-402401&supportedpurview=project


개발 협의
1. 작업 리스트에 따라 Aiming사와 Nobollel측 작업을 분류할 필요가 있다.
	ex) 인프라측 툴 설치및 설정(Aiming)
	   로깅시 필요한 백엔드 작업 (Nobollel)

3. 구매와 소비관련의 중요도가 높을 경우 게임서버를 통한 로깅 작업을 실시 하기로 하였기 때문에
이에 따른 BQ의 데이터셋의 데이터(CSV)가 필요하다 필요하다.
Nobollel의 플래너측에서 작성할지 Aiming에서 작성할지?
ex) 
user_id (string) : f80c947f71b14e0eb9879d8631373fc0
product_id (string) : com.aiming.pzlessdev.googleplay.0001
price (int) : 100 
store (string) : google_play


게임서버에서 BQ에 로깅작업의 검토
・cloud logging에ops agent를 사용해서 전송
(この方法が一番柔軟性が高くて、Aiming側で好きに対応できる一般的な方法)
1. 게임서버 인프라(AWS)에 Google Cloud의 Ops Agent설치
2. Ops Agent 설정 (google-cloud-ops-agent/config.yaml)
3. 백엔드 로그 설정 (로그 경로 추가)
4. Google Cloud에서 서비스 계정 생성 및 Cloud Logging권한 부여및 어카운트 키(Json) 생성
5. 게임서버 인프라(AWS)에 5에서 취득한 키 저장 및 환경 변수(경로) 작성
6. Ops Agent재시작
7. sinks를 이용하여 Cloud Logging에서 BigQuery로 로그 데이터를 전송
8. sinks 필터 설정
9. 공통작업 실시
10. Ops Agent를 이용한 로깅 관련 스크립트 작성

・BigQuery PHP Library를 사용
1. 게임서버에 인프라(AWS)에 Composer를 사용하여 BigQuery PHP Client Library를 설치
2. Google Cloud에서 서비스 계정 생성 및 BigQuery권한 부여및 어카운트 키(Json) 생성
3. 게임서버 인프라(AWS)에 5에서 취득한 키 저장 및 환경 변수(경로) 작성
4. 공통작업 실시
5. BigQuery Library를 이용하여 로깅 관련 스크립트 작성

・BigQuery REST API 사용
BigQuery API를 사용하여 삽입할 데이터를 사전에 json으로 가공하여 BigQuery API를 호출하고 데이터를 전송한다.
1. Google Cloud에서 서비스 계정 생성 및 BigQuery권한 부여및 OAuth 2.0 클라이언트 ID를 생성
2. Google의 OAuth 2.0 인증을 사용하여 액세스 토큰을 취득
3. 공통작업 실시
4. REST API를 이용하여 로깅 관련 스크립트 작성
