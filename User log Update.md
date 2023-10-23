유저의 로그에 대해서는 클라이언트의 UHistory에서 취득 하고 있다.
이는 24시간에 한번(예정) Json데이터로 S3에 파일로써 저장한다.
(업로드 로직은 현재 실장 되어 있지 않음)

때문에 실시간 Log취득은 불가능하고 현재 UHistory에는 젬의 사용에 대한 로그는 존재 하지 않는다.

이번 추가및 수정 되는 Log는 유상, 무상 젬 사용에 따른
w_use_paid_money_log의 추가와 w_gacha_log의 개수이다.

가챠
현재 테이블 구조

|gacha_log_id|user_id|card_id|created_at|
|---|---|---|---|
|varchar(32)|varchar(32)|int|created_at|

통화사용(유무상다이아)

|used_diamond_log_id|user_id|is_paid|use_paid_money_type_id|amount_used|created_at|
|---|---|---|---|---|---|
|varchar(32)|varchar(32)|int|int|int|created_at|

use_paid_money_type

|use_paid_money_type_id|type|
|---|---|
|int|varchar(32)|
現、Shop・Gacha・Fever

법률을 때문에 유료다이아의 경우 전우케킨 이기 때문에 이것을 생각할 필가 있음


타스크 작성하자.
