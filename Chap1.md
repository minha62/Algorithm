# Chap1. 알고리즘 분석


## Outline

1.1 실행시간

1.2 의사코드

1.3 실행시간 측정과 표기

1.4 전형적인 함수들의 증가율

1.5 알아야 할 수학적 배경

1.6 응용문제

---

### 알고리즘 분석

- 알고리즘 : 주어진 문제를 유한한 시간 내에 해결하는 단계적 절차
- 데이터구조 : 데이터를 조직하고 접근하는 체계적 방식

=⇒ "좋은" 알고리즘과 데이터구조를 설계하는 것이 목표

- "좋은"의 척도
    - 알고리즘과 데이터구조 작업에 소요되는 실행시간 ⇒ 시간복잡도
    - 기억장소 사용량 ⇒ 공간복잡도


### 실행시간(running time)

- 알고리즘은 입력을 출력으로 변환
- 알고리즘의 실행시간은 입력의 크기와 함께 성장
- 평균실행시간(average case running time)이 아닌 최악실행시간(worst case running time)에 집중 ⇒ 분석이 비교적 용이


### 실행시간 구하기: 실험적 방법 vs 이론적 방법

### 1. 실험적 방법

- 알고리즘을 구현하는 프로그램을 작성
- 시스템콜을 사용하여 실제 실행시간을 정확히 측정
- 결과를 도표로 작성

    ### 실험의 한계

    - 실험 결과는 실험에 포함되지 않은 입력에 대한 실행시간을 제대로 반영하지 않을 수도 있음
    - 두 알고리즘을 비교하기 위해선, 동일한 하드웨어와 소프트웨어 환경 필요
        - HW : processor, clock rate, memory, disk  ....
        - SW : OS, programming language, compiler  .....
    - 알고리즘을 완전한 프로그램으로 구현해야 함

### 2. 이론적 방법

- 모든 입력 가능성을 고려
- 하드웨어나 소프트웨어와 무관하게 알고리즘 속도 평가 가능
    - 실행시간을 입력 크기, n의 함수로 규정
- 알고리즘을 구현한 프로그램 대신, 고급언어인 의사코드로 표현된 알고리즘을 사용


---


### 알고리즘의 정의와 조건

- 입력(well defined input) : 모호하지 않고 잘 정의된 0개 이상의 입력
- 출력(well defined output) : 명확하게 정의된 1개 이상의 출력
- 명확성(clear and unambiguous) : 모호하지 않고 명확한 명령어
- 유한성(finite) : 한정된 수의 단계 후 반드시 종료. 무한루프(X)
- 유효성(feasible) : 명령어들은 현재 실행 가능한 연산이어야 함


### 알고리즘의 기술 방법

- 자연어
- 흐름도(Flowchart)
- 의사코드(pseudo-code)
- 프로그래밍 언어(C, Python...)


### 의사코드(pseudo-code)

: 알고리즘을 설명하기 위한 고급언어

- 컴퓨터가 아닌, 인간에게 읽히기 위해 작성됨
- 저급의 상세 구현 내용이 아닌, 고급 개념을 소통하기 위해 작성됨
- 자연어 문장보다 더 구조적, 프로그래밍 언어보다 덜 상세
- 알고리즘을 설명에 선호되는 표기법


### 의사코드 문법

### 임의접근기계 모델

### 원시작업

### 원시작업 수 세기

***