# Architectural State

## General Purpose Registers

* 32 32-bit general purpose registers (_r0_ - _r31_).
* _r0_ acts as the null register, reads as 0 and discards all writes.
* Call instructions write the return address to whatever `rd` names.
* Stack pointer, return address, and argument registers are defined in ABI.

## Program Counter (PC)

* 32 bits wide.
* 4-byte alignment in v0; 2-byte with compressed instructions.
* Misaligned jump or branch traps.
* Reset PC at `0x00000000`.

## Register Banks

* 16 banks (_b0_ - _b15_) of 32 registers.
* No registers shared between banks.
* Current bank is selected by writing to _STATUS_; takes effect on the next instruction.
* On a trap, the current bank is saved to _STATUS.PREVBANK_ and the CPU switches to _b0_.
* A trap taken in _b0_ is a double fault and it causes a CPU halt.
* _b0_ is reserved for the kernel and trap handler.
* Move from bank: `rd <- b[n].rs`.
* Move to bank: `b[n].rd <- rs`.

## Control/Status Registers (CSRs)

* 12-bit CSR address space.
* Only accessible through CSR instructions.
* Unassigned CSR addresses are reserved; when one is accessed, it traps

### v0

* _STATUS_: current (bits 3:0) and previous bank (bits 7:4); remaining bits reserved for interrupt-enable and privilege.
* _EPC_: address of the instruction that trapped.
* _CAUSE_: trap reason (illegal instruction, misaligned access, syscall, breakpoint, invalid CSR).
* _TVAL_: the faulting address or the faulting instruction's bits.
* _CYCLE_/ _CYCLEH_: 64-bit cycle count; read-only; high and low halves.
* _ID_: core and ISA version; read-only.

## Memory and I/O

* Flat; byte-addressed; 32-bit address space; little-endian
* Memory-mapped I/O.
* The ISA only defines the reset PC and trap vector.

### Trap vector

Fixed at `0x00000100`.

## CPU States

* Running or halted.
* Halts occur on a double fault or a `HALT` instruction.

## Reset

* All registers are 0.
* PC = `0x00000000`.
* Current bank = `b0`.
* _CYCLE_ = `0`.

## Et cetera

* No flags or condition codes.
* No modes; an instruction doesn't change function based on mode.