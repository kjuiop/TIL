# Pagination 구현 방법의 종류

- offset 과 limit 예약어를 통하여 select 의 전체 결과 중 일부만 가져오는 방법이다.
- Cursor 방식 : cursor 는 어떠한 레코드를 가리키는 포인터이고, 이 cursor 가 가리키는 레코드부터 일정 갯수만큼 가져오는 방식이다. Seek Method, Keyset Pagination 이라고도 한다.
- limit 와 offset 은 처음부터 모든 값을 우선 가져와서 임시 테이블에 저장한 다음 필요한 만큼만 반환하고 나머지는 버리기 때문이다.
- offset 방식은 데이터의 잦은 추가와 삭제가 있을 경우 데이터의 중복과 누락이 발생할 수 있다.