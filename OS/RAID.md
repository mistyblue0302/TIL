## RAID

RAID란 여러 디스크를 마치 하나의 저장 장치처럼 사용하는 기술로, 성능과 데이터 안전성을 높인다.

- 패리티란? 홀수 또는 짝수를 나타내는 속성으로 오류를 검사하기 위해 추가된 값

### RAID에서 사용하는 기술

- 스트라이핑 : 연속된 데이터를 일정한 크기로 나누어 여러 디스크에 번갈아 저장한다. 최소 두 개의 디스크가 필요하다.
- 미러링 : 데이터를 동일한 내용으로 복제하여 여러 디스크에 저장한다. 두 개의 디스크로 구현할 경우 하나의 디스크에 에러가 발생해도 다른 디스크의 데이터는 그대로 보존된다.

### RAID의 종류

![img](https://github.com/mistyblue0302/TIL/blob/main/Image/RAID.png)

![img](https://github.com/mistyblue0302/TIL/blob/main/Image/RAID2.png)

![img](https://github.com/mistyblue0302/TIL/blob/main/Image/RAID3.png)
