## **Statement와 PreparedStatement**

SQL문을 실행할 수 있는 객체로, 가장 큰 차이점은 **캐시 사용 여부**이다.

SQL은 쿼리 문장 분석 -> 컴파일 -> 실행 단계를 거친다.

Statement는 쿼리 수행때마다 쿼리 문장 분석과 컴파일 단계를 거치게 되고, PreparedStatement는 처음 한 번만 컴파일 단계를 거친 후 캐시에 담아 재사용 한다. 동일한 쿼리를 반복적으로 수행하는 경우 PrepardStatement가 DB에 훨씬 적은 부하를 주며, 성능도 좋다.