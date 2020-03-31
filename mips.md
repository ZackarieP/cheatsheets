# MIPS Instruction Set & Pseudoinstructions

## Instructions

### ADD – Add (with overflow)
#### Description
Adds two registers and stores the result in a register

#### Operation
```mips
$d = $s + $t; advance_pc (4);
```

#### Syntax
```mips
add $d, $s, $t
```

### ADDI -- Add immediate (with overflow)
#### Description
Adds a register and a sign-extended immediate value and stores the result in a register

#### Operation
```mips
$t = $s + imm; advance_pc (4);
```

#### Syntax
```mips
addi $t, $s, imm
```

### ADDIU -- Add immediate unsigned (no overflow)
#### Description
Adds a register and a sign-extended immediate value and stores the result in a register

#### Operation
```mips
$t = $s + imm; advance_pc (4);
```

#### Syntax
```mips
addiu $t, $s, imm
```

### ADDU -- Add unsigned (no overflow)
#### Description
Adds two registers and stores the result in a register

#### Operation
```mips
$d = $s + $t; advance_pc (4);
```

#### Syntax
```mips
addu $d, $s, $t
```

### AND -- Bitwise and
#### Description
Bitwise ands two registers and stores the result in a register

#### Operation
```mips
$d = $s & $t; advance_pc (4);
```

#### Syntax
```mips
and $d, $s, $t
```

### ANDI -- Bitwise and immediate
#### Description
Bitwise ands a register and an immediate value and stores the result in a register

#### Operation
```mips
$t = $s & imm; advance_pc (4);
```

#### Syntax
```mips
andi $t, $s, imm
```

### BEQ -- Branch on equal
#### Description
Branches if the two registers are equal

#### Operation
```mips
if $s == $t advance_pc (offset << 2)); else advance_pc (4);
```

#### Syntax
```mips
beq $s, $t, offset
```

### BGEZ -- Branch on greater than or equal to zero
#### Description
Branches if the register is greater than or equal to zero

#### Operation
```mips
if $s >= 0 advance_pc (offset << 2)); else advance_pc (4);
```

#### Syntax
```mips
bgez $s, offset
```

### BGEZAL -- Branch on greater than or equal to zero and link
#### Description
Branches if the register is greater than or equal to zero and saves the return address in $31

#### Operation
```mips
if $s >= 0 $31 = PC + 8 (or nPC + 4); advance_pc (offset << 2)); else advance_pc (4);
```

#### Syntax
```mips
bgezal $s, offset
```

### BGTZ -- Branch on greater than zero
#### Description
Branches if the register is greater than zero

#### Operation
```mips
if $s > 0 advance_pc (offset << 2)); else advance_pc (4);
```

#### Syntax
```mips
bgtz $s, offset
```

### BLEZ -- Branch on less than or equal to zero
#### Description
Branches if the register is less than or equal to zero

#### Operation
```mips
if $s <= 0 advance_pc (offset << 2)); else advance_pc (4);
```

#### Syntax
```mips
blez $s, offset
```

### BLTZ -- Branch on less than zero
#### Description
Branches if the register is less than zero

#### Operation
```mips
if $s < 0 advance_pc (offset << 2)); else advance_pc (4);
```

#### Syntax
```mips
bltz $s, offset
```

### BLTZAL -- Branch on less than zero and link
#### Description
Branches if the register is less than zero and saves the return address in $31

#### Operation
```mips
if $s < 0 $31 = PC + 8 (or nPC + 4); advance_pc (offset << 2)); else advance_pc (4);
```

#### Syntax
```mips
bltzal $s, offset
```

### BNE -- Branch on not equal
#### Description
Branches if the two registers are not equal

#### Operation
```mips
if $s != $t advance_pc (offset << 2)); else advance_pc (4);
```

#### Syntax
```mips
bne $s, $t, offset
```

### DIV -- Divide
#### Description
Divides $s by $t and stores the quotient in $LO and the remainder in $HI

#### Operation
```mips
$LO = $s / $t; $HI = $s % $t; advance_pc (4);
```

#### Syntax
```mips
div $s, $t
```

### DIVU -- Divide unsigned
#### Description
Divides $s by $t and stores the quotient in $LO and the remainder in $HI

#### Operation
```mips
$LO = $s / $t; $HI = $s % $t; advance_pc (4);
```

#### Syntax
```mips
divu $s, $t
```

### J -- Jump
#### Description
Jumps to the calculated address

#### Operation
```mips
PC = nPC; nPC = (PC & 0xf0000000) | (target << 2);
```

#### Syntax
```mips
j target
```

### JAL -- Jump and link
#### Description
Jumps to the calculated address and stores the return address in $31

#### Operation
```mips
$31 = PC + 8 (or nPC + 4); PC = nPC; nPC = (PC & 0xf0000000) | (target << 2);
```

#### Syntax
```mips
jal target
```

### JR -- Jump register
#### Description
Jump to the address contained in register $s

#### Operation
```mips
PC = nPC; nPC = $s;
```

#### Syntax
```mips
jr $s
```

### LB -- Load byte
#### Description
A byte is loaded into a register from the specified address.

#### Operation
```mips
$t = MEM[$s + offset]; advance_pc (4);
```

#### Syntax
```mips
lb $t, offset($s)
```

### LUI -- Load upper immediate
#### Description
The immediate value is shifted left 16 bits and stored in the register. The lower 16 bits are zeroes.

#### Operation
```mips
$t = (imm << 16); advance_pc (4);
```

#### Syntax
```mips
lui $t, imm
```

### LW -- Load word
#### Description
A word is loaded into a register from the specified address.

#### Operation
```mips
$t = MEM[$s + offset]; advance_pc (4);
```

#### Syntax
```mips
lw $t, offset($s)
```

### MFHI -- Move from HI
#### Description
The contents of register HI are moved to the specified register.

#### Operation
```mips
$d = $HI; advance_pc (4);
```

#### Syntax
```mips
mfhi $d
```

### MFLO -- Move from LO
#### Description
The contents of register LO are moved to the specified register.

#### Operation
```mips
$d = $LO; advance_pc (4);
```

#### Syntax
```mips
mflo $d
```

### MULT -- Multiply
#### Description
Multiplies $s by $t and stores the result in $LO.

#### Operation
```mips
$LO = $s * $t; advance_pc (4);
```

#### Syntax
```mips
mult $s, $t
```

### MULTU -- Multiply unsigned
#### Description
Multiplies $s by $t and stores the result in $LO.

#### Operation
```mips
$LO = $s * $t; advance_pc (4);
```

#### Syntax
```mips
multu $s, $t
```

### NOOP -- no operation
#### Description
Performs no operation.

#### Operation
```mips
advance_pc (4);
```

#### Syntax
```mips
noop
```


Note: The encoding for a NOOP represents the instruction SLL $0, $0, 0 which has no side effects. In fact, nearly every instruction that has $0 as its destination register will have no side effect and can thus be considered a NOOP instruction.

### OR -- Bitwise or
#### Description
Bitwise logical ors two registers and stores the result in a register

#### Operation
```mips
$d = $s | $t; advance_pc (4);
```

#### Syntax
```mips
or $d, $s, $t
```

### ORI -- Bitwise or immediate
#### Description
Bitwise ors a register and an immediate value and stores the result in a register

#### Operation
```mips
$t = $s | imm; advance_pc (4);
```

#### Syntax
```mips
ori $t, $s, imm
```

### SB -- Store byte
#### Description
The least significant byte of $t is stored at the specified address.

#### Operation
```mips
MEM[$s + offset] = (0xff & $t); advance_pc (4);
```

#### Syntax
```mips
sb $t, offset($s)
```

### SLL -- Shift left logical
#### Description
Shifts a register value left by the shift amount listed in the instruction and places the result in a third register. Zeroes are shifted in.

#### Operation
```mips
$d = $t << h; advance_pc (4);
```

#### Syntax
```mips
sll $d, $t, h
```

### SLLV -- Shift left logical variable
#### Description
Shifts a register value left by the value in a second register and places the result in a third register. Zeroes are shifted in.

#### Operation
```mips
$d = $t << $s; advance_pc (4);
```

#### Syntax
```mips
sllv $d, $t, $s
```

### SLT -- Set on less than (signed)
#### Description
If $s is less than $t, $d is set to one. It gets zero otherwise.

#### Operation
```mips
if $s < $t $d = 1; advance_pc (4); else $d = 0; advance_pc (4);
```

#### Syntax
```mips
slt $d, $s, $t
```

### SLTI -- Set on less than immediate (signed)
#### Description
If $s is less than immediate, $t is set to one. It gets zero otherwise.

#### Operation
```mips
if $s < imm $t = 1; advance_pc (4); else $t = 0; advance_pc (4);
```

#### Syntax
```mips
slti $t, $s, imm
```

### SLTIU -- Set on less than immediate unsigned
#### Description
If $s is less than the unsigned immediate, $t is set to one. It gets zero otherwise.

#### Operation
```mips
if $s < imm $t = 1; advance_pc (4); else $t = 0; advance_pc (4);
```

#### Syntax
```mips
sltiu $t, $s, imm
```

### SLTU -- Set on less than unsigned
#### Description
If $s is less than $t, $d is set to one. It gets zero otherwise.

#### Operation
```mips
if $s < $t $d = 1; advance_pc (4); else $d = 0; advance_pc (4);
```

#### Syntax
```mips
sltu $d, $s, $t
```

### SRA -- Shift right arithmetic
#### Description
Shifts a register value right by the shift amount (shamt) and places the value in the destination register. The sign bit is shifted in.

#### Operation
```mips
$d = $t >> h; advance_pc (4);
```

#### Syntax
```mips
sra $d, $t, h
```

### SRL -- Shift right logical
#### Description
Shifts a register value right by the shift amount (shamt) and places the value in the destination register. Zeroes are shifted in.

#### Operation
```mips
$d = $t >> h; advance_pc (4);
```

#### Syntax
```mips
srl $d, $t, h
```

### SRLV -- Shift right logical variable
#### Description
Shifts a register value right by the amount specified in $s and places the value in the destination register. Zeroes are shifted in.

#### Operation
```mips
$d = $t >> $s; advance_pc (4);
```

#### Syntax
```mips
srlv $d, $t, $s
```

### SUB -- Subtract
#### Description
Subtracts two registers and stores the result in a register

#### Operation
```mips
$d = $### s - $t; advance_pc (4);
```

#### Syntax
```mips
sub $d, $s, $t
```

### SUBU -- Subtract unsigned
#### Description
Subtracts two registers and stores the result in a register

#### Operation
```mips
$d = $### s - $t; advance_pc (4);
```

#### Syntax
```mips
subu $d, $s, $t
```

### SW -- Store word
#### Description
The contents of $t is stored at the specified address.

#### Operation
```mips
MEM[$s + offset] = $t; advance_pc (4);
```

#### Syntax
```mips
sw $t, offset($s)
```

### SYSCALL -- System call
#### Description
Generates a software interrupt.

#### Operation
```mips
advance_pc (4);
```

#### Syntax
```mips
syscall
```

### XOR -- Bitwise exclusive or
#### Description
Exclusive ors two registers and stores the result in a register

#### Operation
```mips
$d = $s ^ $t; advance_pc (4);
```

#### Syntax
```mips
xor $d, $s, $t
```

### XORI -- Bitwise exclusive or immediate
#### Description
Bitwise exclusive ors a register and an immediate value and stores the result in a register

#### Operation
```mips
$t = $s ^ imm; advance_pc (4);
```

#### Syntax
```mips
xori $t, $s, imm
```
