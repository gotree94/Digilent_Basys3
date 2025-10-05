# FSM

---
# FSM 예제 동작 개념 정리

## 1. Toggle FSM (입문) - 기본 2-상태 토글

### 📌 핵심 개념
- **상태**: OFF → ON → OFF (2개 상태)
- **입력**: 버튼
- **출력**: LED

### 🔄 동작 원리
1. 초기 상태는 OFF (LED 꺼짐)
2. 버튼을 누르면 → ON 상태로 전환 (LED 켜짐)
3. 다시 버튼을 누르면 → OFF 상태로 전환 (LED 꺼짐)
4. 버튼 엣지 감지로 한 번만 토글되도록 처리

### 💡 학습 포인트
- FSM의 가장 기본 구조 (State Register, Next State Logic, Output Logic)
- 엣지 감지 (Edge Detection) 기법
- 버튼 디바운싱 개념

---

## 2. 시퀀스 FSM (입문) - 4-상태 LED 순차 점등

### 📌 핵심 개념
- **상태**: S0 → S1 → S2 → S3 → S0 (4개 상태)
- **입력**: enable 스위치
- **출력**: 4개의 LED

### 🔄 동작 원리
1. enable이 켜져 있을 때만 동작
2. 1초마다 상태가 자동으로 순환
3. S0: LED[0] 점등 → S1: LED[1] 점등 → S2: LED[2] 점등 → S3: LED[3] 점등
4. 100MHz 클럭을 1초로 분주하는 카운터 사용

### 💡 학습 포인트
- 타이머 기반 상태 전환
- 클럭 분주 (Clock Divider) 개념
- 순환 상태 머신 (Cyclic FSM)

---

## 3. 신호등 FSM (초급) - 3-상태 신호등 제어

### 📌 핵심 개념
- **상태**: RED → GREEN → YELLOW → RED (3개 상태)
- **입력**: enable 스위치
- **출력**: 빨강, 노랑, 초록 LED

### 🔄 동작 원리
1. RED 상태: 빨간불 5초 동안 켜짐
2. GREEN 상태: 초록불 5초 동안 켜짐
3. YELLOW 상태: 노란불 2초 동안 켜짐
4. 각 상태마다 다른 지속 시간 적용

### 💡 학습 포인트
- 상태별 다른 타이머 값 적용
- 함수(function)를 이용한 코드 간결화
- 실생활 응용 예제

---

## 4. 자판기 FSM (중급) - 6-상태 자판기

### 📌 핵심 개념
- **상태**: IDLE → COIN1 → COIN2 → COIN3 → COIN4 → COIN5 (6개 상태)
- **입력**: 100원 버튼, 500원 버튼, 취소 버튼
- **출력**: 음료 제공, 거스름돈, 7-segment 금액 표시

### 🔄 동작 원리
1. IDLE: 대기 상태 (0원)
2. 동전 투입 시 금액에 따라 상태 전환
3. 500원 이상 → 음료 제공 및 거스름돈 반환
4. 취소 버튼 → 투입 금액 전액 반환
5. 7-segment에 현재 투입 금액 표시

### 💡 학습 포인트
- 복수 입력 처리
- 조건부 상태 전환
- 7-segment 디코딩
- 거스름돈 계산 로직

---

## 5. UART 수신기 FSM (중상급) - 5-상태 UART 통신

### 📌 핵심 개념
- **상태**: IDLE → START_BIT → DATA_BITS → STOP_BIT → CLEANUP (5개 상태)
- **입력**: RX 신호
- **출력**: 8비트 데이터, valid 신호, error 신호

### 🔄 동작 원리
1. IDLE: RX 신호가 0으로 떨어지면 Start bit 감지
2. START_BIT: Start bit 중간점에서 샘플링하여 검증
3. DATA_BITS: 8비트 데이터를 LSB부터 순차적으로 수신
4. STOP_BIT: Stop bit 확인 (1이어야 정상)
5. CLEANUP: 수신 완료 후 IDLE로 복귀

### 💡 학습 포인트
- Baud rate 생성 (100MHz → 9600 baud)
- 신호 동기화 (메타스테이블 방지)
- 시프트 레지스터 사용
- Framing error 검출
- 통신 프로토콜 구현

---

## 6. 엘리베이터 FSM (상급) - 6-상태 엘리베이터 제어

### 📌 핵심 개념
- **상태**: IDLE → MOVING_UP/DOWN → DOOR_OPENING → DOOR_OPEN_WAIT → DOOR_CLOSING (6개 상태)
- **입력**: 4개 층 호출 버튼, 도어 센서
- **출력**: 모터 제어, 도어 제어, 현재 층 표시, 방향 LED

### 🔄 동작 원리
1. 요청 큐(request_queue)에 각 층 호출 저장
2. 목표 층까지 이동 (층당 2초 소요)
3. 도착 후 도어 개방 → 3초 대기 → 도어 폐쇄
4. 도어 센서 감지 시 재개방
5. 방향에 따라 효율적으로 층 방문

### 💡 학습 포인트
- 요청 큐 관리
- 복잡한 조건 분기
- 최적 경로 알고리즘
- 다중 출력 제어
- 안전 기능 구현 (도어 센서)

---

## 7. I2C Master FSM (고급) - 9-상태 I2C 통신

### 📌 핵심 개념
- **상태**: IDLE → START_COND → ADDR_SEND → ADDR_ACK → DATA_WR/RD → ACK → STOP_COND (9개 상태)
- **입력**: start, rw, slave_addr, wr_data
- **출력**: rd_data, busy, ack_error, SDA, SCL

### 🔄 동작 원리
1. START 조건: SCL이 HIGH일 때 SDA를 HIGH → LOW
2. 주소 전송: 7비트 슬레이브 주소 + R/W 비트
3. ACK 확인: 슬레이브가 SDA를 LOW로 당김
4. 데이터 전송/수신: 8비트 데이터 처리
5. STOP 조건: SCL이 HIGH일 때 SDA를 LOW → HIGH

### 💡 학습 포인트
- I2C 프로토콜 상세 구현
- 양방향(inout) 신호 제어
- Quarter clock 분주 (4단계 타이밍)
- ACK/NACK 처리
- 버스 충돌 방지

---

## 8. 게임 FSM (최고급) - 9-상태 반응속도 게임

### 📌 핵심 개념
- **상태**: IDLE → READY → WAIT_RANDOM → LED_ON → MEASURING → SUCCESS/FAIL → GAME_OVER (9개 상태)
- **입력**: start_btn, react_btn
- **출력**: 16개 LED 패턴, 4자리 7-segment, 부저

### 🔄 동작 원리
1. 게임 시작 후 랜덤 시간 대기 (1~3초)
2. LED가 켜지면 빠르게 버튼 클릭
3. 반응 시간 측정 (1ms 단위)
4. 점수 계산: 200ms 미만(3점), 400ms 미만(2점), 그 외(1점)
5. 5라운드 진행 후 최종 점수 표시

### 💡 학습 포인트
- LFSR(Linear Feedback Shift Register) 난수 생성
- 1ms 정밀 타이머 구현
- 동적 점수 계산 로직
- 멀티 라운드 관리
- 4자리 7-segment 제어
- 사운드 피드백 (부저)
- 복합적인 게임 로직 구현

---

## 📊 난이도별 핵심 기술 비교

| 예제 | 상태 수 | 핵심 기술 | 난이도 |
|------|---------|-----------|--------|
| Toggle | 2 | 엣지 감지, 기본 FSM | ⭐ |
| 시퀀스 | 4 | 타이머, 클럭 분주 | ⭐ |
| 신호등 | 3 | 상태별 타이머 | ⭐⭐ |
| 자판기 | 6 | 복수 입력, 조건 분기 | ⭐⭐⭐ |
| UART | 5 | 통신 프로토콜, 동기화 | ⭐⭐⭐⭐ |
| 엘리베이터 | 6 | 요청 큐, 최적화 | ⭐⭐⭐⭐ |
| I2C | 9 | 양방향 제어, 정밀 타이밍 | ⭐⭐⭐⭐⭐ |
| 게임 | 9 | LFSR, 복합 로직 | ⭐⭐⭐⭐⭐ |

---

## 🎓 학습 로드맵

### 1단계: 기초 (예제 1-2)
- FSM 기본 구조 이해
- 상태 전환 메커니즘
- 타이머 사용법

### 2단계: 응용 (예제 3-4)
- 실생활 문제 해결
- 복수 입력 처리
- 조건부 로직

### 3단계: 통신 (예제 5, 7)
- 표준 프로토콜 구현
- 타이밍 정밀 제어
- 에러 처리

### 4단계: 고급 제어 (예제 6, 8)
- 복잡한 알고리즘
- 다중 기능 통합
- 최적화 기법

---

## 💻 Basys3 보드 활용 팁

### 공통 리소스
- **클럭**: 100MHz 시스템 클럭 사용
- **리셋**: 중앙 버튼 (U18)
- **LED**: 16개 LED 활용 가능
- **7-segment**: 4자리 디스플레이
- **스위치**: 16개 스위치
- **버튼**: 5개 버튼

### 제약 파일(XDC) 설정 필수
각 예제를 Basys3에서 실행하려면 적절한 핀 할당이 필요합니다.

---

## 🔧 디버깅 팁

1. **시뮬레이션 먼저**: 각 예제를 먼저 시뮬레이션으로 검증
2. **LED 활용**: 상태를 LED로 표시하여 디버깅
3. **타이머 축소**: 시뮬레이션 시 카운터 값을 줄여서 테스트
4. **단계별 테스트**: 복잡한 FSM은 부분별로 나눠서 검증

---

## 🎯 다음 단계

이 8개 예제를 마스터하면:
- ✅ FSM 설계 완벽 이해
- ✅ 실전 프로젝트 수행 가능
- ✅ SPI, CAN 등 다른 프로토콜 확장 가능
- ✅ CPU, 메모리 컨트롤러 등 복잡한 시스템 설계 준비 완료

---

## 1. Toggle FSM (입문)

**📋 테스트 시나리오
***✅ 포함된 테스트
  * TEST 1: 첫 번째 버튼 누름 → LED 켜짐 확인
  * TEST 2: 두 번째 버튼 누름 → LED 꺼짐 확인
  * TEST 3: 세 번째 버튼 누름 → LED 다시 켜짐 확인
  * TEST 4: 버튼 길게 누르기 → 한 번만 토글 확인
  * TEST 5: 리셋 기능 → LED 초기화 확인
  * TEST 6: 빠른 연속 버튼 누름 → 디바운싱 확인

** 🔧 시뮬레이션 실행 방법
```bash
# Vivado 시뮬레이터
xvlog toggle_fsm.v
xvlog tb_toggle_fsm.v
xelab -debug typical tb_toggle_fsm -s sim
xsim sim -gui

# 또는 ModelSim
vlog toggle_fsm.v tb_toggle_fsm.v
vsim tb_toggle_fsm
run -all
```
** 📊 예상 결과
  * 각 테스트마다 PASSED/FAILED 메시지 출력
  * $monitor로 모든 신호 변화 실시간 출력
  * VCD 파일 생성으로 파형 분석 가능

```verilog
// ========================================
// 1번 - Toggle FSM (입문)
// ========================================
// 버튼을 누르면 LED가 켜지고 꺼지는 토글 동작
// Basys3 보드 100MHz 클럭 사용

module toggle_fsm(
    input clk,           // 100MHz 클럭
    input reset,         // 리셋 버튼
    input btn,           // 입력 버튼
    output reg led       // 출력 LED
);

    // State 정의
    localparam OFF = 1'b0;
    localparam ON  = 1'b1;
    
    reg state, next_state;
    
    // 버튼 디바운싱을 위한 변수
    reg btn_prev;
    wire btn_edge;
    
    // 상승 엣지 감지
    assign btn_edge = btn & ~btn_prev;
    
    // State Register (Sequential Logic)
    always @(posedge clk or posedge reset) begin
        if (reset) begin
            state <= OFF;
            btn_prev <= 0;
        end
        else begin
            state <= next_state;
            btn_prev <= btn;
        end
    end
    
    // Next State Logic (Combinational Logic)
    always @(*) begin
        case (state)
            OFF: begin
                if (btn_edge)
                    next_state = ON;
                else
                    next_state = OFF;
            end
            ON: begin
                if (btn_edge)
                    next_state = OFF;
                else
                    next_state = ON;
            end
            default: next_state = OFF;
        endcase
    end
    
    // Output Logic
    always @(*) begin
        case (state)
            OFF: led = 1'b0;
            ON:  led = 1'b1;
            default: led = 1'b0;
        endcase
    end

endmodule
```

```verilog
// ========================================
// 1번 - Toggle FSM 테스트벤치
// ========================================
`timescale 1ns / 1ps

module tb_toggle_fsm;

    // 입력 신호 (reg)
    reg clk;
    reg reset;
    reg btn;
    
    // 출력 신호 (wire)
    wire led;
    
    // DUT (Device Under Test) 인스턴스화
    toggle_fsm uut (
        .clk(clk),
        .reset(reset),
        .btn(btn),
        .led(led)
    );
    
    // 클럭 생성 (100MHz = 10ns 주기)
    initial begin
        clk = 0;
        forever #5 clk = ~clk;  // 5ns마다 토글 (10ns 주기)
    end
    
    // 테스트 시나리오
    initial begin
        // 파형 덤프 설정 (시뮬레이션 결과 저장)
        $dumpfile("toggle_fsm.vcd");
        $dumpvars(0, tb_toggle_fsm);
        
        // 초기화
        reset = 1;
        btn = 0;
        
        // 리셋 해제
        #100;
        reset = 0;
        $display("Time=%0t: Reset released", $time);
        
        // 테스트 1: 첫 번째 버튼 누름 (LED 켜짐)
        #50;
        btn = 1;
        $display("Time=%0t: Button pressed (1st time)", $time);
        #20;
        btn = 0;
        $display("Time=%0t: Button released", $time);
        #100;
        
        if (led == 1)
            $display("Time=%0t: TEST 1 PASSED - LED is ON", $time);
        else
            $display("Time=%0t: TEST 1 FAILED - LED should be ON", $time);
        
        // 테스트 2: 두 번째 버튼 누름 (LED 꺼짐)
        #50;
        btn = 1;
        $display("Time=%0t: Button pressed (2nd time)", $time);
        #20;
        btn = 0;
        $display("Time=%0t: Button released", $time);
        #100;
        
        if (led == 0)
            $display("Time=%0t: TEST 2 PASSED - LED is OFF", $time);
        else
            $display("Time=%0t: TEST 2 FAILED - LED should be OFF", $time);
        
        // 테스트 3: 세 번째 버튼 누름 (LED 다시 켜짐)
        #50;
        btn = 1;
        $display("Time=%0t: Button pressed (3rd time)", $time);
        #20;
        btn = 0;
        $display("Time=%0t: Button released", $time);
        #100;
        
        if (led == 1)
            $display("Time=%0t: TEST 3 PASSED - LED is ON", $time);
        else
            $display("Time=%0t: TEST 3 FAILED - LED should be ON", $time);
        
        // 테스트 4: 버튼을 계속 누르고 있을 때 (한 번만 토글되어야 함)
        #50;
        btn = 1;
        $display("Time=%0t: Button pressed and held", $time);
        #200;  // 버튼을 200ns 동안 누르고 있음
        btn = 0;
        $display("Time=%0t: Button released after long press", $time);
        #100;
        
        if (led == 0)
            $display("Time=%0t: TEST 4 PASSED - LED toggled only once", $time);
        else
            $display("Time=%0t: TEST 4 FAILED - LED should be OFF", $time);
        
        // 테스트 5: 리셋 테스트 (LED가 꺼져야 함)
        #50;
        btn = 1;
        #20;
        btn = 0;
        #50;  // LED가 켜진 상태
        
        reset = 1;
        $display("Time=%0t: Reset activated", $time);
        #50;
        reset = 0;
        $display("Time=%0t: Reset deactivated", $time);
        #50;
        
        if (led == 0)
            $display("Time=%0t: TEST 5 PASSED - LED reset to OFF", $time);
        else
            $display("Time=%0t: TEST 5 FAILED - LED should be OFF after reset", $time);
        
        // 테스트 6: 빠른 연속 버튼 누름 (디바운싱 테스트)
        #100;
        $display("Time=%0t: Testing rapid button presses", $time);
        
        // 첫 번째 누름
        btn = 1; #15; btn = 0; #15;
        // 두 번째 누름
        btn = 1; #15; btn = 0; #15;
        // 세 번째 누름
        btn = 1; #15; btn = 0; #100;
        
        // LED는 3번 토글되어 켜진 상태여야 함 (OFF->ON->OFF->ON)
        if (led == 1)
            $display("Time=%0t: TEST 6 PASSED - Rapid presses handled correctly", $time);
        else
            $display("Time=%0t: TEST 6 FAILED - LED should be ON after 3 toggles", $time);
        
        // 시뮬레이션 종료
        #200;
        $display("\n========================================");
        $display("Toggle FSM Testbench Completed");
        $display("========================================");
        $finish;
    end
    
    // 상태 변화 모니터링
    initial begin
        $monitor("Time=%0t | clk=%b reset=%b btn=%b | led=%b", 
                 $time, clk, reset, btn, led);
    end
    
    // 타임아웃 (무한 루프 방지)
    initial begin
        #10000;  // 10us 후 자동 종료
        $display("ERROR: Simulation timeout!");
        $finish;
    end

endmodule
```

---

## 2️. 시퀀스 FSM (입문)

```verilog
// ========================================
// 2번 - 시퀀스 FSM (입문)
// ========================================
// 4개의 LED가 순차적으로 켜지는 패턴
// Basys3 보드 100MHz 클럭 사용

module sequence_fsm(
    input clk,              // 100MHz 클럭
    input reset,            // 리셋
    input enable,           // 동작 활성화
    output reg [3:0] leds   // 4개의 LED
);

    // State 정의
    localparam S0 = 2'b00;
    localparam S1 = 2'b01;
    localparam S2 = 2'b10;
    localparam S3 = 2'b11;
    
    reg [1:0] state, next_state;
    
    // 1초 카운터 (100MHz / 100,000,000 = 1Hz)
    reg [26:0] counter;
    wire tick;
    
    assign tick = (counter == 27'd99_999_999);
    
    // 1초 타이머
    always @(posedge clk or posedge reset) begin
        if (reset)
            counter <= 0;
        else if (enable) begin
            if (tick)
                counter <= 0;
            else
                counter <= counter + 1;
        end
        else
            counter <= 0;
    end
    
    // State Register
    always @(posedge clk or posedge reset) begin
        if (reset)
            state <= S0;
        else if (enable && tick)
            state <= next_state;
    end
    
    // Next State Logic
    always @(*) begin
        case (state)
            S0: next_state = S1;
            S1: next_state = S2;
            S2: next_state = S3;
            S3: next_state = S0;
            default: next_state = S0;
        endcase
    end
    
    // Output Logic
    always @(*) begin
        case (state)
            S0: leds = 4'b0001;  // LED 0만 켜짐
            S1: leds = 4'b0010;  // LED 1만 켜짐
            S2: leds = 4'b0100;  // LED 2만 켜짐
            S3: leds = 4'b1000;  // LED 3만 켜짐
            default: leds = 4'b0000;
        endcase
    end

endmodule
```

```verilog
// ========================================
// 2번 - 시퀀스 FSM 테스트벤치
// ========================================
`timescale 1ns / 1ps

module tb_sequence_fsm;

    // 입력 신호 (reg)
    reg clk;
    reg reset;
    reg enable;
    
    // 출력 신호 (wire)
    wire [3:0] leds;
    
    // DUT (Device Under Test) 인스턴스화
    sequence_fsm uut (
        .clk(clk),
        .reset(reset),
        .enable(enable),
        .leds(leds)
    );
    
    // 클럭 생성 (100MHz = 10ns 주기)
    initial begin
        clk = 0;
        forever #5 clk = ~clk;  // 5ns마다 토글 (10ns 주기)
    end
    
    // 1초 카운터를 빠르게 하기 위한 매개변수
    // 실제 시뮬레이션에서는 1초를 기다릴 수 없으므로 짧게 설정
    // sequence_fsm 모듈에서 counter 값을 줄여야 함
    // 테스트를 위해 100 클럭 = 1초로 가정
    
    // 테스트 시나리오
    initial begin
        // 파형 덤프 설정
        $dumpfile("sequence_fsm.vcd");
        $dumpvars(0, tb_sequence_fsm);
        
        // 초기화
        reset = 1;
        enable = 0;
        
        $display("========================================");
        $display("Sequence FSM Testbench Started");
        $display("========================================\n");
        
        // 리셋 해제
        #100;
        reset = 0;
        $display("Time=%0t: Reset released", $time);
        
        // 테스트 1: enable이 꺼져있을 때 (동작하지 않아야 함)
        #200;
        $display("\n--- TEST 1: Enable OFF (No operation) ---");
        $display("Time=%0t: Enable=0, LEDs should remain at initial state", $time);
        
        if (leds == 4'b0001)
            $display("Time=%0t: TEST 1 PASSED - LEDs stayed at S0", $time);
        else
            $display("Time=%0t: TEST 1 FAILED - LEDs changed without enable", $time);
        
        // 테스트 2: enable 켜고 시퀀스 동작 확인
        #100;
        enable = 1;
        $display("\n--- TEST 2: Enable ON (Sequence operation) ---");
        $display("Time=%0t: Enable=1, Starting sequence", $time);
        
        // 주의: 실제로는 1초마다 변경되지만, 시뮬레이션에서는
        // counter 값을 줄여서 테스트해야 합니다.
        // 여기서는 개념적으로 시간을 표시합니다.
        
        // S0 상태 확인
        #50;
        $display("Time=%0t: State S0 - LEDs=%b (Expected: 0001)", $time, leds);
        if (leds == 4'b0001)
            $display("Time=%0t: S0 PASSED", $time);
        else
            $display("Time=%0t: S0 FAILED", $time);
        
        // 충분한 시간 대기 (실제 구현에서 counter를 줄인 경우)
        // 예: counter가 100까지만 카운트하도록 수정했다면
        #1500;  // 100 클럭 * 10ns = 1000ns 정도 대기
        
        // S1 상태 확인
        $display("Time=%0t: State S1 - LEDs=%b (Expected: 0010)", $time, leds);
        if (leds == 4'b0010)
            $display("Time=%0t: S1 PASSED", $time);
        else
            $display("Time=%0t: S1 FAILED", $time);
        
        #1500;
        
        // S2 상태 확인
        $display("Time=%0t: State S2 - LEDs=%b (Expected: 0100)", $time, leds);
        if (leds == 4'b0100)
            $display("Time=%0t: S2 PASSED", $time);
        else
            $display("Time=%0t: S2 FAILED", $time);
        
        #1500;
        
        // S3 상태 확인
        $display("Time=%0t: State S3 - LEDs=%b (Expected: 1000)", $time, leds);
        if (leds == 4'b1000)
            $display("Time=%0t: S3 PASSED", $time);
        else
            $display("Time=%0t: S3 FAILED", $time);
        
        #1500;
        
        // S0으로 다시 돌아왔는지 확인 (순환)
        $display("Time=%0t: Back to S0 - LEDs=%b (Expected: 0001)", $time, leds);
        if (leds == 4'b0001)
            $display("Time=%0t: CYCLE TEST PASSED - Returned to S0", $time);
        else
            $display("Time=%0t: CYCLE TEST FAILED", $time);
        
        // 테스트 3: enable 끄기 (동작 멈춤)
        #1000;
        $display("\n--- TEST 3: Disable during operation ---");
        enable = 0;
        $display("Time=%0t: Enable=0, Sequence should stop", $time);
        
        // 현재 LED 상태 저장
        reg [3:0] leds_before;
        leds_before = leds;
        
        #3000;  // 충분히 대기
        
        if (leds == leds_before)
            $display("Time=%0t: TEST 3 PASSED - Sequence stopped", $time);
        else
            $display("Time=%0t: TEST 3 FAILED - Sequence should not change", $time);
        
        // 테스트 4: 다시 enable 켜기 (이어서 동작)
        #500;
        $display("\n--- TEST 4: Re-enable ---");
        enable = 1;
        $display("Time=%0t: Enable=1, Sequence resumes", $time);
        
        #1500;
        $display("Time=%0t: LEDs=%b (Should have moved to next state)", $time, leds);
        
        // 테스트 5: 리셋 테스트
        #2000;
        $display("\n--- TEST 5: Reset during operation ---");
        reset = 1;
        $display("Time=%0t: Reset activated", $time);
        
        #100;
        reset = 0;
        $display("Time=%0t: Reset deactivated", $time);
        
        #50;
        if (leds == 4'b0001)
            $display("Time=%0t: TEST 5 PASSED - Reset to S0", $time);
        else
            $display("Time=%0t: TEST 5 FAILED - Should reset to S0", $time);
        
        // 완전한 사이클 테스트
        #500;
        $display("\n--- TEST 6: Complete cycle verification ---");
        enable = 1;
        
        #1500;
        $display("Time=%0t: S0->S1 transition, LEDs=%b", $time, leds);
        #1500;
        $display("Time=%0t: S1->S2 transition, LEDs=%b", $time, leds);
        #1500;
        $display("Time=%0t: S2->S3 transition, LEDs=%b", $time, leds);
        #1500;
        $display("Time=%0t: S3->S0 transition, LEDs=%b", $time, leds);
        
        if (leds == 4'b0001)
            $display("Time=%0t: TEST 6 PASSED - Complete cycle verified", $time);
        else
            $display("Time=%0t: TEST 6 FAILED - Cycle incomplete", $time);
        
        // 시뮬레이션 종료
        #1000;
        $display("\n========================================");
        $display("Sequence FSM Testbench Completed");
        $display("========================================");
        $display("\nNOTE: For actual simulation, modify the counter");
        $display("      in sequence_fsm.v from 99_999_999 to 100");
        $display("      for faster testing.");
        $finish;
    end
    
    // LED 상태 변화 모니터링
    always @(leds) begin
        case (leds)
            4'b0001: $display("  --> LED Pattern: 0001 (State S0)");
            4'b0010: $display("  --> LED Pattern: 0010 (State S1)");
            4'b0100: $display("  --> LED Pattern: 0100 (State S2)");
            4'b1000: $display("  --> LED Pattern: 1000 (State S3)");
            default: $display("  --> LED Pattern: %b (Unknown)", leds);
        endcase
    end
    
    // 타임아웃 (무한 루프 방지)
    initial begin
        #50000;  // 50us 후 자동 종료
        $display("ERROR: Simulation timeout!");
        $finish;
    end

endmodule


// ========================================
// 시뮬레이션을 위한 수정된 sequence_fsm
// ========================================
// 원본 sequence_fsm의 counter를 줄인 버전
// 테스트 시 이 버전을 사용하세요

/*
module sequence_fsm(
    input clk,
    input reset,
    input enable,
    output reg [3:0] leds
);

    localparam S0 = 2'b00;
    localparam S1 = 2'b01;
    localparam S2 = 2'b10;
    localparam S3 = 2'b11;
    
    reg [1:0] state, next_state;
    
    // 시뮬레이션용: 100 클럭으로 변경 (원본은 99_999_999)
    reg [26:0] counter;
    wire tick;
    
    assign tick = (counter == 27'd100);  // 테스트용으로 축소
    
    always @(posedge clk or posedge reset) begin
        if (reset)
            counter <= 0;
        else if (enable) begin
            if (tick)
                counter <= 0;
            else
                counter <= counter + 1;
        end
        else
            counter <= 0;
    end
    
    always @(posedge clk or posedge reset) begin
        if (reset)
            state <= S0;
        else if (enable && tick)
            state <= next_state;
    end
    
    always @(*) begin
        case (state)
            S0: next_state = S1;
            S1: next_state = S2;
            S2: next_state = S3;
            S3: next_state = S0;
            default: next_state = S0;
        endcase
    end
    
    always @(*) begin
        case (state)
            S0: leds = 4'b0001;
            S1: leds = 4'b0010;
            S2: leds = 4'b0100;
            S3: leds = 4'b1000;
            default: leds = 4'b0000;
        endcase
    end

endmodule
*/
```

---

## 3️. 신호등 FSM (초급)

```verilog
// ========================================
// 3번 - 신호등 FSM (초급)
// ========================================
// 빨강(5초) -> 초록(5초) -> 노랑(2초) -> 반복
// Basys3 보드 100MHz 클럭 사용

module traffic_light_fsm(
    input clk,              // 100MHz 클럭
    input reset,            // 리셋
    input enable,           // 동작 활성화
    output reg red,         // 빨간불
    output reg yellow,      // 노란불
    output reg green        // 초록불
);

    // State 정의
    localparam RED    = 2'b00;
    localparam GREEN  = 2'b01;
    localparam YELLOW = 2'b10;
    
    reg [1:0] state, next_state;
    
    // 타이머 카운터 (1초 = 100,000,000 클럭)
    reg [26:0] counter;
    reg [3:0] time_count;  // 상태별 시간 카운터
    
    wire tick_1sec;
    assign tick_1sec = (counter == 27'd99_999_999);
    
    // 상태별 지속 시간 정의
    localparam RED_TIME    = 4'd5;  // 5초
    localparam GREEN_TIME  = 4'd5;  // 5초
    localparam YELLOW_TIME = 4'd2;  // 2초
    
    // 1초 타이머
    always @(posedge clk or posedge reset) begin
        if (reset)
            counter <= 0;
        else if (enable) begin
            if (tick_1sec)
                counter <= 0;
            else
                counter <= counter + 1;
        end
        else
            counter <= 0;
    end
    
    // State Register와 시간 카운터
    always @(posedge clk or posedge reset) begin
        if (reset) begin
            state <= RED;
            time_count <= 0;
        end
        else if (enable) begin
            if (tick_1sec) begin
                if (time_count == get_state_time(state) - 1) begin
                    state <= next_state;
                    time_count <= 0;
                end
                else begin
                    time_count <= time_count + 1;
                end
            end
        end
        else begin
            state <= RED;
            time_count <= 0;
        end
    end
    
    // 상태별 시간을 반환하는 함수
    function [3:0] get_state_time;
        input [1:0] s;
        begin
            case (s)
                RED:    get_state_time = RED_TIME;
                GREEN:  get_state_time = GREEN_TIME;
                YELLOW: get_state_time = YELLOW_TIME;
                default: get_state_time = RED_TIME;
            endcase
        end
    endfunction
    
    // Next State Logic
    always @(*) begin
        case (state)
            RED:    next_state = GREEN;
            GREEN:  next_state = YELLOW;
            YELLOW: next_state = RED;
            default: next_state = RED;
        endcase
    end
    
    // Output Logic
    always @(*) begin
        // 기본값
        red = 0;
        yellow = 0;
        green = 0;
        
        case (state)
            RED: begin
                red = 1;
                yellow = 0;
                green = 0;
            end
            GREEN: begin
                red = 0;
                yellow = 0;
                green = 1;
            end
            YELLOW: begin
                red = 0;
                yellow = 1;
                green = 0;
            end
            default: begin
                red = 1;
                yellow = 0;
                green = 0;
            end
        endcase
    end

endmodule
```

```verilog
// ========================================
// 3번 - 신호등 FSM 테스트벤치
// ========================================
`timescale 1ns / 1ps

module tb_traffic_light_fsm;

    // 입력 신호 (reg)
    reg clk;
    reg reset;
    reg enable;
    
    // 출력 신호 (wire)
    wire red;
    wire yellow;
    wire green;
    
    // 신호등 상태를 문자열로 표시하기 위한 변수
    reg [63:0] light_state;
    
    // DUT (Device Under Test) 인스턴스화
    traffic_light_fsm uut (
        .clk(clk),
        .reset(reset),
        .enable(enable),
        .red(red),
        .yellow(yellow),
        .green(green)
    );
    
    // 클럭 생성 (100MHz = 10ns 주기)
    initial begin
        clk = 0;
        forever #5 clk = ~clk;
    end
    
    // 신호등 상태를 문자열로 변환
    always @(*) begin
        if (red && !yellow && !green)
            light_state = "RED";
        else if (!red && !yellow && green)
            light_state = "GREEN";
        else if (!red && yellow && !green)
            light_state = "YELLOW";
        else if (!red && !yellow && !green)
            light_state = "OFF";
        else
            light_state = "ERROR";
    end
    
    // 테스트 시나리오
    initial begin
        // 파형 덤프 설정
        $dumpfile("traffic_light_fsm.vcd");
        $dumpvars(0, tb_traffic_light_fsm);
        
        // 초기화
        reset = 1;
        enable = 0;
        
        $display("========================================");
        $display("Traffic Light FSM Testbench Started");
        $display("========================================\n");
        
        // 리셋 해제
        #100;
        reset = 0;
        $display("Time=%0t: Reset released", $time);
        
        // 테스트 1: enable이 꺼져있을 때
        #200;
        $display("\n--- TEST 1: Enable OFF ---");
        $display("Time=%0t: Enable=0, Lights should be RED", $time);
        
        #100;
        if (red && !yellow && !green)
            $display("Time=%0t: TEST 1 PASSED - Initial state is RED", $time);
        else
            $display("Time=%0t: TEST 1 FAILED - Should be RED", $time);
        
        // 테스트 2: enable 켜고 전체 사이클 동작 확인
        #100;
        enable = 1;
        $display("\n--- TEST 2: Full Cycle Test ---");
        $display("Time=%0t: Enable=1, Starting traffic light cycle", $time);
        $display("Expected sequence: RED(5s) -> GREEN(5s) -> YELLOW(2s) -> RED");
        
        // 주의: 실제 시뮬레이션을 위해서는 counter 값을 줄여야 합니다
        // 예: 99_999_999 -> 100 (1초를 100 클럭으로)
        
        // RED 상태 확인 (5초)
        #50;
        $display("\nTime=%0t: Checking RED state (should last 5 seconds)", $time);
        if (red && !yellow && !green)
            $display("Time=%0t: RED state - PASS", $time);
        else
            $display("Time=%0t: RED state - FAIL", $time);
        
        // RED 상태 유지 확인 (중간 시점)
        #2500;  // 가정: 1초 = 500ns (축소된 counter 사용 시)
        if (red && !yellow && !green)
            $display("Time=%0t: Still RED (mid-duration) - PASS", $time);
        else
            $display("Time=%0t: Should still be RED - FAIL", $time);
        
        // GREEN으로 전환 대기 (RED 5초 완료)
        #2500;
        $display("\nTime=%0t: Expecting transition to GREEN", $time);
        #100;
        if (!red && !yellow && green)
            $display("Time=%0t: GREEN state - PASS", $time);
        else
            $display("Time=%0t: GREEN state - FAIL (R=%b Y=%b G=%b)", $time, red, yellow, green);
        
        // GREEN 상태 유지 확인
        #2500;
        if (!red && !yellow && green)
            $display("Time=%0t: Still GREEN (mid-duration) - PASS", $time);
        else
            $display("Time=%0t: Should still be GREEN - FAIL", $time);
        
        // YELLOW로 전환 대기 (GREEN 5초 완료)
        #2500;
        $display("\nTime=%0t: Expecting transition to YELLOW", $time);
        #100;
        if (!red && yellow && !green)
            $display("Time=%0t: YELLOW state - PASS", $time);
        else
            $display("Time=%0t: YELLOW state - FAIL (R=%b Y=%b G=%b)", $time, red, yellow, green);
        
        // YELLOW 상태 유지 확인 (2초)
        #1000;
        if (!red && yellow && !green)
            $display("Time=%0t: Still YELLOW (mid-duration) - PASS", $time);
        else
            $display("Time=%0t: Should still be YELLOW - FAIL", $time);
        
        // RED로 다시 전환 대기 (YELLOW 2초 완료)
        #1000;
        $display("\nTime=%0t: Expecting transition back to RED", $time);
        #100;
        if (red && !yellow && !green)
            $display("Time=%0t: Back to RED - CYCLE COMPLETE - PASS", $time);
        else
            $display("Time=%0t: Should be RED - FAIL", $time);
        
        // 테스트 3: 사이클 반복 확인
        $display("\n--- TEST 3: Cycle Repeat Test ---");
        $display("Time=%0t: Verifying cycle repeats correctly", $time);
        
        // 다시 GREEN으로 전환 확인
        #5000;
        #100;
        if (!red && !yellow && green)
            $display("Time=%0t: Second GREEN cycle - PASS", $time);
        else
            $display("Time=%0t: Second cycle failed - FAIL", $time);
        
        // 테스트 4: enable 끄기 (동작 멈춤)
        #1000;
        $display("\n--- TEST 4: Disable Test ---");
        enable = 0;
        $display("Time=%0t: Enable=0, Traffic light should stop and reset to RED", $time);
        
        #200;
        if (red && !yellow && !green)
            $display("Time=%0t: TEST 4 PASSED - Reset to RED when disabled", $time);
        else
            $display("Time=%0t: TEST 4 FAILED - Should be RED", $time);
        
        // 시간이 지나도 상태 변경 없어야 함
        #3000;
        if (red && !yellow && !green)
            $display("Time=%0t: Still RED (no state change) - PASS", $time);
        else
            $display("Time=%0t: Should not change state - FAIL", $time);
        
        // 테스트 5: 다시 enable 켜기
        #500;
        $display("\n--- TEST 5: Re-enable Test ---");
        enable = 1;
        $display("Time=%0t: Enable=1, Cycle should restart from RED", $time);
        
        #100;
        if (red && !yellow && !green)
            $display("Time=%0t: Starting from RED - PASS", $time);
        else
            $display("Time=%0t: Should start from RED - FAIL", $time);
        
        // 다음 상태로 전환 확인
        #5000;
        #100;
        if (!red && !yellow && green)
            $display("Time=%0t: Transitioned to GREEN - PASS", $time);
        else
            $display("Time=%0t: Should be GREEN - FAIL", $time);
        
        // 테스트 6: 리셋 테스트
        #2000;
        $display("\n--- TEST 6: Reset Test ---");
        reset = 1;
        $display("Time=%0t: Reset activated (should go to RED)", $time);
        
        #100;
        reset = 0;
        $display("Time=%0t: Reset deactivated", $time);
        
        #50;
        if (red && !yellow && !green)
            $display("Time=%0t: TEST 6 PASSED - Reset to RED", $time);
        else
            $display("Time=%0t: TEST 6 FAILED - Should be RED after reset", $time);
        
        // 테스트 7: 타이밍 정확도 테스트
        $display("\n--- TEST 7: Timing Accuracy Test ---");
        $display("Time=%0t: Verifying state durations", $time);
        
        enable = 1;
        
        // RED 시작 시간 기록
        #100;
        $display("Time=%0t: RED started", $time);
        
        // 정확히 5초(축소 시간) 후 GREEN 확인
        #5000;
        #100;
        if (!red && !yellow && green) begin
            $display("Time=%0t: GREEN started (RED lasted correct duration) - PASS", $time);
            
            // GREEN 5초 확인
            #5000;
            #100;
            if (!red && yellow && !green) begin
                $display("Time=%0t: YELLOW started (GREEN lasted correct duration) - PASS", $time);
                
                // YELLOW 2초 확인
                #2000;
                #100;
                if (red && !yellow && !green)
                    $display("Time=%0t: RED started (YELLOW lasted correct duration) - PASS", $time);
                else
                    $display("Time=%0t: Timing error in YELLOW duration - FAIL", $time);
            end else
                $display("Time=%0t: GREEN duration error - FAIL", $time);
        end else
            $display("Time=%0t: RED duration error - FAIL", $time);
        
        // 시뮬레이션 종료
        #1000;
        $display("\n========================================");
        $display("Traffic Light FSM Testbench Completed");
        $display("========================================");
        $display("\nNOTE: For actual simulation, modify the counter");
        $display("      in traffic_light_fsm.v from 99_999_999 to 100");
        $display("      for faster testing.");
        $display("      Timing: 1 second = 100 clocks (1000ns)");
        $finish;
    end
    
    // 신호등 상태 변화 모니터링
    always @(red or yellow or green) begin
        $display("  --> Traffic Light: R=%b Y=%b G=%b [%s]", 
                 red, yellow, green, light_state);
    end
    
    // 타임아웃 (무한 루프 방지)
    initial begin
        #100000;  // 100us 후 자동 종료
        $display("ERROR: Simulation timeout!");
        $finish;
    end

endmodule


// ========================================
// 시뮬레이션을 위한 수정된 traffic_light_fsm
// ========================================
// 원본 traffic_light_fsm의 counter를 줄인 버전
// 테스트 시 이 버전을 사용하세요

/*
module traffic_light_fsm(
    input clk,
    input reset,
    input enable,
    output reg red,
    output reg yellow,
    output reg green
);

    localparam RED    = 2'b00;
    localparam GREEN  = 2'b01;
    localparam YELLOW = 2'b10;
    
    reg [1:0] state, next_state;
    
    // 시뮬레이션용으로 축소 (1초 = 100 클럭)
    reg [26:0] counter;
    reg [3:0] time_count;
    
    wire tick_1sec;
    assign tick_1sec = (counter == 27'd100);  // 테스트용
    
    localparam RED_TIME    = 4'd5;
    localparam GREEN_TIME  = 4'd5;
    localparam YELLOW_TIME = 4'd2;
    
    always @(posedge clk or posedge reset) begin
        if (reset)
            counter <= 0;
        else if (enable) begin
            if (tick_1sec)
                counter <= 0;
            else
                counter <= counter + 1;
        end
        else
            counter <= 0;
    end
    
    always @(posedge clk or posedge reset) begin
        if (reset) begin
            state <= RED;
            time_count <= 0;
        end
        else if (enable) begin
            if (tick_1sec) begin
                if (time_count == get_state_time(state) - 1) begin
                    state <= next_state;
                    time_count <= 0;
                end
                else begin
                    time_count <= time_count + 1;
                end
            end
        end
        else begin
            state <= RED;
            time_count <= 0;
        end
    end
    
    function [3:0] get_state_time;
        input [1:0] s;
        begin
            case (s)
                RED:    get_state_time = RED_TIME;
                GREEN:  get_state_time = GREEN_TIME;
                YELLOW: get_state_time = YELLOW_TIME;
                default: get_state_time = RED_TIME;
            endcase
        end
    endfunction
    
    always @(*) begin
        case (state)
            RED:    next_state = GREEN;
            GREEN:  next_state = YELLOW;
            YELLOW: next_state = RED;
            default: next_state = RED;
        endcase
    end
    
    always @(*) begin
        red = 0;
        yellow = 0;
        green = 0;
        
        case (state)
            RED: begin
                red = 1;
                yellow = 0;
                green = 0;
            end
            GREEN: begin
                red = 0;
                yellow = 0;
                green = 1;
            end
            YELLOW: begin
                red = 0;
                yellow = 1;
                green = 0;
            end
            default: begin
                red = 1;
                yellow = 0;
                green = 0;
            end
        endcase
    end

endmodule
*/
```

---

## 4️. 자판기 FSM (중급)

```verilog
// ========================================
// 4번 - 자판기 FSM (중급)
// ========================================
// 500원짜리 음료 판매, 100원/500원 동전 투입 가능
// 거스름돈 반환 기능 포함
// Basys3 보드 100MHz 클럭 사용

module vending_machine_fsm(
    input clk,              // 100MHz 클럭
    input reset,            // 리셋
    input coin_100,         // 100원 투입
    input coin_500,         // 500원 투입
    input cancel,           // 취소 버튼
    output reg dispense,    // 음료 제공
    output reg [2:0] change,// 거스름돈 (100원 단위)
    output reg [6:0] seg,   // 7-segment 디스플레이
    output reg [3:0] an     // 7-segment anode
);

    // State 정의
    localparam IDLE   = 3'b000;  // 0원
    localparam COIN1  = 3'b001;  // 100원
    localparam COIN2  = 3'b010;  // 200원
    localparam COIN3  = 3'b011;  // 300원
    localparam COIN4  = 3'b100;  // 400원
    localparam COIN5  = 3'b101;  // 500원 이상 (제공)
    
    reg [2:0] state, next_state;
    reg [2:0] amount;  // 현재 투입 금액 (100원 단위)
    
    // 버튼 엣지 감지
    reg coin_100_prev, coin_500_prev, cancel_prev;
    wire coin_100_edge, coin_500_edge, cancel_edge;
    
    assign coin_100_edge = coin_100 & ~coin_100_prev;
    assign coin_500_edge = coin_500 & ~coin_500_prev;
    assign cancel_edge = cancel & ~cancel_prev;
    
    // State Register
    always @(posedge clk or posedge reset) begin
        if (reset) begin
            state <= IDLE;
            coin_100_prev <= 0;
            coin_500_prev <= 0;
            cancel_prev <= 0;
        end
        else begin
            state <= next_state;
            coin_100_prev <= coin_100;
            coin_500_prev <= coin_500;
            cancel_prev <= cancel;
        end
    end
    
    // Next State Logic
    always @(*) begin
        next_state = state;
        
        if (cancel_edge) begin
            next_state = IDLE;
        end
        else begin
            case (state)
                IDLE: begin
                    if (coin_100_edge)
                        next_state = COIN1;
                    else if (coin_500_edge)
                        next_state = COIN5;
                end
                
                COIN1: begin
                    if (coin_100_edge)
                        next_state = COIN2;
                    else if (coin_500_edge)
                        next_state = COIN5;
                end
                
                COIN2: begin
                    if (coin_100_edge)
                        next_state = COIN3;
                    else if (coin_500_edge)
                        next_state = COIN5;
                end
                
                COIN3: begin
                    if (coin_100_edge)
                        next_state = COIN4;
                    else if (coin_500_edge)
                        next_state = COIN5;
                end
                
                COIN4: begin
                    if (coin_100_edge)
                        next_state = COIN5;
                    else if (coin_500_edge)
                        next_state = COIN5;
                end
                
                COIN5: begin
                    // 음료 제공 후 IDLE로 복귀
                    next_state = IDLE;
                end
                
                default: next_state = IDLE;
            endcase
        end
    end
    
    // Output Logic
    always @(*) begin
        // 기본값
        dispense = 0;
        change = 0;
        amount = 0;
        
        case (state)
            IDLE: begin
                amount = 0;
                dispense = 0;
                change = 0;
            end
            COIN1: amount = 1;
            COIN2: amount = 2;
            COIN3: amount = 3;
            COIN4: amount = 4;
            COIN5: begin
                dispense = 1;
                if (amount > 5)
                    change = amount - 5;
                else
                    change = 0;
            end
            default: begin
                amount = 0;
                dispense = 0;
                change = 0;
            end
        endcase
        
        // 취소 시 전액 반환
        if (cancel_edge && state != IDLE)
            change = amount;
    end
    
    // 7-segment 디스플레이 (투입 금액 표시)
    always @(*) begin
        an = 4'b1110;  // 첫 번째 디지트만 활성화
        
        case (state)
            IDLE:   seg = 7'b1000000;  // 0
            COIN1:  seg = 7'b1111001;  // 1
            COIN2:  seg = 7'b0100100;  // 2
            COIN3:  seg = 7'b0110000;  // 3
            COIN4:  seg = 7'b0011001;  // 4
            COIN5:  seg = 7'b0010010;  // 5
            default: seg = 7'b1111111;  // off
        endcase
    end

endmodule
```

```verilog

```

---

## 5️. UART 수신기 FSM (중상급)

```verilog
// ========================================
// 5번 - UART 수신기 FSM (중상급)
// ========================================
// 9600 baud, 8-N-1 프로토콜
// Basys3 보드 100MHz 클럭 사용

module uart_rx_fsm(
    input clk,              // 100MHz 클럭
    input reset,            // 리셋
    input rx,               // UART RX 신호
    output reg [7:0] data,  // 수신된 데이터
    output reg valid,       // 데이터 유효 신호
    output reg error        // 에러 플래그
);

    // State 정의
    localparam IDLE       = 3'b000;
    localparam START_BIT  = 3'b001;
    localparam DATA_BITS  = 3'b010;
    localparam STOP_BIT   = 3'b011;
    localparam CLEANUP    = 3'b100;
    
    reg [2:0] state, next_state;
    
    // Baud rate 생성 (100MHz / 9600 = 10416.67)
    localparam BAUD_RATE = 16'd10417;
    localparam HALF_BAUD = 16'd5208;
    
    reg [15:0] baud_counter;
    reg [2:0] bit_counter;    // 0~7 비트 카운터
    reg [7:0] shift_reg;      // 수신 데이터 시프트 레지스터
    reg sample_tick;
    
    // RX 신호 동기화 (메타스테이블 방지)
    reg rx_sync1, rx_sync2;
    always @(posedge clk) begin
        rx_sync1 <= rx;
        rx_sync2 <= rx_sync1;
    end
    
    // Baud rate 카운터
    always @(posedge clk or posedge reset) begin
        if (reset) begin
            baud_counter <= 0;
            sample_tick <= 0;
        end
        else begin
            sample_tick <= 0;
            
            if (state == IDLE) begin
                baud_counter <= 0;
            end
            else if (state == START_BIT && baud_counter == HALF_BAUD - 1) begin
                baud_counter <= 0;
                sample_tick <= 1;
            end
            else if (baud_counter == BAUD_RATE - 1) begin
                baud_counter <= 0;
                sample_tick <= 1;
            end
            else begin
                baud_counter <= baud_counter + 1;
            end
        end
    end
    
    // State Register
    always @(posedge clk or posedge reset) begin
        if (reset)
            state <= IDLE;
        else
            state <= next_state;
    end
    
    // Next State Logic
    always @(*) begin
        next_state = state;
        
        case (state)
            IDLE: begin
                if (rx_sync2 == 0)  // Start bit 감지 (falling edge)
                    next_state = START_BIT;
            end
            
            START_BIT: begin
                if (sample_tick) begin
                    if (rx_sync2 == 0)  // Start bit 확인
                        next_state = DATA_BITS;
                    else
                        next_state = IDLE;  // False start
                end
            end
            
            DATA_BITS: begin
                if (sample_tick && bit_counter == 7)
                    next_state = STOP_BIT;
            end
            
            STOP_BIT: begin
                if (sample_tick)
                    next_state = CLEANUP;
            end
            
            CLEANUP: begin
                next_state = IDLE;
            end
            
            default: next_state = IDLE;
        endcase
    end
    
    // 비트 카운터
    always @(posedge clk or posedge reset) begin
        if (reset)
            bit_counter <= 0;
        else begin
            if (state == DATA_BITS && sample_tick)
                bit_counter <= bit_counter + 1;
            else if (state != DATA_BITS)
                bit_counter <= 0;
        end
    end
    
    // 데이터 시프트 레지스터
    always @(posedge clk or posedge reset) begin
        if (reset)
            shift_reg <= 0;
        else begin
            if (state == DATA_BITS && sample_tick)
                shift_reg <= {rx_sync2, shift_reg[7:1]};  // LSB first
        end
    end
    
    // Output Logic
    always @(posedge clk or posedge reset) begin
        if (reset) begin
            data <= 0;
            valid <= 0;
            error <= 0;
        end
        else begin
            valid <= 0;
            error <= 0;
            
            if (state == STOP_BIT && sample_tick) begin
                if (rx_sync2 == 1) begin  // 유효한 stop bit
                    data <= shift_reg;
                    valid <= 1;
                end
                else begin
                    error <= 1;  // Framing error
                end
            end
        end
    end

endmodule
```

```verilog

```

---

## 6️. 엘리베이터 FSM (상급)

```verilog
// ========================================
// 6번 - 엘리베이터 FSM (상급)
// ========================================
// 4층 엘리베이터 제어, 도어 개폐, 방향 표시
// Basys3 보드 100MHz 클럭 사용

module elevator_fsm(
    input clk,                  // 100MHz 클럭
    input reset,                // 리셋
    input [3:0] floor_req,      // 각 층 호출 버튼 (1~4층)
    input door_sensor,          // 도어 센서 (막힘 감지)
    output reg [1:0] current_floor,  // 현재 층 (0~3)
    output reg motor_up,        // 모터 상승
    output reg motor_down,      // 모터 하강
    output reg door_open,       // 도어 열림
    output reg [6:0] seg,       // 7-segment (층 표시)
    output reg dir_up_led,      // 상승 방향 LED
    output reg dir_down_led     // 하강 방향 LED
);

    // State 정의
    localparam IDLE           = 3'b000;
    localparam MOVING_UP      = 3'b001;
    localparam MOVING_DOWN    = 3'b010;
    localparam DOOR_OPENING   = 3'b011;
    localparam DOOR_OPEN_WAIT = 3'b100;
    localparam DOOR_CLOSING   = 3'b101;
    
    reg [2:0] state, next_state;
    reg [3:0] request_queue;    // 각 층 요청 큐
    reg [1:0] target_floor;     // 목표 층
    reg [1:0] direction;        // 0: none, 1: up, 2: down
    
    // 타이머 관련
    reg [26:0] timer;
    reg [3:0] time_count;
    wire tick_1sec;
    
    assign tick_1sec = (timer == 27'd99_999_999);
    
    // 1초 타이머
    always @(posedge clk or posedge reset) begin
        if (reset)
            timer <= 0;
        else begin
            if (tick_1sec)
                timer <= 0;
            else
                timer <= timer + 1;
        end
    end
    
    // 버튼 엣지 감지
    reg [3:0] floor_req_prev;
    wire [3:0] floor_req_edge;
    
    assign floor_req_edge = floor_req & ~floor_req_prev;
    
    always @(posedge clk) begin
        floor_req_prev <= floor_req;
    end
    
    // 요청 큐 관리
    always @(posedge clk or posedge reset) begin
        if (reset)
            request_queue <= 4'b0000;
        else begin
            // 새 요청 추가
            request_queue <= request_queue | floor_req_edge;
            
            // 현재 층 도착 시 해당 요청 제거
            if (state == DOOR_OPENING)
                request_queue[current_floor] <= 0;
        end
    end
    
    // 다음 목표 층 결정
    always @(*) begin
        target_floor = current_floor;
        
        if (direction == 1) begin  // 상승 중
            // 위쪽 요청 찾기
            if (current_floor < 3 && request_queue[3])
                target_floor = 3;
            else if (current_floor < 2 && request_queue[2])
                target_floor = 2;
            else if (current_floor < 1 && request_queue[1])
                target_floor = 1;
            else if (request_queue[0])
                target_floor = 0;
        end
        else if (direction == 2) begin  // 하강 중
            // 아래쪽 요청 찾기
            if (current_floor > 0 && request_queue[0])
                target_floor = 0;
            else if (current_floor > 1 && request_queue[1])
                target_floor = 1;
            else if (current_floor > 2 && request_queue[2])
                target_floor = 2;
            else if (request_queue[3])
                target_floor = 3;
        end
        else begin  // 방향 없음 - 가장 가까운 요청
            if (request_queue != 4'b0000) begin
                // 간단한 최근접 층 찾기
                if (request_queue[current_floor])
                    target_floor = current_floor;
                else if (current_floor < 3 && request_queue[current_floor + 1])
                    target_floor = current_floor + 1;
                else if (current_floor > 0 && request_queue[current_floor - 1])
                    target_floor = current_floor - 1;
                else if (request_queue[0])
                    target_floor = 0;
                else if (request_queue[1])
                    target_floor = 1;
                else if (request_queue[2])
                    target_floor = 2;
                else if (request_queue[3])
                    target_floor = 3;
            end
        end
    end
    
    // State Register
    always @(posedge clk or posedge reset) begin
        if (reset) begin
            state <= IDLE;
            current_floor <= 0;
            time_count <= 0;
            direction <= 0;
        end
        else begin
            state <= next_state;
            
            // 층 이동
            if (state == MOVING_UP && tick_1sec && time_count >= 2) begin
                current_floor <= current_floor + 1;
                time_count <= 0;
            end
            else if (state == MOVING_DOWN && tick_1sec && time_count >= 2) begin
                current_floor <= current_floor - 1;
                time_count <= 0;
            end
            else if ((state == MOVING_UP || state == MOVING_DOWN) && tick_1sec)
                time_count <= time_count + 1;
            else if (state == DOOR_OPEN_WAIT && tick_1sec)
                time_count <= time_count + 1;
            else if (state != DOOR_OPEN_WAIT && state != MOVING_UP && state != MOVING_DOWN)
                time_count <= 0;
        end
    end
    
    // Next State Logic
    always @(*) begin
        next_state = state;
        
        case (state)
            IDLE: begin
                if (request_queue != 4'b0000) begin
                    if (target_floor == current_floor)
                        next_state = DOOR_OPENING;
                    else if (target_floor > current_floor)
                        next_state = MOVING_UP;
                    else
                        next_state = MOVING_DOWN;
                end
            end
            
            MOVING_UP: begin
                if (current_floor == target_floor)
                    next_state = DOOR_OPENING;
            end
            
            MOVING_DOWN: begin
                if (current_floor == target_floor)
                    next_state = DOOR_OPENING;
            end
            
            DOOR_OPENING: begin
                next_state = DOOR_OPEN_WAIT;
            end
            
            DOOR_OPEN_WAIT: begin
                if (time_count >= 3 && !door_sensor)  // 3초 대기
                    next_state = DOOR_CLOSING;
            end
            
            DOOR_CLOSING: begin
                if (!door_sensor)
                    next_state = IDLE;
                else
                    next_state = DOOR_OPENING;  // 재개방
            end
            
            default: next_state = IDLE;
        endcase
    end
    
    // Output Logic
    always @(*) begin
        motor_up = 0;
        motor_down = 0;
        door_open = 0;
        dir_up_led = 0;
        dir_down_led = 0;
        
        case (state)
            MOVING_UP: begin
                motor_up = 1;
                dir_up_led = 1;
            end
            MOVING_DOWN: begin
                motor_down = 1;
                dir_down_led = 1;
            end
            DOOR_OPENING, DOOR_OPEN_WAIT, DOOR_CLOSING: begin
                door_open = 1;
            end
        endcase
    end
    
    // 7-segment 디스플레이 (현재 층 표시: 1~4)
    always @(*) begin
        case (current_floor)
            2'd0: seg = 7'b1111001;  // 1
            2'd1: seg = 7'b0100100;  // 2
            2'd2: seg = 7'b0110000;  // 3
            2'd3: seg = 7'b0011001;  // 4
            default: seg = 7'b1111111;
        endcase
    end

endmodule
```

```verilog

```


---

## 7️. I2C Master FSM (고급)

```verilog
// ========================================
// 7번 - I2C Master FSM (고급)
// ========================================
// 100kHz I2C 통신, 7비트 주소, 단일 바이트 쓰기/읽기 지원
// Basys3 보드 100MHz 클럭 사용

module i2c_master_fsm(
    input clk,              // 100MHz 클럭
    input reset,            // 리셋
    input start,            // 전송 시작
    input rw,               // 1: read, 0: write
    input [6:0] slave_addr, // 슬레이브 주소 (7비트)
    input [7:0] wr_data,    // 쓰기 데이터
    output reg [7:0] rd_data,   // 읽기 데이터
    output reg busy,        // 통신 중 플래그
    output reg ack_error,   // ACK 에러
    inout sda,              // I2C Data (양방향)
    output reg scl          // I2C Clock
);

    // State 정의
    localparam IDLE        = 4'd0;
    localparam START_COND  = 4'd1;
    localparam ADDR_SEND   = 4'd2;
    localparam ADDR_ACK    = 4'd3;
    localparam DATA_WR     = 4'd4;
    localparam DATA_WR_ACK = 4'd5;
    localparam DATA_RD     = 4'd6;
    localparam DATA_RD_ACK = 4'd7;
    localparam STOP_COND   = 4'd8;
    
    reg [3:0] state, next_state;
    
    // I2C 클럭 생성 (100kHz, 250kHz quarter period)
    // 100MHz / 400 = 250kHz (quarter period)
    localparam CLK_DIV = 16'd250;
    reg [15:0] clk_counter;
    reg [1:0] quarter_tick;  // 0~3: 4분주 틱
    
    // 비트 카운터
    reg [2:0] bit_cnt;
    
    // 데이터 레지스터
    reg [7:0] addr_rw;       // {slave_addr, rw}
    reg [7:0] data_tx;
    reg [7:0] data_rx;
    reg sda_out;
    reg sda_oe;              // SDA output enable
    
    // SDA 양방향 제어
    assign sda = sda_oe ? sda_out : 1'bz;
    wire sda_in = sda;
    
    // 클럭 분주기
    always @(posedge clk or posedge reset) begin
        if (reset) begin
            clk_counter <= 0;
            quarter_tick <= 0;
        end
        else begin
            if (state == IDLE) begin
                clk_counter <= 0;
                quarter_tick <= 0;
            end
            else if (clk_counter == CLK_DIV - 1) begin
                clk_counter <= 0;
                quarter_tick <= quarter_tick + 1;
            end
            else begin
                clk_counter <= clk_counter + 1;
            end
        end
    end
    
    // State Register
    always @(posedge clk or posedge reset) begin
        if (reset) begin
            state <= IDLE;
            bit_cnt <= 0;
        end
        else begin
            state <= next_state;
            
            // 비트 카운터 관리
            if (state == ADDR_SEND && quarter_tick == 3)
                bit_cnt <= bit_cnt + 1;
            else if (state == DATA_WR && quarter_tick == 3)
                bit_cnt <= bit_cnt + 1;
            else if (state == DATA_RD && quarter_tick == 3)
                bit_cnt <= bit_cnt + 1;
            else if (state != ADDR_SEND && state != DATA_WR && state != DATA_RD)
                bit_cnt <= 0;
        end
    end
    
    // Next State Logic
    always @(*) begin
        next_state = state;
        
        case (state)
            IDLE: begin
                if (start)
                    next_state = START_COND;
            end
            
            START_COND: begin
                if (quarter_tick == 3)
                    next_state = ADDR_SEND;
            end
            
            ADDR_SEND: begin
                if (bit_cnt == 7 && quarter_tick == 3)
                    next_state = ADDR_ACK;
            end
            
            ADDR_ACK: begin
                if (quarter_tick == 3) begin
                    if (sda_in == 0)  // ACK received
                        next_state = rw ? DATA_RD : DATA_WR;
                    else
                        next_state = STOP_COND;  // NACK
                end
            end
            
            DATA_WR: begin
                if (bit_cnt == 7 && quarter_tick == 3)
                    next_state = DATA_WR_ACK;
            end
            
            DATA_WR_ACK: begin
                if (quarter_tick == 3)
                    next_state = STOP_COND;
            end
            
            DATA_RD: begin
                if (bit_cnt == 7 && quarter_tick == 3)
                    next_state = DATA_RD_ACK;
            end
            
            DATA_RD_ACK: begin
                if (quarter_tick == 3)
                    next_state = STOP_COND;
            end
            
            STOP_COND: begin
                if (quarter_tick == 3)
                    next_state = IDLE;
            end
            
            default: next_state = IDLE;
        endcase
    end
    
    // SCL 생성
    always @(posedge clk or posedge reset) begin
        if (reset)
            scl <= 1;
        else begin
            case (state)
                IDLE, START_COND, STOP_COND:
                    scl <= 1;
                default: begin
                    if (quarter_tick == 0 || quarter_tick == 1)
                        scl <= 0;
                    else
                        scl <= 1;
                end
            endcase
        end
    end
    
    // SDA 제어
    always @(posedge clk or posedge reset) begin
        if (reset) begin
            sda_out <= 1;
            sda_oe <= 0;
            addr_rw <= 0;
            data_tx <= 0;
            data_rx <= 0;
        end
        else begin
            case (state)
                IDLE: begin
                    sda_out <= 1;
                    sda_oe <= 1;
                    addr_rw <= {slave_addr, rw};
                    data_tx <= wr_data;
                end
                
                START_COND: begin
                    if (quarter_tick == 2) begin
                        sda_out <= 0;  // START condition
                        sda_oe <= 1;
                    end
                end
                
                ADDR_SEND: begin
                    sda_oe <= 1;
                    if (quarter_tick == 0)
                        sda_out <= addr_rw[7 - bit_cnt];
                end
                
                ADDR_ACK: begin
                    sda_oe <= 0;  // Release for ACK
                end
                
                DATA_WR: begin
                    sda_oe <= 1;
                    if (quarter_tick == 0)
                        sda_out <= data_tx[7 - bit_cnt];
                end
                
                DATA_WR_ACK: begin
                    sda_oe <= 0;  // Release for ACK
                end
                
                DATA_RD: begin
                    sda_oe <= 0;  // Release for reading
                    if (quarter_tick == 2)
                        data_rx[7 - bit_cnt] <= sda_in;
                end
                
                DATA_RD_ACK: begin
                    sda_oe <= 1;
                    sda_out <= 1;  // Master NACK (single byte read)
                end
                
                STOP_COND: begin
                    sda_oe <= 1;
                    if (quarter_tick == 0)
                        sda_out <= 0;
                    else if (quarter_tick == 2)
                        sda_out <= 1;  // STOP condition
                end
            endcase
        end
    end
    
    // Output signals
    always @(posedge clk or posedge reset) begin
        if (reset) begin
            busy <= 0;
            ack_error <= 0;
            rd_data <= 0;
        end
        else begin
            busy <= (state != IDLE);
            
            // ACK 에러 체크
            if (state == ADDR_ACK && quarter_tick == 3 && sda_in == 1)
                ack_error <= 1;
            else if (state == DATA_WR_ACK && quarter_tick == 3 && sda_in == 1)
                ack_error <= 1;
            else if (state == IDLE)
                ack_error <= 0;
            
            // 읽기 데이터 출력
            if (state == DATA_RD_ACK && quarter_tick == 3)
                rd_data <= data_rx;
        end
    end

endmodule
```

```verilog

```

---

## 8️. 게임 FSM (최고급)

```verilog
// ========================================
// 8번 - 게임 FSM (최고급)
// ========================================
// 반응속도 테스트 게임
// LED가 랜덤하게 켜지면 빠르게 버튼을 눌러야 함
// 반응 시간 측정 및 점수 계산
// Basys3 보드 100MHz 클럭 사용

module reaction_game_fsm(
    input clk,              // 100MHz 클럭
    input reset,            // 리셋
    input start_btn,        // 게임 시작 버튼
    input react_btn,        // 반응 버튼
    output reg [15:0] leds, // LED 패턴
    output reg [6:0] seg0,  // 7-segment digit 0 (반응시간 1의 자리)
    output reg [6:0] seg1,  // 7-segment digit 1 (반응시간 10의 자리)
    output reg [6:0] seg2,  // 7-segment digit 2 (반응시간 100의 자리)
    output reg [6:0] seg3,  // 7-segment digit 3 (점수)
    output reg [3:0] an,    // 7-segment anode
    output reg game_over,   // 게임 오버 신호
    output reg buzzer       // 부저 (성공/실패 피드백)
);

    // State 정의
    localparam IDLE         = 4'd0;
    localparam READY        = 4'd1;
    localparam WAIT_RANDOM  = 4'd2;
    localparam LED_ON       = 4'd3;
    localparam MEASURING    = 4'd4;
    localparam SUCCESS      = 4'd5;
    localparam FAIL         = 4'd6;
    localparam SHOW_RESULT  = 4'd7;
    localparam GAME_OVER    = 4'd8;
    
    reg [3:0] state, next_state;
    
    // 타이머 및 카운터
    reg [26:0] ms_counter;      // 1ms 카운터
    reg [15:0] reaction_time;   // 반응 시간 (ms)
    reg [15:0] wait_time;       // 랜덤 대기 시간
    reg [3:0] score;            // 점수 (0~9)
    reg [2:0] round;            // 라운드 (1~5)
    
    // LFSR for pseudo-random number generation
    reg [15:0] lfsr;
    wire lfsr_feedback;
    assign lfsr_feedback = lfsr[15] ^ lfsr[13] ^ lfsr[12] ^ lfsr[10];
    
    // 1ms tick 생성
    wire tick_1ms;
    assign tick_1ms = (ms_counter == 27'd99_999);
    
    always @(posedge clk or posedge reset) begin
        if (reset)
            ms_counter <= 0;
        else begin
            if (tick_1ms)
                ms_counter <= 0;
            else
                ms_counter <= ms_counter + 1;
        end
    end
    
    // 버튼 엣지 감지
    reg start_btn_prev, react_btn_prev;
    wire start_btn_edge, react_btn_edge;
    
    assign start_btn_edge = start_btn & ~start_btn_prev;
    assign react_btn_edge = react_btn & ~react_btn_prev;
    
    always @(posedge clk) begin
        start_btn_prev <= start_btn;
        react_btn_prev <= react_btn;
    end
    
    // LFSR 업데이트
    always @(posedge clk or posedge reset) begin
        if (reset)
            lfsr <= 16'hACE1;  // 초기 시드
        else if (tick_1ms)
            lfsr <= {lfsr[14:0], lfsr_feedback};
    end
    
    // State Register
    always @(posedge clk or posedge reset) begin
        if (reset) begin
            state <= IDLE;
            round <= 0;
            score <= 0;
            reaction_time <= 0;
            wait_time <= 0;
        end
        else begin
            state <= next_state;
            
            // 대기 시간 카운트
            if (state == WAIT_RANDOM && tick_1ms) begin
                if (wait_time > 0)
                    wait_time <= wait_time - 1;
            end
            
            // 반응 시간 측정
            if (state == MEASURING && tick_1ms)
                reaction_time <= reaction_time + 1;
            
            // 라운드 및 점수 관리
            if (state == READY) begin
                round <= round + 1;
                reaction_time <= 0;
                // 랜덤 대기 시간 설정 (1~3초)
                wait_time <= 1000 + (lfsr[10:0] % 2000);
            end
            else if (state == SUCCESS) begin
                // 반응시간에 따른 점수 부여
                if (reaction_time < 200)
                    score <= score + 3;
                else if (reaction_time < 400)
                    score <= score + 2;
                else
                    score <= score + 1;
            end
            else if (state == IDLE) begin
                round <= 0;
                score <= 0;
            end
        end
    end
    
    // Next State Logic
    always @(*) begin
        next_state = state;
        
        case (state)
            IDLE: begin
                if (start_btn_edge)
                    next_state = READY;
            end
            
            READY: begin
                next_state = WAIT_RANDOM;
            end
            
            WAIT_RANDOM: begin
                if (react_btn_edge)
                    next_state = FAIL;  // 너무 빨리 누름
                else if (wait_time == 0)
                    next_state = LED_ON;
            end
            
            LED_ON: begin
                next_state = MEASURING;
            end
            
            MEASURING: begin
                if (react_btn_edge)
                    next_state = SUCCESS;
                else if (reaction_time >= 1000)  // 1초 초과
                    next_state = FAIL;
            end
            
            SUCCESS: begin
                if (tick_1ms && reaction_time >= 1500)  // 1.5초 결과 표시
                    next_state = (round >= 5) ? GAME_OVER : READY;
            end
            
            FAIL: begin
                if (tick_1ms && reaction_time >= 1500)  // 1.5초 결과 표시
                    next_state = (round >= 5) ? GAME_OVER : READY;
            end
            
            GAME_OVER: begin
                if (start_btn_edge)
                    next_state = IDLE;
            end
            
            default: next_state = IDLE;
        endcase
    end
    
    // LED 패턴 제어
    always @(*) begin
        case (state)
            IDLE:
                leds = 16'b0000000000000000;
            READY:
                leds = 16'b1111111111111111;  // 모든 LED 깜빡임
            WAIT_RANDOM:
                leds = 16'b0000000000000000;
            LED_ON, MEASURING:
                // 랜덤 LED 패턴
                leds = {lfsr[15:8], lfsr[7:0]};
            SUCCESS:
                leds = 16'b0101010101010101;  // 체크 패턴
            FAIL:
                leds = 16'b1010101010101010;  // X 패턴
            GAME_OVER:
                leds = score >= 10 ? 16'hFFFF : 16'h0000;  // 최종 점수 표시
            default:
                leds = 16'b0000000000000000;
        endcase
    end
    
    // 부저 제어
    always @(*) begin
        case (state)
            SUCCESS: buzzer = 1;
            FAIL: buzzer = (reaction_time[7:0] < 8'd128);  // 톤 생성
            default: buzzer = 0;
        endcase
    end
    
    // 7-segment 디코더 함수
    function [6:0] decode_7seg;
        input [3:0] digit;
        begin
            case (digit)
                4'd0: decode_7seg = 7'b1000000;
                4'd1: decode_7seg = 7'b1111001;
                4'd2: decode_7seg = 7'b0100100;
                4'd3: decode_7seg = 7'b0110000;
                4'd4: decode_7seg = 7'b0011001;
                4'd5: decode_7seg = 7'b0010010;
                4'd6: decode_7seg = 7'b0000010;
                4'd7: decode_7seg = 7'b1111000;
                4'd8: decode_7seg = 7'b0000000;
                4'd9: decode_7seg = 7'b0010000;
                default: decode_7seg = 7'b1111111;
            endcase
        end
    endfunction
    
    // 7-segment 디스플레이 제어
    always @(*) begin
        an = 4'b0000;  // 모든 digit 활성화
        
        // 반응 시간 표시 (0~999ms)
        seg0 = decode_7seg(reaction_time % 10);
        seg1 = decode_7seg((reaction_time / 10) % 10);
        seg2 = decode_7seg((reaction_time / 100) % 10);
        
        // 점수 표시
        seg3 = decode_7seg(score);
        
        if (state == IDLE || state == GAME_OVER) begin
            // 최종 점수만 표시
            seg0 = 7'b1111111;
            seg1 = 7'b1111111;
            seg2 = 7'b1111111;
            seg3 = decode_7seg(score);
        end
    end
    
    // 게임 오버 플래그
    always @(*) begin
        game_over = (state == GAME_OVER);
    end

endmodul
```

```verilog

```

---
