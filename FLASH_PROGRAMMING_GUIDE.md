# Basys 3 <br> Quad SPI Flash Programming Guide

## 개요

Basys 3 보드는 Xilinx XC7A35T FPGA와 Quad SPI Flash (Winbond W25Q128, 16MB)를 탑재하고 있습니다.
`.bit` 파일을 `.mcs`로 변환한 후 SPI Flash에 기록하면 전원을 껐다 켜도 설계가 자동으로 로드됩니다.

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
   | Manufacturer | Spansion (또는 Macronix)       |
   | Type        | FlasH                           |
   | Density     | 128Mb                           |
   | Part Name   | **S25FL128Sxxxxxx0** 또는 **MX25L12833F** |

   > Basys 3 rev. C 이상: Spansion S25FL128S 사용
   > Basys 3 rev. D 이상: Macronix MX25L12833F 사용

4. **OK** 클릭

### 3단계: MCS 파일 생성 및 기록

1. "Do you want to program the configuration memory device now?" → **OK**
2. **Configuration Memory File** 창에서:
   - **Add Configuration Memory Device** 또는 **Program Configuration Memory Device** 클릭
   - **Configuration file** 항목에서 **Browse** → `.bit` 파일 선택
3. Program Configuration Memory 창에서 **Program** 클릭
4. 자동으로 `.bit` → `.mcs` 변환 후 Flash 기록이 수행됨

### 수동으로 MCS 파일 생성 방법

Hardware Manager Tcl 콘솔에서:

```tcl
write_cfgmem -format MCS -size 16 -interface BPIx16 -loadbit "0x0 ./hw.runs/impl_1/GPIO_demo.bit" -file ./flash_output.mcs -force
```

> **참고:** Basys 3는 QSPI이므로 interface는 자동 감지됩니다.
> Vivado 2022.2 기준 `-interface SPIx4`가 기본값입니다.

---

## 방법 2: Vivado Tcl 콘솔 사용 (CLI)

### 1단계: Vivado Tcl Console 열기

Vivado 메뉴: **Tools → Run Tcl Command...** 또는 하단 Tcl Console 탭

### 2단계: 비트스트림을 MCS로 변환

```tcl
write_cfgmem -format MCS -size 16 -interface SPIx4 -loadbit "0x0 C:/Users/Administrator/Desktop/Basys-3-GPIO-hw.xpr/hw/hw.runs/impl_1/GPIO_demo.bit" -file C:/Users/Administrator/Desktop/flash_output.mcs -force
```

| 옵션        | 설명                                    |
|------------|----------------------------------------|
| `-format MCS` | 출력 포맷 (MCS, HEX 등 가능)           |
| `-size 16`    | Flash 크기 (16MB = 128Mbit)           |
| `-interface SPIx4` | Basys 3 QSPI 인터페이스          |
| `-loadbit`    | 입력 비트스트림 파일 경로              |
| `-file`       | 출력 MCS 파일 경로                     |
| `-force`      | 기존 파일 덮어쓰기                    |

### 3단계: Flash에 기록

```tcl
open_hw_manager
connect_hw_server
open_hw_target
set_property PROGRAM.FILE {C:/Users/Administrator/Desktop/flash_output.mcs} [current_hw_device]
program_hw_cfgmem -hw_cfgmem [get_cfgmem_parts {s25fl128sxxxxxx0-1.2}]
```

### 전체 자동화 스크립트 예시

```tcl
# 설정
set BIT_FILE "C:/Users/Administrator/Desktop/Basys-3-GPIO-hw.xpr/hw/hw.runs/impl_1/GPIO_demo.bit"
set MCS_FILE "C:/Users/Administrator/Desktop/flash_output.mcs"

# 1. MCS 파일 생성
write_cfgmem -format MCS -size 16 -interface SPIx4 -loadbit "0x0 $BIT_FILE" -file $MCS_FILE -force

# 2. 하드웨어 연결
open_hw_manager
connect_hw_server
open_hw_target

# 3. 프로그래밍
set cfgmem_part "s25fl128sxxxxxx0-1.2"
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

## 방법 3: vitis-ide (Unified IDE) 사용

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

---

## Basys 3 Flash 디바이스 파트 번호 참고

| 리비전 | Flash 칩                 | Vivado 파트 번호                     |
|--------|--------------------------|--------------------------------------|
| Rev. C | Spansion S25FL128S       | s25fl128sxxxxxx0-1.2                |
| Rev. D | Macronix MX25L12833F     | mx25l12833f-spi-x1_x2_x4           |

> Basys 3 리비전은 보드 하단 스티커에서 확인할 수 있습니다.

---

*작성일: 2026-07-12*
*대상 프로젝트: hw.xpr (xc7a35tcpg236-1, Basys 3)*
*비트스트림 파일: hw.runs/impl_1/GPIO_demo.bit*
