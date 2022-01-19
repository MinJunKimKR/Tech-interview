# About Project

## Nest

## Hasura

## Postgresql

### Mysql과의 차이점

1. MVCC의 방식이 다르다
   1. Mysql은 undo log를 사용하여 영역에 이전 데이터를 저장하는 방법을 사용한다. - Undo Segment
   2. Postgresql은 기존의 데이터는 삭제 마킹을하고, 새로운 버전의 데이터를 추가한다. - MGA
2. Mysql은 JSONB를 지원하지 않는다
3. Update방식이 다르다
   1. Mysql은 update를 바로 한다.
   2. postgresql은 delete를 한뒤에 insert하는 방식으로 update를 진행한다.

## 애자일

### 워터폴과의 차이점
