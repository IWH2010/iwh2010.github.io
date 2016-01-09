---
layout: post
title: Deep information
---

Before we get started there is some information that needs to be passed. When we work in IDA or Olly, what we see is the so called machine code being translated by the 
debugger into assembly. We can't see actual programing code. Don't be afraid of the following, it might look like it's tough to understand but as we work these things will make more sense.
Just try to understand as much as possible. :relieved:

BIT - The smallest possible piece of data. It can be either a 0 or a 1. If you put a bunch of bits together, you end up in the 'binary number system'

 - 00000001 = 1 
 - 00000010 = 2
 - 00000011 = 3
 - etc. 

BYTE - A byte consists of 8 bits. It can have a maximal value of 255 (0-255). To make it easier to read binary numbers, we use the 'hexadecimal number system'. It's a 'base-16 system', while binary is a 'base-2 system' 
 
WORD - A word is just 2 bytes put together or 16 bits. A word can have a maximal value of 0FFFFh (or 65535d). 

DOUBLE WORD - A double word is 2 words together or 32 bits. Max value = 0FFFFFFFF (or 4294967295d). 

KILOBYTE - There are 1024 (32*32) bytes. Not 1000 as the name suggests. 

MEGABYTE - 1024*1024 or 1,048,578 bytes. Not 1 million bytes.

When debugging there are 9 registers we need to know about. These are "special places" in the computers memory where we can store data.

 - EAX - Extended Accumulator Register
 - EBX - Extended Base Register
 - ECX - Extended Counter Register
 - EDX - Extended Data Register
 - ESI - Extended Source Index
 - EDI - Extended destinationination Index
 - EBP - Extended Base Pointer
 - ESP - Extended Stack Pointer
 - EIP - Extended Instruction Pointer
 
Flags are single bits which indicate the status of something. The ones that are important to us are:
 
 - The Z-Flag
	
	 - Zero flag is the most useful flag for cracking. It is used in about 90% of all cases
	 
 - The O-Flag
 
	 - Overflow flag is used in about 4% of all cracking attempts
	 
 - The C-Flag
 
	 - Carry flag is used in about 1% of all cracking attempts

	 
When reversing we need to know these flags to understand if a jump is executed or not.
A flag can only be '0' or '1', meaning 'not set' or 'set'.

**The stack:**

The Stack is a part in memory where you can store different things for later use.
See it as a pile of books in a chest where the last put in is the first to grab out.

Try to remember the following rule: the last sheet of paper you put in the stack, is the first one you'll take out!

The command 'push' saves the contents of a register onto the stack.

The command 'pop' grabs the last saved contents of a register from the stack and puts it in a specific register.

Grab your seat because here comes the bang!

**Instructions**


Most instructions (mnemonics) have two operators (like "add EAX, EBX"), but some have one ("not EAX") or even three ("IMUL EAX, EDX, 64"). 
When you have an instruction that says something with "DWORD PTR [XXX]" then the DWORD (4 byte) value at memory offset [XXX] is meant.
Note that the bytes are saved in reverse order in the memory.

Most instructions with 2 operators can be used in the following ways (example: add):

 - add eax,ebx - Register, Register
 - add eax,123 - Register, Value
 - add eax,dword ptr [404000] - Register, Dword Pointer [value]
 - add eax,dword ptr [eax] - Register, Dword Pointer [register]
 - add eax,dword ptr [eax+00404000] - Register, Dword Pointer [register+value]
 - add dword ptr [404000],eax - Dword Pointer [value], Register
 - add dword ptr [404000],123 - Dword Pointer [value], Value
 - add dword ptr [eax],eax - Dword Pointer [register], Register
 - add dword ptr [eax],123 - Dword Pointer [register], Value
 - add dword ptr [eax+404000],eax - Dword Pointer [register+value], Register
 - add dword ptr [eax+404000],123 - Dword Pointer [register+value], value

**ADD (Addition)**

Syntax: ADD destination, source

The ADD instruction adds a value to a register or a memory address. It can be used in these ways:

 - These instruction *can* set the Z-Flag, the O-Flag and the C-Flag (and some others, which are not needed for cracking).

**AND (Logical And)**

Syntax: AND destination, source     

The AND instruction uses a logical AND on two values.

This instruction *will* clear the O-Flag and the C-Flag and can set the Z-Flag.

To understand AND better, consider those two binary values:

 - 1001010110
 - 0101001101
 
If you AND them, the result is 0001000100

When two 1 stand below each other, the result is of this bit is 1, if not: The result is 0.

**CALL (Call)**

Syntax: CALL something

The instruction CALL pushes the RVA (Relative Virtual Address) of the instruction that follows the CALL to the stack and calls a sub program/procedure.

CALL can be used in the following ways:

 - CALL	404000 - CALL Address (Most common)
 - CALL	EAX - CALL Register (If EAX would be 404000, it would be the same as above)
 - CALL	DWORD PTR [EAX] - Calls the adress that is stored at [EAX]
 - CALL	DWORD PTR [EAX+5] - Calls the adress that is stored at [EAX+5]

**CMP (Compare)**

Syntax: CMP destination, source

The CMP instruction compares two things and can set the C/O/Z flags if the result fits.

Examples:
 
 - CMP	EAX, EBX - Compares eax and ebx and sets z-flag if they are equal
 - CMP	EAX,[404000] - Compares eax with the dword at 404000
 - CMP	[404000],EAX - Compares eax with the dword at 404000

 
**DEC (Decrement)**

Syntax: DEC something

DEC is used to decrease a value (that is: value=value-1)

Examples:
 
 - DEC EAX - Decrease EAX
 - DEC [EAX] - Decrease the dword that is stored at [EAX]
 - DEC [401000] - Decrease the dword that is stored at [401000]
 - DEC [EAX+401000] - Decrease the dword that is stored at [EAX+401000]

The DEC instruction *can* set the Z/O flags if the result fits.
    
**DIV (Division)**

Syntax: DIV divisor

DIV is used to divide EAX through divisor (unsigned division). The dividend is always EAX, the result is stored in EAX, the modulo-value in EDX.

Examples:

 - MOV EAX,64 - EAX = 64h = 100
 - MOV ECX,9 - ECX = 9
 - DIV ECX - Divide EAX through ECX

The DIV instruction *can* set the C/O/Z flags if the result fits.



**IDIV (Integer Division)**

Syntax: IDIV divisor

The IDIV works in the same way as DIV, but IDIV is a signed division.

The idiv instruction *can* set the C/O/Z flags if the result fits.

**IMUL (Integer Multiplication)**

Syntax:
 
 - IMUL value 
 - IMUL destination,value,value
 - MUL destination,value

IMUL multiplies either EAX with value (IMUL value) or it multiplies two values and puts them into a destination register (IMUL destination, value, value)
or it multiplies a registerwith a value (IMUL destination, value).

If the multiplication result is too big to fit into the destination register, the O/C flags are set. The Z flag can be set, too.

**INC (Increment)**

Syntax: INC register

INC is the opposite of the DEC instruction; it increases values by 1. INC *can* set the Z/O flags.

**INT**

Syntax: INT destination 

Generates a call to an interrupt handler. The destination value must be an integer (e.g., INT 21h).
INT3 and INTO are interrupt calls that take no parameters but call the handlers for interrupts 3 and 4, respectively.


**MOV (Move)**

Syntax: MOV destination, source

This is an easy to understand instruction. MOV copies the value from source to destination and source stays what it was before.

There are some variants of MOV:
 
 - MOVS/MOVSB/MOVSW/MOVSD EDI, ESI: Those variants copy the byte/word/dword ESI points to, to the space EDI points to.
 - MOVSX: 	MOVSX expands Byte or Word operands to Word or Dword size and keeps the sign of the value.
 - MOVZX:	MOVZX expands Byte or Word operands to Word or Dword size and fills the rest of the space with 0.

**MUL (Multiplication)**

Syntax: MUL value 

This instruction is the same as IMUL, except that it multiplies unsigned. It *can* set the O/Z/F flags.

**NOP (No Operation)**

Syntax: NOP

This instruction does absolutely nothing.

**OR (Logical Inclusive Or)**

Syntax: OR destination,source

The OR instruction connects two values using the logical inclusive or. This instruction *clears* the O-Flag and the C-Flag and *can* set the Z-Flag.

To understand OR better, consider those two binary values:

- 1001010110
- 0101001101
     
If you OR them, the result is 1101011111.

Only when there are two 0 on top of each other, the resulting bit is 0. Else the resulting bit is 1.

**POP**

Syntax: POP destination

POP loads the value of byte/word/dword ptr [esp] and puts it into destination. Additionally it increases the stack by the size of the value that was popped of the stack, so that the next
POP would get the next value.


**PUSH**

Syntax: PUSH operand

PUSH is the opposite of POP. It stores a value on the stack and decreases it by the size of the operand that was pushed, so that ESP points to the value that was pushed.


**REP/REPE/REPZ/REPNE/REPNZ**

Syntax: REP/REPE/REPZ/REPNE/REPNZ ins 

Repeat Following String Instruction: Repeats ins until CX=0 or until indicated condition 

 - (ZF=1, ZF=1, ZF=0, ZF=0) is met. The ins value must be a string operation such as CMPS, INS, LODS, MOVS, OUTS, SCAS, or STOS.


**RET (Return)**

Syntax: 
	- RET
    - RET digit

RET does nothing but return from a part of code that was reached using a CALL instruction. RET digit *cleans* the stack before it returns.

**SUB (Subtraction)**

Syntax: SUB destination,source

SUB is the opposite of the ADD command. It subtracts the value of source from the value of destination and stores the result in destination.

SUB *can* set the Z/O/C flags.

**TEST**

Syntax: TEST operand1, operand2

This instruction is in 99% of all cases used for "TEST EAX, EAX". It performs a Logical AND(AND instruction) but does not save the values.
It *only* sets the Z-Flag, when EAX is 0 or clears it, when EAX is not 0. The O/C flags are *always* cleared.


**XOR**

Syntax: XOR destination,source

The XOR instruction connects two values using logical exclusive OR (remember OR uses inclusive OR). 

This instruction clears the O-Flag and the C-Flag and can set the Z-Flag.

To understand XOR better, consider those two binary values:

 - 1001010110
 - 0101001101

If you OR them, the result is 1100011011.

When two bits on top of each other are equal, the resulting bit is 0. Else the resulting bit is 1.
The most often seen use of XOR is “XOR, EAX, EAX”. This will set EAX to 0, because when you XOR a value with itself, the result is *always* 0.

**JUMPS**

These are the most important jumps and the condition that needs to be met, so that they'll be executed (Important jumps are marked with * and very important with **):


 - JA* - Jump if (unsigned) above - CF=0 and ZF=0
 - JAE	-	Jump if (unsigned) above or equal		- CF=0
 - JB*	-	Jump if (unsigned) below			- CF=1
 - JBE	-	Jump if (unsigned) below or equal		- CF=1 or ZF=1
 - JC	-	Jump if carry flag set			- CF=1
 - JCXZ	-	Jump if CX is 0				- CX=0
 - JE**	-	Jump if equal					- ZF=1
 - JECXZ	-	Jump if ECX is 0				- ECX=0
 - JG*	-	Jump if (signed) greater			- ZF=0 and SF=OF (SF = Sign Flag)
 - JGE*	-	Jump if (signed) greater or equal		- SF=OF
 - JL*	-	Jump if (signed) less				- SF != OF (!= is not)
 - JLE*	-	Jump if (signed) less or equal		- ZF=1 and OF != OF
 - JMP**	-	Jump						- Jumps always
 - JNA	-	Jump if (unsigned) not above		- CF=1 or ZF=1
 - JNAE	-	Jump if (unsigned) not above or equal	- CF=1
 - JNB	-	Jump if (unsigned) not below		- CF=0
 - JNBE 	-	Jump if (unsigned) not below or equal	- CF=0 and ZF=0
 - JNC	-	Jump if carry flag not set			- CF=0
 - JNE**	-	Jump if not equal				- ZF=0
 - JNG	-	Jump if (signed) not greater			- ZF=1 or SF!=OF
 - JNGE	-	Jump if (signed) not greater or equal	- SF!=OF
 - JNL	-	Jump if (signed) not less			- SF=OF
 - JNLE	-	Jump if (signed) not less or equal		- ZF=0 and SF=OF
 - JNO	-	Jump if overflow flag not set		- OF=0
 - JNP	-	Jump if parity flag not set			- PF=0
 - JNS	-	Jump if sign flag not set			- SF=0
 - JNZ	-	Jump if not zero				- ZF=0
 - JO	-	Jump if overflow flag is set			- OF=1
 - JP	-	Jump if parity flag set			- PF=1
 - JPE	-	Jump if parity is equal			- PF=1
 - JPO	-	Jump if parity is odd				- PF=0
 - JS	-	Jump if sign flag is set			- SF=1
 - JZ	-	Jump if zero					- ZF=1

























