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


게임서버에서 BQ에 로깅을 남기는 방법
・cloud logging에ops agent를 사용해서 전송
Google Cloud의 vm인스턴스에 Ops Agent를 설치 하여 사용하기 때문에
SandBox에서의 테스트는 어려울 것으로 보여진다.
때문에 Aiming사에게 Google Cloud의 권한을 받아 작업할 필요가 있다.

・BigQuery PHP Library를 사용
테스트용으로 준비한 BQ Sandbox_Nobollel에 PHP에서 라이브러리를 사용하여 데이터를 로깅한다.

・BigQuery API 사용
BigQuery API를 사용하여 삽입할 데이터를 사전에 json으로 가공하여 BigQuery API를 호출하고 데이터를 전송한다.




개발시 필요한 정보
개발 환경에 대해서 가장 베스트는 Aiming사로 부터 제공 받는 것이다.
・cloud logging에ops agent를 사용해서 전송하는 경우
Google Cloud의 vm인스턴스에 Ops Agent를 설치 하여 사용하기 때문에 SandBox에서의 테스트는 불가능 하기 때문에 