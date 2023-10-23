유저의 로그에 대해서는 클라이언트의 UHistory에서 취득 하고 있다.
이는 24시간에 한번(예정) Json데이터로 S3에 파일로써 저장한다.
(업로드 로직은 현재 실장 되어 있지 않음)

때문에 실시간 Log취득은 불가능하고 현재 UHistory에는 젬의 사용에 대한 로그는 존재 하지 않는다.

이번 추가및 수정 되는 Log는 유상, 무상 젬 사용에 따른
w_use_paid_money_log의 추가와 w_gacha_log의 개수이다.




#### ガチャ
現、テーブル構造

|gacha_log_id|user_id|card_id|created_at|
|---|---|---|---|
|varchar(32)|varchar(32)|int|created_at|

#### 通貨使用(有・無償ダイヤ)

|used_diamond_log_id|user_id|is_paid|use_paid_money_type_id|amount_used|created_at|
|---|---|---|---|---|---|
|varchar(32)|varchar(32)|int|int|int|created_at|

use_paid_money_type

|use_paid_money_type_id|type|
|---|---|
|int|varchar(32)|
現、Shop・Gacha・Fever

패턴이 너무 많기 때문에 use_paid_money_type_id를 취득이 아니라
Json타입으로 이력을 남길 필요가 있다.

클라이언트에서 위의 데이터를 넘길때
json으로 가공한 log_data를 넘겨 받아서 저장하자.
log데이터는 추후 KPI, 로그 툴 등에서 가공하여 처리 해야 한다.

json데이터 설계

shop:

Gacha:

Fever:



법률을 때문에 유료다이아의 경우 前受金 이기 때문에 이것을 생각할 필가 있음
타스크 작성하자.

ガチャ와 通貨使用 나눈다.

통화는 다이아만 대상으로 한다.
리얼머니 -> 다이아 이기 때문에

서버가 무료 다이아에 대한 커런트치를 가질 필요가 있을까? (이것에 대해서 공수가 크게 차이남)




데이터 베이스에 로그를 남기려고 하는데
남기고 싶은 컬럼이 조금씩 다르기 때문에 문제가 발생했다.
문제에 대해서 해결책으로는 log_data컬럼에 json으로 각 형식으로 가공해서 넣으려고 한다.
이렇게 되면 추후 관리툴에서 log를 필터링 해서 확인 할 경우
전체의 로그를 취득 후에 값을 찾아서 보여줘야 하기 때문에 비효율일꺼 같다.
다른 방법으로는 각 형식을 json에 저장하는 것이 아니라  각 형식의 log용 테이블을 각각 작성하고
본 컬럼은 log_data가 아닌 각 형식의 log_id를 넣고 다른 테이블에서 관리 하는 방법이다.

때문에
각 패턴의 로그용 테이블을 작성하고 사용하는 방법으로 하자.


유상통화 로깅