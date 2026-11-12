# NanoTekSpice

A digital logic and electronics simulator built in **C++20** as part of the Epitech curriculum.  
**NanoTekSpice** constructs a directed graph of digital electronic components (elementary gates, CMOS 4000 series, RAM/ROM chipsets) from configuration files (`.nts`) and simulates their behavior using **tristate boolean logic**.

---

## Features

- **Tristate Logic System**: Simulates realistic hardware states handling `0` (`False`), `1` (`True`), and `U` (`Undefined`).
- **Graph-Based Circuit Simulation**: Dynamic resolution of pin-to-pin links and dependency propagation.
- **Factory Design Pattern**: Generic and extensible component factory instantiating standard and specialized chips.
- **Interactive CLI Shell**: Real-time circuit control (simulate ticks, inspect pin states, toggle inputs, and run infinite clock loops).
- **Graphical Interface (Bonus)**: Interactive visualization of the circuit and components rendered with **SFML**.
- **Comprehensive CMOS Library**: From elementary boolean gates up to counters, shift registers, decoders, RAM, and ROM.

---

## Supported Components

### Special Components
| Component | Description |
| :--- | :--- |
| `input` | Circuit input pin (values: `0`, `1`, `U`) |
| `output` | Circuit output pin displaying computed value |
| `clock` | Square-wave clock source toggling on each simulation tick |
| `true` | Constant positive logic supply (`1`) |
| `false` | Constant ground logic supply (`0`) |
| `logger` | Triggers byte logging on clock transitions |

### Elementary Gates
- `and`, `or`, `xor`, `not`

### Gates (CMOS 4000 Series)
- **4001**: Quad 2-input NOR gate
- **4011**: Quad 2-input NAND gate
- **4030**: Quad 2-input XOR gate
- **4069**: Hex inverter (NOT gate)
- **4071**: Quad 2-input OR gate
- **4081**: Quad 2-input AND gate

### Advanced Chipsets
- **4008**: 4-bit binary full adder with look-ahead carry
- **4013**: Dual D-type flip-flop with set/reset
- **4017**: 5-stage Johnson decade counter / divider with 10 decoded outputs
- **4040**: 12-stage ripple carry binary counter
- **4094**: 8-stage shift-and-store bus register
- **4512**: 8-channel data selector (multiplexer) with 3-state output
- **4514**: 4-bit latch / 4-to-16 line decoder (demultiplexer)
- **4801**: 1K × 8-bit static CMOS RAM
- **2716**: 16K (2K × 8-bit) UV erasable PROM

---

## Getting Started

### Prerequisites

- C++20 compliant compiler (`clang++` or `g++`)
- `make`
- *(Optional for GUI Bonus)*: `SFML` (graphics, window, system)

### Compilation

```bash
# Build the CLI simulator
make

# Build the graphical SFML interface (bonus)
make bonus

# Clean object files
make clean

# Remove all binaries and artifacts
make fclean

# Rebuild the project
make re
```

---

## Usage

Run the simulator by providing a circuit configuration file (`.nts`):

```bash
./nanotekspice [circuit_file.nts]
```

### Interactive Shell Commands

| Command | Description |
| :--- | :--- |
| `display` | Prints current tick and the values of all inputs and outputs |
| `simulate` | Runs one simulation step (updates clocks, resolves graph) |
| `loop` | Continuously simulates the circuit until interrupted (`Ctrl+C`) |
| `[input]=[value]` | Updates an input pin value (`0`, `1`, or `U`) |
| `exit` | Exits the simulator |
| `Ctrl+D` | EOF — Exits with return code 0 |

---

## Configuration File Format (`.nts`)

Circuits are defined with two mandatory sections:

```plaintext
.chipsets:
input a
input b
clock clk
4071 or_gate
output s

.links:
a:1 or_gate:1
b:1 or_gate:2
or_gate:3 s:1
```

1. **`.chipsets:`** declares component types and their unique instance names.
2. **`.links:`** establishes bidirectional connections between component pins (`component:pin`).

---

## Architecture

```
├── include/
│   ├── IComponent.hpp          # Base component interface & Tristate operators
│   ├── Circuit.hpp             # Circuit container and graph manager
│   ├── Factory.hpp             # Component factory registry
│   ├── Parser.hpp              # .nts file parser and lexical analyzer
│   ├── Shell.hpp               # Interactive CLI command loop
│   └── Exception.hpp           # Custom exception hierarchy
├── src/
│   ├── components/             # Implementations (Special, Elementary, Gates, Advanced)
│   ├── Circuit.cpp
│   ├── Factory.cpp
│   ├── Parser.cpp
│   ├── Shell.cpp
│   └── main.cpp
└── bonus/
    ├── GraphicInterface.cpp    # SFML graphical interface
    ├── GraphicComponent.cpp    # Interactive UI nodes
    └── main_gui.cpp
```

---

## Documentation

API documentation can be generated using Doxygen:

```bash
doxygen Doxyfile
```

The generated HTML documentation will be placed in the `html/` director
