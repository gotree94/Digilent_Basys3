# Basys 3 Quad SPI Flash Programming Guide

## 개요

Basys 3 보드는 Xilinx XC7A35T FPGA와 Quad SPI Flash (Macronix MX25L3273F, 32Mbit/4MB)를 탑재하고 있습니다.
`.bit` 파일을 `.mcs`로 변환한 후 SPI Flash에 기록하면 전원을 껐다 켜도 설계가 자동으로 로드됩니다.

---

## MCS vs PRM 파일 차이

| 파일   | 설명                                              |
|--------|--------------------------------------------------|
| `.mcs` | FPGA 설정 데이터 (Intel HEX 포맷) - Flash에 기록되는 실제 데이터 |
| `.prm` | Flash 프로그래밍 명령 시퀀스 파일 - Vivado가 Flash를 기록할 때 사용하는 내부 지시서 |

> **둘 다 필요합니다.** Vivado가 프로그래밍 시 `.prm` 파일을 참조하여 적절한 erase/program 명령을 보냅니다.

---

## 방법 1: Vivado Hardware Manager 사용 (GUI)

### 1단계: Hardware Manager 열기

1. Vivado에서 **Flow Navigator → Open Hardware Manager** 클릭
2. 보드가 연결되어 있다면 자동으로 감지됨
3. **Open Target → Auto Connect** 클릭

### 2단계: Flash 디바이스 프로그래밍

1. Hardware Manager에서 FPGA 디바이스를 마우스 오른쪽 클릭
2. **Add Configuration Memory Device...** 선택
3. Memory Part 검색창에서以下 입력:

   | 항목       | 값                              |
   |-----------|----------------------------------|
   | Manufacturer | Macronix                      |
   | Type        | SPI Flash                      |
   | Density     | 32Mb                           |
   | Part Name   | **MX25L3273F**                  |

4. **OK** 클릭

### 3단계: MCS 파일 생성 및 기록

1. "Do you want to program the configuration memory device now?" → **OK**
2. **Configuration Memory File** 창에서:
   - **Add Configuration Memory Device** 또는 **Program Configuration Memory Device** 클릭
   - **Configuration file** 항목에서 **Browse** → `.bit` 파일 선택
3. Program Configuration Memory 창에서 **Program** 클릭
4. 자동으로 `.bit` → `.mcs` 변환 후 Flash 기록이 수행됨

---

## 방법 2: Vivado Tcl 콘솔 사용 (CLI)

### 중요: Tcl 경로 형식

Vivado Tcl 콘솔에서는 반드시 **'/'(forward slash)**를 사용해야 합니다.
백슬래시(`\`)는 Tcl escape 문자로 해석되어 경로 오류가 발생합니다.

```tcl
# 올바른 경로 이동
cd "C:/Users/Administrator/Desktop/Basys-3-GPIO-hw.xpr/hw"

# 잘못된 경로 이동 (백슬래시 사용)
# cd "C:\Users\Administrator\Desktop\Basys-3-GPIO-hw.xpr\hw"  ← 오류 발생
```

### 1단계: Vivado Tcl Console 열기

Vivado 메뉴: **Tools → Run Tcl Command...** 또는 하단 Tcl Console 탭

### 2단계: 디렉토리 변경

```tcl
cd "C:/Users/Administrator/Desktop/Basys-3-GPIO-hw.xpr/hw"
```

### 3단계: 비트스트림을 MCS로 변환

```tcl
write_cfgmem -format MCS -size 4 -interface SPIx4 -loadbit "up 0x0 ./hw.runs/impl_1/GPIO_demo.bit" -file ./flash_output.mcs -force
```

| 옵션            | 설명                                    |
|----------------|----------------------------------------|
| `-format MCS`  | 출력 포맷 (MCS, HEX 등 가능)           |
| `-size 4`      | Flash 크기 (4MB = 32Mbit, MX25L3273F) |
| `-interface SPIx4` | Basys 3 QSPI 인터페이스          |
| `-loadbit`     | `"up 0x0 <bitfile>"` 형식 (keyword 필수) |
| `-file`        | 출력 MCS 파일 경로                     |
| `-force`       | 기존 파일 덮어쓰기                    |

> **주의:** `-loadbit` 값에는 반드시 `up` 또는 `down` 키워드가 포함되어야 합니다.
> 잘못된 예: `"0x0 ./file.bit"` → 올바른 예: `"up 0x0 ./file.bit"`

### 4단계: Flash에 기록

```tcl
open_hw_manager
connect_hw_server
open_hw_target
set_property PROGRAM.FILE {C:/Users/Administrator/Desktop/flash_output.mcs} [current_hw_device]
program_hw_cfgmem -hw_cfgmem [get_cfgmem_parts {mx25l3273f-spi-x1_x2_x4}]
```

### 전체 자동화 스크립트 예시

```tcl
# 설정
cd "C:/Users/Administrator/Desktop/Basys-3-GPIO-hw.xpr/hw"
set BIT_FILE "./hw.runs/impl_1/GPIO_demo.bit"
set MCS_FILE "./flash_output.mcs"

# 1. MCS 파일 생성
write_cfgmem -format MCS -size 4 -interface SPIx4 -loadbit "up 0x0 $BIT_FILE" -file $MCS_FILE -force

# 2. 하드웨어 연결
open_hw_manager
connect_hw_server
open_hw_target

# 3. 프로그래밍
set cfgmem_part "mx25l3273f-spi-x1_x2_x4"
create_hw_cfgmem -hw_device [lindex [get_hw_devices] 0] -mem_dev [get_cfgmem_parts $cfgmem_part]
set cfgmem [get_property PROGRAM.HW_CFGMEM [lindex [get_hw_devices] 0]]
set_property PROGRAM.ADDRESS_RANGE  {use_file} $cfgmem
set_property PROGRAM.FILES          $MCS_FILE $cfgmem
set_property PROGRAM.PRM_FILE      {} $cfgmem
set_property PROGRAM.UNUSED_PIN     {pull-none} $cfgmem
set_property PROGRAM.BLANK_CHECK    0 $cfgmem
set_property PROGRAM.ERASE          1 $cfgmem
set_property PROGRAM.CFG_PROGRAM    1 $cfgmem
set_property PROGRAM.VERIFY         1 $cfgmem
program_hw_cfgmem -hw_cfgmem $cfgmem

# 4. 종료
close_hw_target
disconnect_hw_server
```

---

## 방법 3: Vitis Unified IDE 사용

1. Vitis에서 **Xilinx → Program Device** 메뉴 사용
2. Flash 선택 시 **Configuration Memory File** 옵션 선택
3. `.bit` 파일 경로 지정 후 Program 클릭

---

## 방법 4: 보드에서 직접 Program (USB cable)

### 부팅 설정 확인

Basys 3 보드의 **JP4 jumper** 설정:

| JP4 위치  | 동작                           |
|----------|-------------------------------|
| USB      | USB에서 비트스트림 다운로드 (JTAG) |
| QSPI     | Flash에서 부팅                 |

Flash 프로그래밍 후 **JP4를 QSPI로 변경**하면 전원 인가 시 Flash에서 자동 부팅됩니다.

---

## 확인 방법

### Flash에서 부팅 확인

1. 보드 전원 OFF
2. JP4를 QSPI로 설정
3. 전원 ON → 설계가 자동으로 동작하는지 확인

### Flash 읽기 및 검증

Tcl Console에서:

```tcl
# Flash 내용을 파일로 읽기
read_cfgmem -hardware [get_hw_devices xc7a35t_0] -file ./flash_readback.bin -format BIN -force -blank_check

# 기존 MCS와 비교 (선택)
```

---

## 문제 해결

| 증상                           | 해결 방법                                    |
|-------------------------------|---------------------------------------------|
| "No configuration memory device found" | Add Configuration Memory Device에서 올바른 파트 선택 |
| Flash 프로그래밍 실패          | USB 케이블 연결 확인, 보드 리셋 후 재시도      |
| 부팅 시 FPGA 동작 안 함        | JP4 jumper가 QSPI 위치인지 확인               |
| MCS 파일 생성 실패            | 비트스트림 파일 경로 확인, -force 옵션 사용   |
| Flash 파트 인식 실패          | Basys 3 리비전 확인 후 해당 파트 사용          |
| Cannot parse string error     | `-loadbit`에 `up` 키워드 추가 필요           |
| cd 경로 오류                  | 백슬래시 대신 슬래시(`/`) 사용               |
| BPIx16 interface error        | `-interface SPIx4` 로 변경 필요              |

### 자주 발생하는 write_cfgmem 오류

**오류 1: Cannot parse string**
```
ERROR: [Writecfgmem 68-6] Cannot parse string "0x0 ./hw.runs/impl_1/GPIO_demo.bit".
```
**원인:** `-loadbit` 값에 `up` 키워드 누락
**해결:** `"up 0x0 ./hw.runs/impl_1/GPIO_demo.bit"` 으로 변경

**오류 2: BPIx16 interface 오류**
```
INFO: [Writecfgmem 68-23] Start address provided has been multiplied by a factor of 2 due to the use of interface BPIX16.
```
**원인:** Basys 3는 SPI Flash인데 BPIx16 지정
**해결:** `-interface SPIx4` 로 변경

---

## Basys 3 Flash 디바이스 파트 번호 참고

| Flash 칩                 | 크기      | Vivado 파트 번호                     |
|--------------------------|----------|--------------------------------------|
| Macronix MX25L3273F      | 32Mbit   | mx25l3273f-spi-x1_x2_x4            |
| Macronix MX25L12833F     | 128Mbit  | mx25l12833f-spi-x1_x2_x4           |
| Spansion S25FL128S       | 128Mbit  | s25fl128sxxxxxx0-1.2                |

> 보드에 장착된 Flash 칩은 보드 하단에서 확인할 수 있습니다.

---

*작성일: 2026-07-12*
*최종 수정: 2026-07-12*
*대상 프로젝트: hw.xpr (xc7a35tcpg236-1, Basys 3)*
*비트스트림 파일: hw.runs/impl_1/GPIO_demo.bit*
*Flash 칩: Macronix MX25L3273F (32Mbit/4MB)*
