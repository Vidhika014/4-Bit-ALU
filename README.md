# 4-Bit Arithmetic Logic Unit (ALU) on FPGA

A Verilog HDL implementation and hardware verification of a **4-bit Arithmetic Logic Unit (ALU)** mapped onto the **Real Digital Boolean Board (AMD / Xilinx Spartan-7 FPGA)**. The project executes multiple arithmetic and logic operations based on switch inputs and renders the computed output directly on an onboard 7-segment display.

---

## Overview

The 4-bit ALU processes two 4-bit operands (`a[3:0]` and `b[3:0]`) using a 3-bit operation select code (`sel[2:0]`). The intermediate 4-bit binary result is decoded into active-low 7-segment cathode signals (`seg[6:0]`) to display hexadecimal outputs (`0` to `F`) on a chosen display digit.

### Functional Truth Table

| `sel[2:0]` | Operation Expression | Description |
| :--- | :--- | :--- |
| `3'b000` | `y = 4'b0000` | **No Operation (NOP) / Clear** |
| `3'b001` | `y = a + b` | **Addition** |
| `3'b010` | `y = a - b` | **Subtraction** |
| `3'b011` | `y = a & b` | **Bitwise AND** |
| `3'b100` | `y = a \| b` | **Bitwise OR** |
| `3'b101` | `y = ~a` | **Bitwise NOT of A** |
| `3'b110` | `y = ~b` | **Bitwise NOT of B** |
| `3'b111` | `y = 4'b0000` | **No Operation (NOP)** |

---

## RTL Architecture & Source Code

The RTL implementation consists of:
* **`top_module`**: Top-level interface routing external board pins to internal submodules.
* **`ALU`**: Combinational execution unit utilizing a `case(sel)` statement to perform arithmetic and boolean functions.
* **`seg_7`**: Active-low hex to 7-segment cathode decoder.

![Verilog Source Code](Screenshot%202026-08-19%20175023.png)

---

## Pin Constraints (XDC)

The design is configured for 3.3V I/O logic (`LVCMOS33`) with the following hardware pin mappings:

| Signal Name | Hardware Interface | FPGA Package Pin | Signal Name | Hardware Interface | FPGA Package Pin |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `a[0]` | Switch `SW0` | **`V2`** | `D1_AN[0]` | Digit 0 Anode Enable | **`H3`** |
| `a[1]` | Switch `SW1` | **`U2`** | `D1_AN[1]` | Digit 1 Anode Enable | **`J4`** |
| `a[2]` | Switch `SW2` | **`U1`** | `D1_AN[2]` | Digit 2 Anode Enable | **`F3`** |
| `a[3]` | Switch `SW3` | **`T2`** | `D1_AN[3]` | Digit 3 Anode Enable | **`E4`** |
| `b[0]` | Switch `SW4` | **`T1`** | `seg[0]` | Segment A Cathode | **`F4`** |
| `b[1]` | Switch `SW5` | **`R2`** | `seg[1]` | Segment B Cathode | **`J3`** |
| `b[2]` | Switch `SW6` | **`R1`** | `seg[2]` | Segment C Cathode | **`D2`** |
| `b[3]` | Switch `SW7` | **`P2`** | `seg[3]` | Segment D Cathode | **`C2`** |
| `sel[0]` | Switch `SW8` | **`L1`** | `seg[4]` | Segment E Cathode | **`B1`** |
| `sel[1]` | Switch `SW9` | **`K2`** | `seg[5]` | Segment F Cathode | **`H4`** |
| `sel[2]` | Switch `SW10` | **`K1`** | `seg[6]` | Segment G Cathode | **`D1`** |

![Constraint File](Screenshot%202026-08-19%20175046.png)

---

## Hardware Testing & Results

The bitstream was deployed to the Spartan-7 FPGA on the Boolean board. The hardware outputs match the functional ALU specifications across tested switch inputs:

![Hardware Test Verification](Screenshot%202026-08-19%20175105.png)

* **Fig. 1 (Addition)**: When `sel = 001`, the ALU calculates `a + b` and outputs the arithmetic sum.
* **Fig. 2 (Bitwise NOT of A)**: When `sel = 101`, the ALU inverts operand `a`.
* **Fig. 3 (Bitwise AND)**: When `sel = 011`, the ALU calculates `a & b`.
* **Fig. 4 (No Operation)**: When `sel = 000` (or `111`), the output defaults to `0`.

---

## How to Run

1. Clone the repository:
   ```bash
   git clone [https://github.com/your-username/4bit-alu-fpga.git](https://github.com/your-username/4bit-alu-fpga.git)
