## Introduction ##

The Deus is virtual machine provides the execution environment for programs running under the Meta(Deus) operating system. The virtual machine models a CISC-like, three-operand, superscalar, `file-to-file` architecture.  Code can either be interpreted or compiled on-the-fly into machine code for the target architecture.

For code of the Deus has been created another project [Deus-Ex](https://code.google.com/p/deus-ex/)

## Memory Organization ##

The [Deus](Deus.md) virtualis mahcina does not provide any possibility or any interface for work with directly random access memory. All operands are files. All types are types of files, not a memory.


## Special Files ##

For special functions [Deus](Deus.md) has special files. These files execute functions of special registers of the processor of normal architecture. But, as well as all in the [Deus](Deus.md) universe, they are provided as normal files on namespace.

Deus is omnipresent. Each Bot which requires execution, got the Deus. Deus is in each Robot.

Any bot, is more difficult than basic type, is provided in the form of a file tree on namespace. special files there too.

#### Instruction Pointer ####

It is located **`deus/ex`**

It is specifies the current instruction for execution. It works in the same way, as everywhere, but contains not a memory address, but a file descriptor.

#### Status Flags ####

It is located **`deus/flags`**

The **`flags`** is intended for interrupt disabling and other special cases of control of the [Deus](Deus.md) machine.

#### Current FSM State ####

It is current state of executing bot. It is located **`deus/state`**

#### Queue of events ####

It is located **`deus/queue`**

Queue of events

#### Secret Key ####

It is located **`deus/key`**

Deus works only with the ciphered data. For implementation of this concept the virtual machine contains the **`key`**. Any data before processing are decrypted through **`key`**, and the result are ciphered by it. Process occurs is completely transparent without involvement of the user instructions or programs. The encryption algorithm can be selected in case of machine recompilation. AES is by default used.

For special cases for quick, compact and real-time devices the **`key`** can be disabled. Safety in this case completely in responsibility of the user.

#### Public Key ####

It is located **`deus/pub`**

Public key is used for establishment of encrypted link of interprocessor communication. Generation of secret key of the session made through Deffi-Hellman's protocol.

## Data Types ##

A **`byte`** is 8-bits, _integer_, unsigned. A **`int`** is 16-bits, _integer_, signed. A **`word`** is 32-bits, _integer_, signed. A **`great`** is 64-bits, _integer_, signed. A **`real`** is 32-bits IEEE format floating-point numbers. A **`double`** is 64-bits floating-point.

## Operand Size ##

The operand size of each instruction is encoded explicitly by the operand code. The operand size and type are specified by the last character of the instruction mnemonic:
```
	b - byte, unsigned 8-bits integer
	i - int, signed 16-bits integer
	w - word, signed 32-bits integer
	g - great, signed 64-bits integer
	r - real, 32-bits floating-point
	d - double, 64-bits floating-point
	c - unicode string into UTF-8
	s - set of bytes
```

## Effective Addresses ##

Each instruction can potentially address three operands. All operands are file or constant. If the middle operand of a three address instruction is omitted, it is assumed to be the same as the destination operand.

All operands generate an effective address from three basic modes: immediate, indirect and double indirect. The assembler syntax for each mode is:
```
	127b			constant unsigned 8-bits
	3.14159275r		constant signed 32-bits floating-point
	/path/to/file		immediate value from a file
```

## Garbage Collection ##

The garbage collection performs through reference-counted for naturally on the namespace. Mark-and-sweep heap can be created in file by means of the user library.

## Instruction Set ##

The Deus instruction set provides a close match to the architecture of existing processors. Instructions are of the form:
```
	OPERATOR    src1, src2, dst
```
If the middle operand is omitted, it is to be the same as the destination.
```
	OPERATOR    src, dst
```

### Access statements ###

> #### Attach ####
> > To establish a connection


> #### Clone ####
> > Duplicate file ID


> #### Walk ####
> > Advance FID one level of name hierarchy


> #### Open ####
> > Check of a validity of a key for file I/O


> #### Create ####
> > Create new file
```
		newX
Syntax:
		newb	src1, src2, dst
		newi	src1, src2, dst
		neww	src1, src2, dst
		newg	src1, src2, dst
		newr	src1, src2, dst
		newd	src1, src2, dst
		newc	src1, src2, dst
		news	src1, src2, dst

Function: creat(src1->path); src2->key, dst->key
```
> > > The `new`_X_ instruction allocates and initializes storage to a new file. The path are specified by src1 operand. Secret key are specified by src2 operand. The key for access is generated automatically and is located in dst.


> #### Read ####
> > Read contents of file

> #### Write ####
> > Write contents of file


> #### Close ####
> > Discard FID


> #### Remove ####
> > Remove file
```
		rmvX
Syntax:
		rmvb	src, dst
		rmvi	src, dst
		rmvw	src, dst
		rmvg	src, dst
		rmvr	src, dst
		rmvd	src, dst
		rmvc	src1, src2, dst
		rmvs	src1, src2, dst

Function: unlink(dst->path);
```
> > > `rmv`_X_ instruction remove a file. The path are specified by dst operand.


> #### Stat ####
> > Report file stat


> #### Wstat ####
> > Modify file stat


> #### Error ####
> > Return error condition for failed operation


> #### Flush ####
> > Disregard outstanding request



### Assignment  statements ###

> #### Logical AND ####
```
             andX
Syntax:
             andb	src1, src2, dst
             andi	src1, src2, dst
             andw	src1, src2, dst
             andg	src1, src2, dst

Function: dst = src1 & src2
```
> The instructions compute the bitwise AND of the two file addressed by src1 and src2 and stores the result in the dst file.

> #### Logical OR ####
```
            orx
Syntax:
             orb	src1, src2, dst
             ori	src1, src2, dst
             orw	src1, src2, dst
             org	src1, src2, dst

Function:	dst = src1 | src
```
> These instructions compute the bitwise OR of the two operands addressed by src1 and src2 and store the result in the dst operand.

> #### Exclusive OR ####
```
            xorx 
Syntax:		
            xorb	src1, src2, dst
            xori	src1, src2, dst
            xorw	src1, src2, dst
            xorg	src1, src2, dst

Function: dst = src1 ^ src2
```
> These instructions compute the bitwise exclusive-OR of the two files addressed by src1 and src2 and store the result in the dst file.

> #### Shift left arithmetic ####
```
            shlx
Syntax:
            shlb	src1, src2, dst
            shlw	src1, src2, dst
            shli	src1, src2, dst
            shlg	src1, src2, dst

Function:  dst = src2 << src1
```
> The **shl** instructions shift the src2 operand left by the number of bits specified by the src1 operand and store the result in the dst operand. Shift counts less than 0 or greater than the number of bits in the object have undefined results.

> #### Shift right arithmetic ####
```
            shrx
Syntax:
            shrb	src1, src2, dst
            shri	src1, src2, dst
            shrw	src1, src2, dst
            shrg	src1, src2, dst

Function:	dst = src2 >> src1
```
> The **shr** instructions shift the src2 operand right by the number of bits specified by the src1 operand and store the result in the dst operand. Shift counts less than 0 or greater than the number of bits in the object have undefined results.

> #### Addition ####
```
             addx
Syntax:
             addb	src1, src2, dst
             addi	src1, src2, dst
             addw	src1, src2, dst
             addg	src1, src2, dst
             addr	src1, src2, dst
             addd	src1, src2, dst
             addc	src1, src2, dst

Function: dst = src1 + src2
```
The instructions compute the sum of the operands addressed by src1 and src2 and stores the result in the dst file. For addb the result is truncated to eight bits.

The **addc** instruction concatenates the two UTF-8 strings.

> #### Subtract ####
```
            subx
Syntax:
            subb	src1, src2, dst
            subi	src1, src2, dst
            subw	src1, src2, dst
            subg	src1, src2, dst
            subr	src1, src2, dst
            subd	src1, src2, dst

Function:  dst = src2 - src1
```
The **sub** instructions subtract the operands addressed by src1 and src2 and stores the result in the dst operand. For subb, the result is truncated to eight bits.

> #### Multiply ####
```
             mulx
Syntax:
             mulb	src1, src2, dst
             muli	src1, src2, dst
             mulw	src1, src2, dst
             mulg	src1, src2, dst
             mulr	src1, src2, dst
             muld	src1, src2, dst

Function:  dst = src1 * src2
```
> The src1 operand is multiplied by the src2 operand and the product is stored in the dst operand.

> #### Divide ####
```
		divx
Syntax:
		divb	src1, src2, dst
		divi	src1, src2, dst
		divw	src1, src2, dst
		divg	src1, src2, dst
		divr	src1, src2, dst
		divd	src1, src2, dst

Function:  dst = src2 / src1
```
> The src2 operand is divided by the src1 operand and the quotient is stored in the dst operand. Division by zero causes the thread to terminate.

> #### Modulus ####
```
		modx
Syntax:
		modb	src1, src2, dst
		modi	src1, src2, dst
		modw	src1, src2, dst
		modg	src1, src2, dst

Function: dst = src2 % src1
```
> The **modulus** instructions compute the remainder of the src2 operand divided by the src1 operand and store the result in dst. The operator preserves the condition that the absolute value of a%b is less than the absolute value of b; (a/b)b + a%b is always equal to a.

> #### Move ####
```
		movx
Syntax:
		movb	src, dst
		movi	src, dst
		movw	src, dst
		movg	src, dst
		movr	src, dst
		movd	src, dst
		movc	src, dst
		movs	src1, src2, dst

Function:	dst = src, src2->size
```
> The **move** operators perform assignment. The value specified by the src operand is copied to the dst operand.
> The movs instruction copies data from the src1 file to the dst file for src2 bytes.

> #### Rub (clear) ####
```
		rubx
Syntax:
		rubb	src, dst
		rubi	src, dst
		rubw	src, dst
		rubg	src, dst
		rubr	src, dst
		rubd	src, dst
		rubc	src1, src2, dst
		rubs	src1, src2, dst

Function: memset(&dst, 0, src->size);
```
> > The **rub**_x_ instruction all values are set to zero. The path are specified by dst operand. The size of set of bytes are specified by src2 operand.