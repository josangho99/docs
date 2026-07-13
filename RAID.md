# RAID
## RAID란?
- Redundant Array of Independent Disk or Redundant Array of Inexpensive Disk
- 여러 개의 디스크를 묶어 하나의 디스크처럼 사용하는 기술

### 주요 용어
1. 스트라이핑(Striping)
- 데이터를 여러 개의 디스크에 나눠 기록하여 데이터 입출력 속도를 높임.
2. 미러링(Mirroring)
- 원본 디스크와 같은 크기의 백업 디스크를 두어 동일한 데이터를 동시에 저장하고 한 디스크가 고장 났을 때, 다른 디스크를 사용하여 데이터를 복구하는 것.

## RAID 단계별 정리

* 자주 사용하는 RAID 방식만 정리

### RAID0(Striping)
- 모든 디스크에 데이터를 나눠서 입출력 하도록 구성
- 최소 2개의 디스크 필요
- 용량 N배, 성능[읽기+쓰기] N배
- 단 리스크도 N배, 1개의 디스크만 고장나도 RAID로 묶은 디스크 데이터 모두 사용 불가

### RAID1(Mirroring)
- 모든 디스크에 동일한 데이터를 저장
- 최소 2개의 디스크 필요[비용 문제로 대부분 2개만 사용하며 OS에 주로 사용]
- 쓰기는 느림, 읽기는 N배, 용량은 1/N
- 쓰기의 경우 컨트롤러를 사용하는지, 소프트웨어(ex.IntelVMD)를 사용하느냐에 따라 다르기에 정확한 수치화 불가
- 리스크는 1/N, 1개만 살아있어도 rebuild를 통해 사용 가능

### RAID4
- block 단위로 Striping을 하고, 데이터 복구를 위한 1개의 패리티 코드를 1개의 디스크에 저장
- 최소 3개의 디스크
- 용량 및 성능이 N-1배 증가
- 1개의 디스크 에러시 복구 가능
- 패리티 코드를 하나의 디스크에 저장하기에 해당 디스크 과부화로 인한 수명이 줄어듦
- 위 단점 해결을 위해 RAID5 방식으로 변경

### RAID5
- 가장 많이 사용됨
- block 단위로 Striping을 하고, 데이터 복구를 위한 1개의 패리티 코드를 모든 디스크에 분할 저장함
- 패리티는 단순 XOR 연산으로 생성
- 최소 3개의 디스크
- 용량 및 성능이 N-1배 증가
- 1개의 디스크 에러시 복구 가능[2개 이상은 불가]
- RAID 0에서 성능, 용량을 조금 줄이고 안정성을 높인 RAID Level
- HotSpare 디스크를 추가 지정하여 리스크 관리 가능(기존 디스크가 고장나지 않는 이상 대기 상태라 비용이 증가)

### RAID6
- block 단위로 Striping을 하고, 데이터 복구를 위한 2개의 패리티 코드를 모든 디스크에 분할 저장함
- 1개의 패리티는 단순 XOR연산으로, 1개는 보다 복잡한 알고리즘(ex. Reed Solomon code)으로 생성
- 최소 4개의 디스크
- 용량 및 성능은 N-2배 증가
- 2개의 디스크 에러시 복구 가능[3개 이상은 불가]
- RAID5에서 성능, 용량을 줄이고 안정성을 높인 RAID Level

## 중첩 RAID(Nested RAID)
### RAID01(0+1)
- RAID 0으로 묶인 디스크들을 1로 묶음
- 극단적으로 0으로 묶인 1개의 그룹만 정상이어도 사용 가능

### RAID10(1+0)
- RAID 1로 묶인 디스크들을 0으로 묶음
- 극단적으로 1로 묶인 각 그룹별 디스크가 1개씩만 정상이어도 사용 가능

## Write Policy
### Write Back
- 캐시 메모리에만 저장 후 저장 성공 콜백을 보내주고 디스크에 데이터를 기록하는 방식
- 기본 값으로 사용됨.

### Always Write Back
- BBU 유뮤와 관계없이 항상 캐시 메모리에만 저장 후 저장 성공 콜백을 보내주고 디스크에 데이터를 기록하는 방식
- 저장 콜백을 빠르게 주기에 다음 작업 수행이 원활하며 주로 속도가 느린 HDD에 사용

### Write Through
- 디스크에 데이터를 저장 후 저장 성공 콜백을 보내주는 방식
- 데이터 무결성을 완벽히 유지할 수 있다는 장점이 존재
- 단, 디스크에 입력 완료 후 다음 작업 수행이 가능하기에 속도가 느린 SATA용 HDD에는 권장하지 않음(SAS의 경우 상황에 따르나, 대부분 Write Back 혹은 Always Write back에 사용)

## 기타

### OEM Vender Parts
- 제조사에 싸게 납품하기 위해 만든 제품, 단 라이선스 및 기능 제한을 둠
- 제한을 풀려면 추가 비용을 지불하여 새로운 라이선스 키를 받아야 함

### 참고링크
:link: [RAID 정리 1. RAID 기본 설명 및 RAID Level][RAID-devocean-Link] <br>
:link: [RAID 란?, RAID 구성방식][RAID-velog-Link] <br>
:link: [Reed-Solomon 이란?][Reed-Solomon-Link]

[RAID-devocean-Link]: https://devocean.sk.com/blog/techBoardDetail.do?ID=163608
[RAID-velog-Link]: https://velog.io/@zxcvbnm5288/RAID-%EB%9E%80-RAID-%EA%B5%AC%EC%84%B1%EB%B0%A9%EC%8B%9DRAID-0-1-4-5-6-10-01
[Reed-Solomon-Link]: https://velog.io/@croc100/Reed-Solomon-%EC%9D%B4%EB%9E%80
