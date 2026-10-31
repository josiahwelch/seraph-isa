# Seraph Instruction Set

## Purpose
The Seraph standard is the instruction set (ISA) for the Arcus Personal Computer. It is designed to be
implemented by the Ardens core, and the core will be initially implemented on a field-programmable gate array (FPGA).
The Arcorum compiler will target Seraph. The minimum integer ISA that can run Seraph binaries and compiled Ardor
programs on bare metal will be v0.

## Goals
1. Implementable on an FPGA.
2. Targetable by the Arcorum compiler.
3. Backwards compatible with v0.
4. Legible when written by hand.
5. Performant and efficient.

## Versions

### v0

#### Features
* Integer instructions
* Multiplication
* Register banks with manual switching
* Traps to a fixed address

#### Deferred
* Division
* Interrupt handling
* Privilege levels
* Advanced memory management; MMU
* 64-bit support
* Compressed instructions
* Floating point
* Atomics

## Tie-breakers
1. Every bit pattern either does something defined or traps.
2. If two simple instructions can do it, then a more complex one will not be implemented.
3. An instruction performs the same function regardless of mode.
4. Designed to suit the use cases of the Arcus Personal Computer, not any other.

