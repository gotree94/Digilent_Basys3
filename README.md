# 🧩 Digilent Basys3 Verilog 기본 문법 정리

## 1️⃣ 자료형

### 📘 Net형 (연결선)
- **wire** : 논리적 동작 없이 단순 연결에 사용  
  ```verilog
  wire x;
  ```

### 📘 Register형 (값 저장)
- **reg** : 값을 임시로 저장할 수 있는 변수 (조합/순차회로에서 사용)  
  ```verilog
  reg [3:0] x1, y1, z1;
  ```

### 📘 논리값
| 값 | 의미 |
|:--:|:--|
| `0` | 논리적 0 / 거짓 (GND) |
| `1` | 논리적 1 / 참 (VCC) |
| `X` | 알 수 없는 상태 |
| `Z` | 하이임피던스 (Open / Floating) |

---

## 2️⃣ 파라미터(Parameter)

- 모듈 내 상수 정의에 사용 (비트 폭, 주소 범위 등)
- `parameter`, `localparam` 사용

```verilog
parameter AVG = (x + y) / 2;
```

- 모듈 매개변수화 예시
  ```verilog
  module example_module #(parameter WIDTH = 4) (...);
  module example_module #(.WIDTH(4)) (...);
  ```

---

## 3️⃣ 벡터(Vector)와 배열(Array)

### 📘 벡터
여러 비트로 구성된 비트열

```verilog
wire [3:0] bus_a;
assign bus_a = 4'hF;
```

### 📘 배열
벡터의 집합

```verilog
reg [7:0] array_2d [2:0];       // 2차원 배열
array_2d[2][3] = 8'h00;
```

---

## 4️⃣ 연산자

| 종류 | 연산자 | 예시 |
|------|---------|------|
| **산술** | `+`, `-`, `*`, `/`, `%` | `assign add = a + b;` |
| **논리** | `&&`, `||`, `!` | `if (a && b)` |
| **비트단위** | `~`, `&`, `|`, `^`, `^~`, `~^` | `(1011) ^ (0011) → (1000)` |
| **관계** | `==`, `!=`, `<`, `<=`, `>`, `>=` | `assign eq = (a==b);` |
| **시프트** | `<<`, `>>`, `<<<`, `>>>` | `0110_1001 >> 2 → 0001_1010` |
| **조건** | `? :` | `y = (a==b) ? a : b;` |
| **결합** | `{ }` | `{1'b1, 1'b0, 2'b01} → 1001` |
| **반복결합** | `{a{b}}` | `{2{01}} → 0101` |

---

## 5️⃣ Verilog의 추상화 수준 모델링

| 수준 | 설명 | 예시 |
|------|------|------|
| **게이트 수준** | 논리 게이트 직접 표현 | `and #(3,4) U0(out, in1, in2);` |
| **데이터플로우 수준** | 연산식 기반 연결 | `assign out = in1 & in2;` |
| **행위 수준** | 기능 중심 표현 (C언어 유사) | `always @(posedge clk)` |

---

## 6️⃣ 할당문

### 📘 연속 할당문 (Continuous Assignment)
- **조합회로용**
- `assign` 사용
- **좌변**: `net형`, **우변**: `net/reg`

```verilog
assign result = a & b;
```

### 📘 절차적 할당문 (Procedural Assignment)
- `always` 또는 `initial` 블록 안에서 사용
- **순차회로**: `<=` (non-blocking)
- **조합회로**: `=` (blocking)

| 구분 | 연산자 | 사용 대상 |
|------|--------|------------|
| 블록킹 | `=` | 조합회로 |
| 논블록킹 | `<=` | 순차회로 |

### ✅ 사용 지침
- 순차회로 → `<=`  
- 조합회로 → `=`  
- 한 `always`문 내에서 `=`와 `<=` 혼용 금지  
- 여러 `always`문에서 동일한 `reg` 사용 금지

---

## 7️⃣ Always / Initial 문

### 📘 always 문
- **합성 가능**
- 조합회로, 순차회로, 테스트벤치 등 다양한 용도
- 여러 개 동시에 실행 가능

```verilog
always @(posedge clk or negedge rst)
    if (!rst)
        q <= 0;
    else
        q <= d;
```

### 📘 initial 문
- 초기값 설정, 테스트용 (합성 불가)
- 여러 개 작성 가능, 병렬 실행됨

```verilog
initial begin
    a = 0; b = 1;
end
```

---

## 8️⃣ 구조적 설계 (Structural Design)

- 전체 시스템을 **모듈 단위로 분할**  
- 하위 모듈을 상위 모듈에 **인스턴스화**하여 연결

### 📘 위치 기반 포트 매핑
```verilog
adder U0 (a, b, sum);
```

### 📘 이름 기반 포트 매핑
```verilog
adder U0 (.a(x), .b(y), .sum(z));
```

---

## 9️⃣ 가독성과 효율적 설계 스타일

### 📘 코딩 스타일 가이드
- 일관된 들여쓰기
- 의미 있는 이름 사용
- 클록(`clk`) 및 리셋(`rst_n`) 명확히 표기
- 상수는 `parameter`로 정의
- 포트 선언 순서 정리 (입력→출력)
- 우선순위 없는 조건문 권장 (`case`, `if-else`)

### 📘 효율적 코딩 방법
- `always` 이벤트 목록에 필요한 신호만 포함
- 조건문 기반 조합회로 설계
- 시간 제어 포함 순차회로에서의 `posedge` / `negedge` 정확히 지정

---

✅ **Tip:**  
Basys3 실습 시에는 항상 `timescale`과 `default_nettype`을 명시하는 습관을 들이세요.

```verilog
`timescale 1ns / 1ps
`default_nettype none
```
