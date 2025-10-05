# FSM

---

## 1. Toggle FSM (입문)


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

---

## 2️. 시퀀스 FSM (입문)

---

## 3️. 신호등 FSM (초급)

---

## 4️. 자판기 FSM (중급)

---

## 5️. UART 수신기 FSM (중상급)

---

## 6️. 엘리베이터 FSM (상급)

---

## 7️. I2C Master FSM (고급)

---

## 8️. 게임 FSM (최고급)

---
