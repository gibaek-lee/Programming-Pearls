# Programming-Pearls
- Book Summary: Programming Thinking about below things
	1. (이하 1부)Problem Understanding
	2. Select Data Structure
	3. Algorithm Design
	4. Test Code Development
	5. Debugging
	6. (2부)Performance Design Level
	7. (3부)Practical Programming Process
	
- Reference: Programming Pearls. Jon Bentley. 2/E.

## 1부. 준비
### 칼럼1. 조개껍질깨기
  1. 세부적인 요구사항 파악
  2. 정확한 문제 정의

### 칼럼3. 프로그램의 구조를 결정하는 데이터
  1. 입출력 데이터 파악
  2. 효율적 데이터 구조 디자인 
  3. 데이터 구조에 기반한 프로그램구조(비즈니스 로직, 알고리즘 디자인) 결정

### 칼럼2. 아하! 알고리즘
  - 데이터 구조에 기반한 비즈니스로직, 알고리즘 디자인

### 칼럼4. 정확한 프로그램 작성
  - 함수검증 가상코드 작성: 코드 + 스캐폴딩(assert 활용한 불변식 단정문)
  - 함수검증이란? 함수 내에 아래 세 개의 스캐폴딩 작성
    1. 초기화: 초기 경계조건 불변식 참
    2. 보존: 순차구조와 선택구조(if)로 이루어진 반복구조(for/while) 안에서의 불변식 유지
    3. 종료: 결과값 검증 불변식으로 끝 경계조건 참

### 칼럼5. 프로그래밍에서의 사소한(?) 문제
  - First. 실제코드작성
  - Second. 신뢰도 확보를 위한 자동화 테스트 코드 작성
    1. 알고리즘 오류 검증 테스트: 칼럼4의 '함수 + 검증코드'를 테스트하는 코드 작성
    2. 알고리즘 유효성 검증 테스트: 칼럼4의 '함수 + 검증코드'의 예상 복잡도를 Big O법으로 추정 > input의 갯수를 늘리며 함수 실행시간 확인해보는 코드 작성 > Big O 추정치와 일치하는지 점검
  - Third. 관찰과 질문을 기반으로 한 디버깅

<p align="center">
  <img width="30%" src="Programming-Pearls.png" />
</p>

## 2부. 퍼포먼스
### 칼럼6. 퍼포먼스에 대한 개관
  - 디자인 수준: 최소 노력으로 최대 효과 만드는 디자인 수준 선택해서 퍼포먼스 개선하기
  
  - 효율성에 대한 고수준 디자인 접근
    1. 문제 재정의(칼럼7)
    2. 시스템 구조 재작성(칼럼8): 여러 모듈로 나누기, divide and conquer
    3. 데이터구조와 알고리즘 튜닝(칼럼8)
  
  - 효율성에 대한 저수준 디자인 접근: 고수준에서 다 접근해보고 정말 이정도까지 최적화가 필요하다 느껴지면 시도하기. 시스템 프로파일로 hot spot(비용 높은 연산/함수) 찾기
    4. 코드 튜닝: cpu 연산시간 절약(칼럼9), 메모리 절약(칼럼10)
    5. 시스템 소프트웨어(DB, 운영체제, 컴파일러, 언어 등 교체 검토)
    6. 하드웨어 구입
		
  - Q/A. 효율성 vs 정확성
    1. 초기개발단계: 효율성 < 정확성
    2. 유지보수단계: 효율성(퍼포먼스개선) vs 정확성(버그수정) 중 한정된 시간과 비용 내에서 최선의 선택을 해야한다.

### 칼럼7. 봉투 뒷면에 하는 간단한 계산
  - 기본기법
    1. 추상화 계산, 역 계산: 상식적인 값이 나와야 한다.
	  2. 차원 테스트: 추정치가 물리적으로 맞는 차원을 가지는지 확인
	  3. Rules of Thumb(“72의 법칙”): 실행시간 대략적인 추정
    
  - 퍼포먼스 추정: 메모리 고려(레코드 하나의 크기 + overhead 고려)

### 칼럼8. 알고리즘 디자인 기법
  - 알고리즘 통찰(간단한 프로그램으로 진화 : 복잡도, 계산시간 감소)
	  1. Divide and Conquer: O(n)을 O(log n)으로 할 수 있는지 고려
	  2. Cashing 활용한 Dynamic Programming: 앞선 계산결과 저장하고 이후 활용(메모리 사용하지만 계산시간 이득 <-> 계산 자체가 자주 발생하고 무거운 계산이면 비추)
	  3. 필요없는 loop 없애기: O(n)을 O(C)로 바꿀 수 있는지 고려
	  4. 정보를 사전처리해서 데이터구조로 보관, 활용: ii) Cashing을 정적으로 활용
	  5. Scanning 알고리즘: f[0, …, i-1]을 f[0, …, i]로 확장하는 알고리즘 고민, ii) Cashing 활용하기
	  6. 누적: iv) 데이터구조 활용
	  7. 하한: 최소 복잡도를 정해놓기

### 칼럼9. 코드 튜닝
  - cpu 연산시간 절약. 사소한 최적화이므로 최대한 미루기. 구조(정확성/유지보수/기능성) ⇔ 최적화(효율) 사이의 균형잡기 이슈
  - 방법
    1. % 연산을 minus(-) 연산으로
    2. 함수, 매크로, 인라인 코드 활용
    3. loop 펼치기: pipeline 지연 피하기/branch 분기 감소/명령어 수준의 병렬처리 가능 => 프로세서 병목현상 제거
    4. 데이터 표현법 좌표계 변경: 구면좌표계 ⇔ Cartesian, 부가 메모리 사용하는 대신 계산시간에 이득

### 칼럼10. 메모리 절약
  - 절약(빠른 로드/캐시에 적합/데이터 전송시간 감소로 네트워크에 좋음) => 단순화 => 새로운 통찰
  - 방법
	  1. 저장 대신 다시 계산 ⇔  단, 웹에서는 이 반대가 낫다(유저 컴퓨터에서의 캐시를 적극 활용할 수 있으므로)
	  2. 중복 데이터 구조 단순화: 대칭성 혹은 규칙을 찾기
	  3. 코드 자체 공간 줄이기: 함수 정의 => 인터프리터 사용 => 기계어 사용

## 3부. 프로덕트
### 칼럼12. 표본 선정 문제
실전 프로그래밍 프로세스
  - First. 발견된 문제를 이해하라(칼럼1.a): 문제가 발생한 컨텍스트에 대해 사용자와 대화 나누기 > 솔루션을 찾는 데 도움되는 아이디어 얻어내기
  - Second. 추상적인 문제를 구체화 하라(칼럼1.b): 문제에 대해 간결하고 구체적인 설명을 해보기 > 문제 해결/응용에 도움됨
  - Third. 디자인 공간을 탐구/탐색 하라(칼럼2, 칼럼3, 칼럼4)
	  1. do not! ‘1분 생각하고 하루 동안 코드 구현’, but do! ‘한 시간 생각한 후 한 시간 동안 코드 작성’
    2. 2~3가지 공간을 추가로 탐색하기
	  3. 비정형적 고수준 언어로(문학적으로) 디자인 묘사 > 추상 데이터 구조 표현 > 가상코드로 제어 흐름 나타내기
  - Fourth. 한 솔루션을 구현하라(칼럼5)
	  1. (훨씬 우월한 한 가지 디자인 공간이 안보일시)디자인 공간 탐색에서 나온 상위 몇 가지를 빠르게 구현해보기
    2. 가장 강력한 오퍼레이션 한 가지를 선택하여 직관적 코드로 구현하기
