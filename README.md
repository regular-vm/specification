# The RISC Educational Generalized UtiLity ARchitecture

The RISC Educational Generalized UtiLity ARchitecture (REGULAR) is a capable, general-purpose machine with a minimalist design that is easy to work with while also being well suited for a variety of computational tasks. The little-endian, 32-bit architecture boasts 32 scalar registers, of which 31 (all but the program counter) are available for general purpose use. The instruction set is cleanly designed to simplify decoding, and it is kept deliberately minimal to facilitate different programming styles, reduce implementation complexity, and allow for expansion in the future.

## Registers

REGULAR exposes 32, 32-bit registers to the programmer, named sequentially from `r0` to `r31`, all of which are identical and may be used interchangeably and as the argument to any instruction where appropriate. One special case is that the processor reads from `r0` to determine control flow; many assemblers support a `pc` alias (for "program counter", naturally) for this register. This exception has no effects on its use: it may be read from and operated on just like any other register. At the beginning of execution of each instruction, the processor will ensure that it refers to the address of the instruction immediately after the current one.

### Special note for stack-based programming

REGULAR has no special register to indicate the current position of the stack pointer ("the top of the stack", in procedural languages that implement control flow in this way). When implementing code that is expected to function in this manner is recommended that one of the general purpose registers is reserved to serve this purpose. Frequently this is `r31`, and many assemblers alias it to `sp` for this use.

### Suggested calling convention

To simplify function calls for control flow using a execution stack, it is recommended to implement the interprocedural application binary interface (ABI) as follows:

* Arguments are passed on the stack, in order, followed by any required context. A function's return value, if any, is placed on the stack after this and this structure is torn down by the the caller.
* All registers are callee-saved. Any registers used by the callee should first be saved to the stack so they can be restored prior to return from the procedure.


## Instructions

Each REGULAR instruction is 32 bits wide. The first byte encodes the opcode, while the remaining three bytes encode register information or immediate values. For the purposes of instruction encoding, op is the numerical value of the opcode identifying the instruction, rA, rB, rC, … are registers (not necessarily distinct) with the letters standing in for the register's number, and imm is an immediate constant embedded in the instruction.

### Instruction types

The following are possible encodings for the types of instructions that REGULAR supports:

#### op
<table>
	<tr>
		<th>Bit</th>
		<td>0</td>
		<td>1</td>
		<td>2</td>
		<td>3</td>
		<td>4</td>
		<td>5</td>
		<td>6</td>
		<td>7</td>
		<td>8</td>
		<td>9</td>
		<td>10</td>
		<td>11</td>
		<td>12</td>
		<td>13</td>
		<td>14</td>
		<td>15</td>
		<td>16</td>
		<td>17</td>
		<td>18</td>
		<td>19</td>
		<td>20</td>
		<td>21</td>
		<td>22</td>
		<td>23</td>
		<td>24</td>
		<td>25</td>
		<td>26</td>
		<td>27</td>
		<td>28</td>
		<td>29</td>
		<td>30</td>
		<td>31</td>
	</tr>
	<tr>
		<th>Use</th>
		<td colspan="8">op</td>
		<td colspan="24"><i>ignored</i></td>
	</tr>
</table>

#### op rA
<table>
	<tr>
		<th>Bit</th>
		<td>0</td>
		<td>1</td>
		<td>2</td>
		<td>3</td>
		<td>4</td>
		<td>5</td>
		<td>6</td>
		<td>7</td>
		<td>8</td>
		<td>9</td>
		<td>10</td>
		<td>11</td>
		<td>12</td>
		<td>13</td>
		<td>14</td>
		<td>15</td>
		<td>16</td>
		<td>17</td>
		<td>18</td>
		<td>19</td>
		<td>20</td>
		<td>21</td>
		<td>22</td>
		<td>23</td>
		<td>24</td>
		<td>25</td>
		<td>26</td>
		<td>27</td>
		<td>28</td>
		<td>29</td>
		<td>30</td>
		<td>31</td>
	</tr>
	<tr>
		<th>Use</th>
		<td colspan="8">op</td>
		<td colspan="8">A</td>
		<td colspan="16"><i>ignored</i></td>
	</tr>
</table>

#### op rA imm
<table>
	<tr>
		<th>Bit</th>
		<td>0</td>
		<td>1</td>
		<td>2</td>
		<td>3</td>
		<td>4</td>
		<td>5</td>
		<td>6</td>
		<td>7</td>
		<td>8</td>
		<td>9</td>
		<td>10</td>
		<td>11</td>
		<td>12</td>
		<td>13</td>
		<td>14</td>
		<td>15</td>
		<td>16</td>
		<td>17</td>
		<td>18</td>
		<td>19</td>
		<td>20</td>
		<td>21</td>
		<td>22</td>
		<td>23</td>
		<td>24</td>
		<td>25</td>
		<td>26</td>
		<td>27</td>
		<td>28</td>
		<td>29</td>
		<td>30</td>
		<td>31</td>
	</tr>
	<tr>
		<th>Use</th>
		<td colspan="8">op</td>
		<td colspan="8">A</td>
		<td colspan="16">imm</td>
	</tr>
</table>

#### op rA rB
<table>
	<tr>
		<th>Bit</th>
		<td>0</td>
		<td>1</td>
		<td>2</td>
		<td>3</td>
		<td>4</td>
		<td>5</td>
		<td>6</td>
		<td>7</td>
		<td>8</td>
		<td>9</td>
		<td>10</td>
		<td>11</td>
		<td>12</td>
		<td>13</td>
		<td>14</td>
		<td>15</td>
		<td>16</td>
		<td>17</td>
		<td>18</td>
		<td>19</td>
		<td>20</td>
		<td>21</td>
		<td>22</td>
		<td>23</td>
		<td>24</td>
		<td>25</td>
		<td>26</td>
		<td>27</td>
		<td>28</td>
		<td>29</td>
		<td>30</td>
		<td>31</td>
	</tr>
	<tr>
		<th>Use</th>
		<td colspan="8">op</td>
		<td colspan="8">A</td>
		<td colspan="8">B</td>
		<td colspan="8"><i>ignored</i></td>
	</tr>
</table>

#### op rA rB imm
<table>
	<tr>
		<th>Bit</th>
		<td>0</td>
		<td>1</td>
		<td>2</td>
		<td>3</td>
		<td>4</td>
		<td>5</td>
		<td>6</td>
		<td>7</td>
		<td>8</td>
		<td>9</td>
		<td>10</td>
		<td>11</td>
		<td>12</td>
		<td>13</td>
		<td>14</td>
		<td>15</td>
		<td>16</td>
		<td>17</td>
		<td>18</td>
		<td>19</td>
		<td>20</td>
		<td>21</td>
		<td>22</td>
		<td>23</td>
		<td>24</td>
		<td>25</td>
		<td>26</td>
		<td>27</td>
		<td>28</td>
		<td>29</td>
		<td>30</td>
		<td>31</td>
	</tr>
	<tr>
		<th>Use</th>
		<td colspan="8">op</td>
		<td colspan="8">A</td>
		<td colspan="8">B</td>
		<td colspan="8">imm</td>
	</tr>
</table>

#### op rA rB rC
<table>
	<tr>
		<th>Bit</th>
		<td>0</td>
		<td>1</td>
		<td>2</td>
		<td>3</td>
		<td>4</td>
		<td>5</td>
		<td>6</td>
		<td>7</td>
		<td>8</td>
		<td>9</td>
		<td>10</td>
		<td>11</td>
		<td>12</td>
		<td>13</td>
		<td>14</td>
		<td>15</td>
		<td>16</td>
		<td>17</td>
		<td>18</td>
		<td>19</td>
		<td>20</td>
		<td>21</td>
		<td>22</td>
		<td>23</td>
		<td>24</td>
		<td>25</td>
		<td>26</td>
		<td>27</td>
		<td>28</td>
		<td>29</td>
		<td>30</td>
		<td>31</td>
	</tr>
	<tr>
		<th>Use</th>
		<td colspan="8">op</td>
		<td colspan="8">A</td>
		<td colspan="8">B</td>
		<td colspan="8">C</td>
	</tr>
</table>

The bits of each component of these instructions are laid out so that the lower bits of their numerical value corresponds to lower bit numbers for the instruction as a whole.

### Instruction set


| Name  | Encoding                      | Description |
|-------|-------------------------------|-------------|
| `nop` | 0x00                          | Perform no operation. |
| `add` | 0x01&nbsp;rA&nbsp;rB&nbsp;rC  | Perform an unsigned 32-bit addition of the values contained in rB and rC and store the result in rA. |
| `sub` | 0x02&nbsp;rA&nbsp;rB&nbsp;rC  | Perform an unsigned 32-bit subtraction of the value contained in rC from the value contained in rB and store the result in rA. |
| `and` | 0x03&nbsp;rA&nbsp;rB&nbsp;rC  | Perform a logical AND operation of the values contained in rB and rC and store the result of the operation in rA. |
| `orr` | 0x04&nbsp;rA&nbsp;rB&nbsp;rC  | Perform a logical OR operation of the values contained in rB and rC and store the result of the operation in rA. |
| `xor` | 0x05&nbsp;rA&nbsp;rB&nbsp;rC  | Perform a logical XOR operation of the values contained in rB and rC and store the result of the operation in rA. |
| `not` | 0x06&nbsp;rA&nbsp;rB          | Perform a logical NOT of the value contained in rB and store the result in rA. |
| `lsh` | 0x07&nbsp;rA&nbsp;rB&nbsp;rC  | Logically shift the value in rB by the number of bits represented by the signed quantity in rC. If this value is positive, shift the value contained in rB left by this many bits; if it is negative the shift will be to the right by the absolute value of the value in rC. In both instances newly vacated bits will be zeroed. If the value in rC is outside of the range (-32, 32) the result is undefined. |
| `ash` | 0x08&nbsp;rA&nbsp;rB&nbsp;rC  | Arithmetically shift the value in rB by the number of bits represented by the signed quantity in rC. If this value is positive, shift the value contained in rB left by this many bits; if it is negative the shift will be to the right by the absolute value of the value in rC. Newly vacated bits will be zeroed in the former case and be a duplicate of the most significant bit in the latter. If the value in rC is outside of the range (-32, 32) the result is undefined. |
| `tcu` | 0x09&nbsp;rA&nbsp;rB&nbsp;rC  | Subtract the unsigned value stored in rC from the unsigned value stored in rB with arbitrary precision and store the sign of the result in rA. |
| `tcs` | 0x0a&nbsp;rA&nbsp;rB&nbsp;rC  | Subtract the signed value stored in rC from the signed value stored in rB with arbitrary precision and store the sign of the result in rA. |
| `set` | 0x0b&nbsp;rA&nbsp;imm         | Store, with sign extension, the 16-bit signed value imm into rA. |
| `mov` | 0x0c&nbsp;rA&nbsp;rB          | Copy the value from rB into rA. |
| `ldw` | 0x0d&nbsp;rA&nbsp;rB          | Read a 32-bit word from the memory address referred to by rB and store the value into rA. |
| `stw` | 0x0e&nbsp;rA&nbsp;rB          | Store the value in rB as a 32-bit value at the memory address referred to by rA. |
| `ldb` | 0x0f&nbsp;rA&nbsp;rB          | Read an 8-bit unsigned byte from the memory address referred to by rB and store the value into rA. The upper 24 bits of rA are unaffected. |
| `stb` | 0x10&nbsp;rA&nbsp;rB          | Store the lower 8 bits of the value in rB as a byte at the memory address referred to by rA. |


