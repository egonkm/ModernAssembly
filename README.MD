# Modern Assembly

This project is a proposal to create a modern assembly language, something that looks more like a high level language and makes it easier for new programmers to learn it.
Every CPU architecture has its own peculiarities so an universal assembly language is not possible, but we could use a common syntax that can be applied and adapted as needed.
This new syntax would make an assembly program easier to understand and the whole language easier to learn.
Most programmers already know a high level language before they learn assembly, so making assembly look more like a high level language would improve the learning curve for newcomers.
For me, the definition of a assembly instruction is "one that can be converted in one machine code" and nothing else.
So instead of:

MOV R1, R0

We should have:

R0 = R1

or 

R0 <- R1

or even:

R1 -> R0.

The first form is the preferred one because is how most high level languages assign a value to a variable. 
All these forms are more intuitive than "mov R1, R0". 
First, because it doesn't make it clear which register is the source and which one is the destiny of the operation.
Secondly, even if not that important, because it is wrong, since we are not moving the value of R0 to R1, we are copying it.
It might sound silly, but if we were moving the value then the value in R0 after the instruction should be undefined.

## Modern ARM Assembly

Here is how I envision an assembly language for the ARM should be.
This is not a complete instruction set, but it gives a general idea of what an assembly language should look like, in my opinion.

ADD:

r0 = r1 + r2
r0 = r1 + Imm // Immediate value

ADC:

r0 = r1 + r2 + Carry // or C for brevity
r0 = r1 +c r2 // shorter version
r0 = r1 + Imm + c 
r0 = r1 +c Imm

* Don't set the flags:

r0 = r1 +c r2 (no flags)

MOV:

r1 = r2

r1 = Imm

NEG:

r1 = -r2

MOVK:

r[0..15] = Imm16

r[16..31] = Imm16

r[32..63] = Imm16

MOVZ:

r = Imm16 left shift N
r = Imm16 << N

STR:

Mem[r1+r2] = r3
mem[r1+Imm] = r2
r1 -> mem[r2+r3] 
r1 -> m[r2+Imm]

STRH:

M[r1+r2] = low(r3)
m[r1+Imm] = r2[15..0]

STRB:

m[r1+r2] = byte(r3) | r3[7..0]

LDR:

r0 = m[r1+r2]
r0 <- mem[r1+r2]
m[r1+r2] -> r0

B , BAL:

goto label

Bcondition:

if condition goto label
goto label if condition

Examples:

goto end if eq
if ne goto loop

ORR, AND, EOR, BIC, ORN, EON

r1 = r2 or r3
r1 = r2 and r3
r1 = r2 eor r3
r1 = r2 and not r3
r1 = r2 or not r3

MVN

r1 = not r2

LSR, LSL, ASR, ROR:

r1 = r2 lsr r3 
r1 = r2 lsl r3
r1 = r2 asr r3
r1 = r2 ror r3

ADR:

r1 = label
r1 = addr(label)
r1 = .

ADRP:

r1 = aligned(label)

## Pre and post increment

Instead of STR r0, [r1, 4]!, it should be:

m[r1=r1+Imm] = r0

Instead of STR r0, [r1], Imm:

m[r1] = r0, r1 += Imm
m[r1++Imm] = r0

## Conditional instructions:

instruction if condition 
if condition instruction

Examples:

r0 = r0 + r1 if eq
if eq r0 = r0 + r1

## Bit extraction

r1 = (r2 lsr Imm1) and (Imm2 x 1)

## Bit insertion

r1[Imm1+Imm2..Imm1] = r2[Imm2..0]

r1[Imm1+Imm2..Imm1] = 0 

r1[Imm1+Imm2..Imm1, Imm1..0] = r2[Imm2..0], 0

## Operator (add, and,...) with LSL

r1 = r2 and (r2 lsl Imm)

## CSEL, CINC, CSINC

r1 = r2 if cond else r3
r1 = if cond r2 else r3

if cond r1 = r1+1
r1 += 1 if cond

r1 = r2 if cond else r3+1
r1 = if cond r2 else r3+1
if cond r1=r2 else r1=r3+1





## Conclusion

It shouldn't be hard for existing assemblers to incorporate the new syntax.
This new language can coexist with the traditional one, since each line of code represents one instruction.
All other features of an assembler can be used with the new syntax as well.


 



 



