# 4-Bit Arithmetic Circuit Simulator

A 4-bit arithmetic circuit that performs **Addition, Subtraction, Increment and Decrement** using a 4-bit full adder, 4:1 multiplexers and two control lines (S1, S0). Includes an interactive, gate-level simulator with a live circuit diagram and automatic verification.

**Live demo:** `<add your GitHub Pages link here>`

## Features

- Gate-level simulation (MUX → XOR → AND/OR full adder), not just `+` / `−`
- Live circuit diagram: green wire = logic 1, grey wire = logic 0
- Toggle inputs A and B (4 bits each) and select the operation
- Output in binary, decimal and signed decimal
- Flags: Carry/Borrow, Overflow, Zero
- Verification table plus a one-click test of all 1024 combinations (16 × 16 × 4 operations)

## How it works

One 4-bit full adder is shared by all operations. A 4:1 MUX on each B input decides what goes into the adder, and the carry-in is derived from the control lines.

| S1 | S0 | Operation | MUX output (Y) | Cin = S1 ⊕ S0 | Result |
|----|----|-----------|----------------|---------------|--------|
| 0 | 0 | ADD | B | 0 | A + B |
| 0 | 1 | SUB | B' | 1 | A + B' + 1 = A − B |
| 1 | 0 | INC | 0000 | 1 | A + 0 + 1 = A + 1 |
| 1 | 1 | DEC | 1111 | 0 | A + 1111 = A − 1 |

Subtraction and decrement use 2's complement arithmetic.

### Logic equations

- MUX (per bit): `Yi = S1'·S0'·Bi + S1'·S0·Bi' + S1·S0'·0 + S1·S0·1`
- Carry in: `Cin = S1 ⊕ S0`
- Full adder: `Sum = A ⊕ Y ⊕ Cin`, `Cout = A·Y + Cin·(A ⊕ Y)`
- Overflow (signed): `V = C4 ⊕ C3`
- For SUB and DEC, `Borrow = NOT Cout`

### Block diagram

```
 B3 B2 B1 B0
  |  |  |  |
[MUX][MUX][MUX][MUX] <- S1, S0
  |  |  |  |
 Y3 Y2 Y1 Y0
  |  |  |  |    A3 A2 A1 A0
[FA3]<-[FA2]<-[FA1]<-[FA0]<- Cin = S1 XOR S0
  |  |  |  |
 S3 S2 S1 S0  (+ Cout)
```

## Example results

| A | B | Operation | Result |
|---|---|-----------|--------|
| 0101 (5) | 0011 (3) | ADD | 1000 (8) |
| 1001 (9) | 0110 (6) | SUB | 0011 (3) |
| 1111 (15) | any | INC | 0000 (0, Cout = 1) |
| 0000 (0) | any | DEC | 1111 (15) |

(For INC and DEC the B input is ignored because the MUX supplies a constant.)

## Run it

No installation needed. Open `index.html` (or `arith4.html`) in any browser. Click **Run all 1024 tests** to verify every combination; it should show `1024/1024 PASS`.

## Hardware implementation (optional)

| Component | Purpose |
|-----------|---------|
| 74LS283 | 4-bit full adder |
| 2 × 74LS153 | Four 4:1 MUXes (one per bit) |
| 74LS04 | NOT gates for B' |
| 74LS86 | XOR gate for Cin |

## Project structure

```
.
├── index.html   # simulator (single file, HTML + CSS + JS)
└── README.md
```

## Tech

Plain HTML, CSS and JavaScript with an SVG circuit diagram. No external libraries.

## Author

`<Your name>` — `<College / Course / Year>`

## License

MIT
