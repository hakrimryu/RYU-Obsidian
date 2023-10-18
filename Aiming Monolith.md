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
구매와 소비관련의 중요도가 높을 경우 게임서버를 통한 로깅 작업을 실시 하기로 하였기 때문에
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
1. Google Cloud VM 인스턴스 생성 (Aiming)
2. Ops Agent 설치 (Aiming)
3. Ops Agent 구성 (Aiming)
4. 백엔드 로그 설정




・BigQuery PHP Library를 사용
테스트용으로 준비한 BQ Sandbox_Nobollel에 PHP에서 라이브러리를 사용하여 데이터를 로깅한다.

・BigQuery API 사용
BigQuery API를 사용하여 삽입할 데이터를 사전에 json으로 가공하여 BigQuery API를 호출하고 데이터를 전송한다.ㅋ
