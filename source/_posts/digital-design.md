---
title: Digital Design and Computer Architecture by David Harris
date: 2022-07-08 15:42:00
tags:
- computer science
categories:
- other study
---
#### binary number system

* unsigned
* sign/magnitude
* two's complement : inverting all of the bits in the number, then adding 1 to the least significant bit position. 

------

<!-- more -->

#### gray code

三位二进制码转换为三位格雷码：

$G_2 = B_2$

$G_1 = B_2 \bigoplus B_1$

$G_0 = B_1 \bigoplus B_0$

三位格雷码转换为三位二进制码：

$B_2 = G_2$

$B_1 =  B_2 \bigoplus G_1$

$B_0 = B_1 \bigoplus G_0$

---

#### 逻辑代数常用公式

* 消去律：$AB+A\bar{B}=A$
* 吸收律1：$A+AB=A$
* 吸收律2：$A+\bar{A}B=A+B$
* 包含律：$AB+\bar{A}C+BC=AB+\bar{A}C$
* 对偶数个变量而言，异或是同或的反函数
* 对奇数个变量而言，异或与同或相同
* $A(B\oplus C)=A\oplus B+A\oplus C$
* $A+(B\odot C)=(A+B)\odot (A+C)$



#### 公式法化简

* 并项法：利用公式$AB+A\bar{B}=A$
* 吸收法：利用公式$A+AB=A$
* 消因子法：利用公式$A+\bar{A}B=A+B$
* 消项法：利用公式$AB+\bar{A}C+BC=AB+\bar{A}C$
* 配项法：$\bar{A}+A=1，A\cdot 1=A$



#### 化简为其他形式

与非与非式：

1. 把F化成最简与或式
2. 将最简与或式两次求反

与或非式：

1. 求出反函数$\bar{F}的$最简与或式
2. 对$\bar{F}$求反

或与式：

1. 对最简与或非式进行两次反演律

或非或非式

1. 对最简与或非式的两个与项分别用反演律变为或非式

-------

#### logic gates

* not gate(inverter) : output is the inverse of the input
* buffer(bubble) : output equals its input
* and gate : if two inputs are both true, the gate produces true. Otherwise, the output is false.
* or gate : the gate produces true, if either a or b (or both) are true.

Suppose the lowest voltage in the system is 0 V, also called ground or GND. The highest voltage in the system comes from the power supply and is usually called VDD. 

-------

#### nMOS and pMOS

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/transmission%20gate.jpg" alt="image-20201114142502687" style="zoom:67%;" />

A MOSFET behaves as a voltage-controlled switch in which the gate voltage creates an electric field that turns ON or OFF a connection between the source and drain.

nMOS transistors are OFF when the gate is 0 and ON when the gate is 1.pMOS transistors are just the opposite: ON when the gate is 0 and OFF when the gate is 1.

------

#### transmission gate (pass gate)

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/transmission%20gate.jpg" alt="image-20201115001331300" style="zoom:67%;" />

​	The two sides of the switch are called A and B because a switch is bidirectional and has no preferred input or output side. The control signals are called enables, $EN$ and $\bar{EN}$  1, when EN = 0 and another = 1, both transistors are OFF. Hence, the transmission gate is OFF or disabled, so A and B are not connected. When EN = 1 and another = 0, the transmission gate is ON or enabled, and any logic value can flow between A and B.

-----

* Transistor in series  are slower than transistors in parallel
* pMOS transistor are slower than nMOS transistors



#### pseudo-nMOS

​	The physical dimensions of the pMOS transistor are selected so that the pMOS transistor will pull the output, Y, HIGH weaklyâ€”that is, only if none of the nMOS transistors are ON. But if any nMOS transistor is ON, it overpowers the weak pull-up and pulls Y down close enough to GND to produce a logic 0.

------

#### combinational circuits

A circuit is combinational if it consists of interconnected circuit elements such that :

* Every circuit element is itself combinational
* Every node of the circuit is either designated as an input to the circuit or connects to exactly one output terminal of a circuit element
* The circuit contains no cyclic paths: every path through the circuit visits each circuit node at most once

------

#### Boolean Equations

​	The order of operations is important when interpreting Boolean equations. In Boolean equations, NOT has the highest precedence, followed by AND, then OR. Just as in ordinary equations, products are performed before sums.

------

#### Bubble

​	The inversion circle is called a bubble. Intuitively, you can imagine that “pushing” a bubble through the gate causes it to come out at the other side and flips the body of the gate from AND to OR or vice versa. For example, the NAND gate in Figure 2.19 consists of an AND body with a bubble on the output. Pushing the bubble to the left results in an OR body with bubbles on the inputs. The underlying rules for bubble pushing are

* Pushing bubbles backward (from the output) or forward (from the inputs) changes the body of the gate from AND to OR or vice versa
* Pushing a bubble from the output back to the inputs puts bubbles on all gate inputs
* Pushing bubbles on all gate inputs forward toward the output puts a bubble on the output

------

#### Multiplexer

​	Multiplexers are among the most commonly used combinational circuits. They choose an output from among several possible inputs based on the value of a select signal. A multiplexer is sometimes affectionately called a mux.

​	Figure 2.54 shows the schematic and truth table for a 2:1 multiplexer with two data inputs, $D_0$ and $D_1$, a select input, S, and one output, Y. The multiplexer chooses between the two data inputs based on the select: if S = 0,  $Y=D_0$, and if S = 1, $Y = D_1$. S is also called a control signal because it controls what the multiplexer does.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/multiplexer.jpg" alt="image-20201213175918402" style="zoom:67%;" />

------

#### Decoder

​	A decoder has N inputs and $2^N$ outputs. It asserts exactly one of its outputs depending on the input combination.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/decoder.jpg" alt="image-20201122175036337" style="zoom:67%;" />

------

#### TIMING

​	The transition from LOW to HIGH is called the *rising edge*. Similarly, the transition from HIGH to LOW (not shown in the figure) is called the *falling edge*.

​	We measure delay from the 50% point of the input signal, A, to the 50% point of the output signal, Y. The 50% point is the point at which the signal is half-way (50%) between its LOW and HIGH values as it transitions.

​	Combinational logic is characterized by its ***propagation delay*** and ***contamination delay***. The propagation delay, tpd, is the maximum time from when an input changes until the output or outputs reach their final value. The contamination delay, tcd, is the minimum time from when an input changes until any output starts to change its value.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/delay.jpg" alt="image-20201122182314106" style="zoom: 67%;" />

#### Glitch

​	In general, a glitch can occur when a change in a single variable crosses the boundary between two prime implicants in a K-map. We can eliminate the glitch by adding redundant implicants to the K-map to cover these boundaries. This of course comes at the cost of extra hardware. However, simultaneous transitions on multiple variables can also cause glitches. These glitches cannot be fixed by adding hardware. Because the vast majority of interesting systems have simultaneous (or near-simultaneous) transitions on multiple variables, glitches are a fact of life in most circuits.

------

#### SR Latch

​	Like the cross-coupled inverters, the SR latch is a bistable element with one bit of state stored in Q. However, the state can be controlled through the S and R inputs. When R is asserted, the state is reset to 0. When S is asserted, the state is set to 1. When neither is asserted, the state retains its old value. 

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/SR%20Latch.jpg" alt="image-20201130180035119" style="zoom: 67%;" />

#### D Latch

​	The D latch in Figure 3.7(a) solves these problems. It has two inputs. The data input, D, controls what the next state should be. The clock input, CLK, controls when the state should change.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/D%20Latch.jpg" alt="image-20201130180133190" style="zoom: 67%;" />

#### D Flip-Flop

​	A D flip-flop can be built from two back-to-back D latches controlled by complementary clocks, as shown in Figure 3.8(a). The first latch, L1, is called the master. The second latch, L2, is called the slave. The node between them is named N1. A symbol for the D flip-flop is given in Figure 3.8(b). When the output is not needed, the symbol is often condensed as in Figure 3.8(c). 

​	When CLK = 0, the master latch is transparent and the slave is opaque. Therefore, whatever value was at D propagates through to N1. When CLK = 1, the master goes opaque and the slave becomes transparent. The value at N1 propagates through to Q, but N1 is cut off from D. Hence, whatever value was at D immediately before the clock rises from 0 to 1 gets copied to Q immediately after the clock rises. At all other times, Q retains its old value, because there is always an opaque latch blocking the path between D and Q

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/DFlip-flop.jpg" alt="image-20201130180214581" style="zoom: 67%;" />

------

#### Register

​	An N-bit register is a bank of N flip-flops that share a common CLK input, so that all bits of the register are updated at the same time.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/register.jpg" alt="image-20201209184417515" style="zoom:67%;" />

------

#### Enabled Flip-Flop

​	An enabled flip-flop adds another input called EN or ENABLE to determine whether data is loaded on the clock edge. When EN is
TRUE, the enabled flip-flop behaves like an ordinary D flip-flop. When EN is FALSE, the enabled flip-flop ignores the clock and retains its state. Enabled flip-flops are useful when we wish to load a new value into a flip-flop only some of the time, rather than on every clock edge.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/CLK.jpg" alt="image-20201130180248220" style="zoom:50%;" />

#### Resettable Flip-Flop

​	A resettable flip-flop adds another input called RESET. When RESET is FALSE, the resettable flip-flop behaves like an ordinary D flip-flop. When RESET is TRUE, the resettable flip-flop ignores D and resets the output to 0. Resettable flip-flops are useful when we want to force a known state (i.e., 0) into all the flip-flops in a system when we first turn it on.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/resettable.jpg" alt="image-20201130180332971" style="zoom:50%;" />

------

锁存器（latch）---对脉冲电平敏感，在时钟脉冲的电平作用下改变状态 

触发器（Flip-Flop，简写为 FF）---对脉冲边沿敏感，其状态只在时钟脉冲的上升沿或下降沿的瞬间改变 ；

------

#### The dynamic discipline

​	When the clock rises, the output (or outputs) may start to change after the clock-to-Q contamination delay, $t_{ccq}$, and must definitely settle to the final value within the clock-to-Q propagation delay, $t_{pcq}$. These represent the fastest and slowest delays through the circuit, respectively. For the circuit to sample its input correctly, the input (or inputs) must have stabilized at least some setup time, $t_{setup}$, before the rising edge of the clock and must remain stable for at least some hold time, $t_{hold}$, after the rising edge of the clock. 
$$
T_c\geq t_{pcq}+t_{pd}+t_{setup}
$$

$$
t_{ccq}+t_{cd}\geq t_{hold}
$$

$$
T_c\geq t_{pcq}+t_{pd}+t_{setup}+t_{skew}
$$

$$
t_{pd}\geq T_c+t_{pcq}+t_{setup}+t_{skew}
$$

$$
t_{ccq}\geq t_{cd} + t_{hold}+t_{skew}
$$

$$
t_{cd} \geq t_{hold}+t_{skew}-t_{ccq}
$$

------

#### Resolution time

​	Theoretical and experimental analyses (see Section 3.5.6) have shown that the probability that the resolution time, tres, exceeds some arbitrary time, t, decreases exponentially with t:
$$
P(t_{res}>t)=\frac{T_0}{T_c}e^{-\frac{t}{\tau}}
$$

------

#### Synchronizers

​	if D changes N times per second, the probability of failure per second is N times as great:
$$
P(failure)/sec=N\frac{T_0}{T_c}e^{-\frac{T_c-t_{setup}}{\tau}}
$$
​	System reliability is usually measured in mean time between failures(MTBF). As the name might suggest, MTBF is the average amount of time between failures of the system. It is the reciprocal of the probability that the system will fail in any given second
$$
MTBF=\frac{1}{P(failure)/sec}=\frac{T_ce^{\frac{T_c-t_{setup}}{\tau}}}{NT_0}
$$

------

#### Verilog

* All signals on the left hand side of $<=$ or$=$  in an *always* statement must be declared as *reg*.

```verilog
module mux2 (input [3:0] d0, d1, 
             input s, 
             output [3:0] y);
assign y = s ? d1 : d0;
endmodule
```

```verilog
module mux4 (input [3:0] d0, d1, d2, d3,
			 input [1:0] s, 
			 output [3:0] y);
assign y = s[1] ? (s[0] ? d3 : d2) : (s[0] ? d1 : d0);
endmodule
```

```verilog
module fulladder(input a, b, cin, output s, cout);
wire p, g;
assign p = a ^ b; 
assign g = a & b;
assign s = p ^ cin;
assign cout = g | (p & cin);
endmodule
```

```verilog
module tristate (input [3:0] a,
				 input en, 
				 output [3:0] y);
assign y = en ? a : 4'bz; 
endmodule
```

```verilog
module mux4 (input [3:0] d0, d1, d2, d3, 
			 input [1:0] s, 
			 output [3:0] y);
wire [3:0] low, high;
mux2 lowmux (d0, d1, s[0], low);
mux2 highmux (d2, d3, s[0], high);
mux2 finalmux (low, high, s[1], y);
endmodule
```

```verilog
module mux2 (input [3:0] d0, d1, 
			 input s, 
			 output [3:0] y);
tristate t0 (d0, ~s, y); 
tristate t1 (d1, s, y); 
endmodule
```

```verilog
module mux2_8 (input [7:0] d0, d1, 
			   input s, 
			   output [7:0] y);
mux2 lsbmux (d0[3:0], d1[3:0], s, y[3:0]); 
mux2 msbmux (d0[7:4], d1[7:4], s, y[7:4]); 
endmodule
```

```verilog
//REGISTER
module flop (input clk,
			 input [3:0] d,
			 output reg [3:0] q);
always @ (posedge clk)
q <= d;
endmodule
```

```verilog
//RESETTABLE REGISTER
module flopr (input clk,
			  input reset, 
			  input [3:0] d,
			  out reg [3:0] q);
// asynchronous reset
always @ (posedge clk, posedge reset) 
	if (reset) q <= 4’b0; 
	else q <= d;
endmodule
module flopr (input clk,
			  input reset, 
			  input [3:0] d,
			  out reg [3:0] q);
// synchronous reset 
always @ (posedge clk) 
	if (reset) q <= 4’b0; 
	else q <= d; 
endmodule
```

```verilog
//RESETTABLE ENABLED REGISTER
module flopenr (input clk,
			  input reset,
              input en,
			  input [3:0] d,
			  output reg [3:0] q);
// asynchronous reset
always @ (posedge clk, posedge reset) 
	if (reset) q <= 4’b0; 
	else if (en) q <= d;
endmodule
```

```verilog
//SYNCHRONIZER
module sync (input clk, 
			 input d,
			 output reg q);
reg n1;
always @ (posedge clk) 
	begin 
		n1 <= d;
    	q <= n1;
	end 
endmodule
```

```verilog
//D LATCH
module latch (input clk,
			  input [3:0] d, 
			  output reg [3:0] q);
always @ (clk, d) 
	if (clk) q <= d;
endmodule
```

```verilog
module fulladder (input a, b, cin, 
				  output reg s, cout);
reg p, g;
always @ (*) 
	begin 
		p = a ^ b; 
		g = a & b;
		s = p ^ cin; 
		cout = g | (p & cin);
end endmodule
```

```verilog
//SEVEN-SEGMENT DISPLAY DECODER
module sevenseg (input [3:0] data, 
				 output reg [6:0] segments);
always @ (*) 
   case (data) 
	0: segments = 7’b111_1110;
    1: segments = 7’b011_0000;
    2: segments = 7’b110_1101;
    3: segments = 7’b111_1001;
    4: segments = 7’b011_0011;
    5: segments = 7’b101_1011;
    6: segments = 7’b101_1111;
    7: segments = 7’b111_0000;
    8: segments = 7’b111_1111;
    9: segments = 7’b111_1011;
    default: segments = 7’b000_0000; 
   endcase
endmodule
```

```verilog
module divideby3FSM (input clk,
					 input reset,
                     output y);
reg [1:0] state, nextstate;
parameter S0 = 2'b00;
parameter S1 = 2'b01;
parameter S2 = 2'b10;
// state register
always @ (posedge clk, posedge reset)
	if (reset) 
		state <= S0;
    else
		state <= nextstate;
// next statelogic
always @ (*) 
	case (state) 
		S0: nextstate = S1;
        S1: nextstate = S2;
        S2: nextstate = S0;
        default: nextstate = S0; endcase
// output logic 
assign y = (state == S0);
endmodule
```

```verilog
//PATTERN RECOGNIZER MOORE FSM
module patternMoore (input clk,
					 input reset,
                     input a,
                     output y);
reg [2:0] state, nextstate;
parameter S0 = 3'b000;
parameter S1 = 3'b001;
parameter S2 = 3'b010;
parameter S3 = 3'b011;
parameter S4 = 3'b100;
// state register
always @ (posedge clk, posedge reset)
	if (reset)
    	state <= S0;
	else
		state <= nextstate;
// next state logic 
always @ (*) 
	case (state)
		S0: if (a) nextstate = S1; 
			else nextstate = S0;
		S1: if (a) nextstate = S2;
        	else nextstate = S0;
		S2: if (a) nextstate = S2;
        	else nextstate = S3;
		S3: if (a) nextstate = S4;
        	else nextstate = S0;
		S4: if (a) nextstate = S2;
        	else nextstate = S0;
		default: nextstate = S0;
        endcase
// output logic
assign y = (state == S4);
endmodule
```

```verilog
//PATTERN RECOGNIZER MEALY FSM
module patternMealy (input clk,
					 input reset,
                     input a,
                     output y);
reg [1:0] state, nextstate;
parameter S0 = 2'b00;
parameter S1 = 2'b01;
parameter S2 = 2'b10;
parameter S3 = 2'b11;
// state register
always @ (posedge clk, posedge reset)
	if (reset)
    	state <= S0;
	else
		state <= nextstate;
// next state logic 
always @ (*) 
	case (state)
		S0: if (a) nextstate = S1; 
			else nextstate = S0;
		S1: if (a) nextstate = S2;
        	else nextstate = S0;
		S2: if (a) nextstate = S2;
        	else nextstate = S3;
		S3: if (a) nextstate = S1;
        	else nextstate = S0;
		default: nextstate = S0;
        endcase
// output logic
assign y = (a & state == S3);
endmodule
```

```verilog
//PARAMETERIZED N-BIT MULTIPLEXERS
module mux2 # (parameter width = 8) 
			  (input [width-1:0] d0, d1,
              input s,
              output [width-1:0] y);
              assign y = s ? d1 : d0;
endmodule
module mux4_12 (input [11:0] d0, d1, d2, d3,
			    input [1:0] s,
                output [11:0] y);
wire [11:0] low, hi;
mux2 #(12) lowmux(d0, d1, s[0], low);
mux2 #(12) himux(d2, d3, s[1], hi);
mux2 #(12) outmux(low, hi, s[1], y);
endmodule
```

```verilog
//PARAMETERIZED N:2N DECODER
module decoder # (parameter N = 3) 
				 (input [N-1:0] a,
                  output reg [2**N-1:0] y);
always @ (*)
	begin
    	y = 0;
        y[a] = 1;
	end
endmodule
```

------

#### Full Adder

​	A full adder, introduced in Section 2.1, accepts the carry in, Cin, as shown in Figure 5.3. The figure also shows the output equations for S and Cout.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/fulladder.jpg" style="zoom:67%;" >

#### Carry-lookahead adder

​	The generate and propagate definitions extend to multiple-bit blocks. A block is said to generate a carry if it produces a carry out independent of the carry in to the block. The block is said to propagate a carry if it produces a carry out whenever there is a carry in to the block. We define Gi:j and Pi:j as generate and propagate signals for blocks spanning columns i through j.
​	A block generates a carry if the most significant column generates a carry, or if the most significant column propagates a carry and the previous column generated a carry, and so forth. For example, the generate logic for a block spanning columns 3 through 0 is $G_{3:0} = G_3 + P_3 (G_2 + P_2 (G1 + P1 G0))$

​	A block propagates a carry if all the columns in the block propagate the carry. For example, the propagate logic for a block spanning columns 3 through 0 is $P_{3:0} = P_3 P_2 P_1 P_0$

```verilog
//adder
module adder #(parameter N = 8)
			  (input [N-1:0] a, b,
               input cin,
			   output [N-1:0] s,
               output cout);
assign {cout, s} = a + b + cin;
endmodule
```

#### Subtraction

​	Recall from Section 1.4.6 that adders can add positive and negative numbers using two’s complement number representation. Subtraction is almost as easy: flip the sign of the second number, then add. Flipping the sign of a two’s complement number is done by inverting the bits and adding 1.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/Subtractor.jpg" style="zoom:67%;" >

```verilog
//Subtractor
module subtractor #(parameter N = 8)
				   (input [N-1:0] a, b,
                    output [N-1:0] y);
assign y = a - b;
endmodule
```

#### Comparators

​	An equality comparator produces a single output indicating whether A is equal to B (A == B). A magnitude comparator produces one or more outputs indicating the relative values of A and B.

​	The equality comparator is the simpler piece of hardware. Figure shows the symbol and implementation of a 4-bit equality comparator. It first checks to determine whether the corresponding bits in each column of A and B are equal, using XNOR gates. The numbers are equal if all of the columns are equal.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/comparator.jpg" style="zoom:67%;" >

​	Magnitude comparison is usually done by computing A - B and looking at the sign (most significant bit) of the result, as shown in Figure. If the result is negative (the sign bit is 1), then A is less than B. Otherwise A is greater than or equal to B.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/magnitude.jpg" style="zoom:67%;" >

```verilog
//comparators
module comparators # (parameter N - 8)
    				 (input [N-1:0] a, b,
                      output eq, neq,
                      output lt, lte,
                      output gt, gte);
 assign eq = (a == b);
 assign neq = (a != b);
 assign lt = (a < b);
 assign lte = (a <= b);
 assign gt = (a > b);
 assign gte = (a >= b);
endmodule
```

------

#### ALU

​	An Arithmetic/Logical Unit (ALU) combines a variety of mathematical and logical operations into a single unit. For example, a typical ALU might perform addition, subtraction, magnitude comparison, AND, and OR operations. The ALU forms the heart of most computer systems.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/ALU.jpg" style = "zoom:67%;">

​	Figure shows an implementation of the ALU. The ALU contains an N-bit adder and N two-input AND and OR gates. It also contains an inverter and a multiplexer to optionally invert input B when the F2 control signal is asserted. A 4:1 multiplexer chooses the desired function based on the F1:0 control signals.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/ALU2.jpg" style ="zoom:67%;">

------

#### Shifter and Rotator

​	Figure 5.16 shows the symbol and hardware of 4-bit shifters. The operators <<, >>, and >>> typically indicate shift left, logical shift right, and arithmetic shift right, respectively. Depending on the value of the 2-bit shift amount, shamt1:0, the output, Y, receives the input, A, shifted by 0 to 3 bits.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/shifter.jpg" style="zoom:67%;" >

#### Multiplication

​	Figure  shows the symbol, function, and implementation of a 4 x 4 multiplier. The multiplier receives the multiplicand and multiplier, A and B, and produces the product, P. Figure (b) shows how partial products are formed. Each partial product is a single multiplier bit (B3, B2, B1, or B0) AND the multiplicand bits (A3, A2, A1, A0). With N-bit operands, there are N partial products and N - 1 stages of 1-bit adders. For example, for a 4 x 4 multiplier, the partial product of the first row is B0 AND (A3, A2, A1, A0). This partial product is added to the shifted second partial product, B1 AND (A3, A2, A1, A0). Subsequent rows of AND gates and adders form and add the remaining partial products.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/multiplication.jpg" style="zoom:67%;" >

```verilog
//multiplier
module multiplier # (parameter N = 8)
					(input [N-1:0] a, b,
                     output [2*N-1:0] y);
 assign y = a * b;
endmodule
```

------

#### Counters

​	An N-bit binary counter, shown in Figure 5.30, is a sequential arithmetic circuit with clock and reset inputs and an N-bit output, Q. Reset initializes the output to 0. The counter then advances through all 2N possible outputs in binary order, incrementing on the rising edge of the clock.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/counters.jpg" style="zoom:67%;" >



```verilog
//countersbv
module counter #(parameter N = 8)
			    (input clk,
			     input reset,
			     output reg [N-1:0] q);
always @ (posedge clk or posedge reset)
	if (reset) q <= 0;
    else q <= q + 1;
endmodule
```

------

#### Shift register

​	A shift register has a clock, a serial input, $S_{in}$, a serial output, $S_{out}$, and N parallel outputs, $Q_{N-1:0}$, as shown in Figure 5.33. On each rising edge of the clock, a new bit is shifted in from $S_{in}$ and all the subsequent contents are shifted forward. The last bit in the shift register is available at $S_{out}$. Shift registers can be viewed as serial-to-parallel converters. The input is provided serially (one bit at a time) at Sin. After N cycles, the past N inputs are available in parallel at Q. A shift register can be constructed from N flip-flops connected in series, as shown in Figure 5.34. Some shift registers also have a reset signal to initialize all of the flip-flops.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/shift%20register2.jpg" style="zoom:60%;" >

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/shift%20register2.jpg" style="zoom:60%;" >

```verilog
//shiftreg
module shiftreg # (parameter N = 8) 
				  (input clk,
				   input reset, load,
				   input sin,
				   input [N-1:0] d,
				   output reg [N-1:0] q,
                   output sout);

 always @ (posedge clk or posedge reset) 
	if (reset) q<=0;
	else if (load) q <= d;
    else q <= {q[N-2:0], sin};
 assign sout = q[N-1];
endmodule
```

-------

#### Memory

​	The memory is organized as a two-dimensional array of memory cells. The memory reads or writes the contents of one of the rows of the array. This row is  specified by an Address. The value read or written is called Data. An array with N-bit addresses and M-bit data has $2^N$ rows and M columns. Each row of data is called a word. Thus, the array contains 2N M-bit words.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/memory1.jpg" style="zoom:67%;" >

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/memory2.jpg" style="zoom:67%;" >

​	Figure 5.39 shows a memory array with two address bits and three data bits. The two address bits specify one of the four rows (data words) in the array. Each data word is three bits wide. Figure 5.39(b) shows some possible contents of the memory array. The depth of an array is the number of rows, and the width is the number of columns, also called the word size. The size of an array is given as depth $\times$ width. Figure 5.39 is a 4-word $\times$ 3-bit array, or simply 4 $\times$ 3 array. The symbol for a 1024-word $\times$ 32-bit array is shown in Figure 5.40. The total size of this array is 32 kilobits (Kb).

#### Bit Cell

​	Memory arrays are built as an array of bit cells, each of which stores 1 bit of data. Figure 5.41 shows that each bit cell is connected to a wordline and a bitline. For each combination of address bits, the memory asserts a single wordline that activates the bit cells in that row. When the wordline is HIGH, the stored bit transfers to or from the bitline. Otherwise, the bitline is disconnected from the bit cell. The circuitry to store the bit varies with memory type. To read a bit cell, the bitline is initially left floating (Z). Then the wordline is turned ON, allowing the stored value to drive the bitline to 0 or 1. To write a bit cell, the bitline is strongly driven to the desired value. Then the wordline is turned ON, connecting the bitline to the stored bit. The strongly driven bitline overpowers the contents of the bit cell, writing the desired value into the stored bit.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/bit%20cell.jpg" style="zoom:67%;" >

#### Memory Port

​	Multiported memories can access several addresses simultaneously. Figure 5.43 shows a three-ported memory with two read ports and one write port. Port 1 reads the data from address A1 onto the read data output RD1. Port 2 reads the data from address A2 onto RD2. Port 3 writes the data from the write data input, WD3, into address A3 on the rising edge of the clock if the write enable, WE3, is asserted.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/memory3.jpg" style="zoom:67%;" >

#### DRAM

​	Figure 5.44 shows a DRAM bit cell. The bit value is stored on a capacitor. The nMOS transistor behaves as a switch that either connects or disconnects the capacitor from the bitline. When the wordline is asserted, the nMOS transistor turns ON, and the stored bit value transfers to or from the bitline.

​	As shown in Figure 5.45(a), when the capacitor is charged to $V_{DD}$, the stored bit is 1; when it is discharged to GND (Figure 5.45(b)), the stored bit is 0. The capacitor node is dynamic because it is not actively driven HIGH or LOW by a transistor tied to $V_{DD}$ or GND.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/DRAM.jpg" style="zoom:67%;" >

#### SRAM 

​	The data bit is stored on cross-coupled inverters like those described in Section 3.2. Each cell has two outputs, $bitline$ and $\overline{bitline}$. When the wordline is asserted, both nMOS transistors turn on, and data values are transferred to or from the bitlines.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/RAM.jpg" style="zoom:67%;" >

```verilog
//RAM
module ram # (parameter N = 6, M = 32) 
			 (input clk,
			  input we,
			  input [N-1:0] adr,
			  input [M-1:0] din,
			  output [M-1:0] dout);
always @ (posedge clk) 
 if (we) mem [adr] <= din;
 
assign dout = mem[adr];
endmodule
```



------

#### Register File

​	Figure 5.47 shows a 32-register  32-bit three-ported register file built from a three-ported memory similar to that of Figure 5.43. The register file has two read ports (A1/RD1 and A2/RD2) and one write port (A3/WD3). The 5-bit addresses, A1, A2, and A3, can each access all $2^5=32$ registers. So, two registers can be read and one register written simultaneously.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/registerfile.jpg" style="zoom:67%;" >

------

#### ROM

​	The contents of a ROM can be indicated using dot notation. Figure 5.49 shows the dot notation for a 4-word $\times$ 3-bit ROM containing the data from Figure 5.39. A dot at the intersection of a row (wordline) and a column (bitline) indicates that the data bit is 1.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/rom.jpg" style="zoom:67%;" >

```verilog
//ROM
module rom (input [1:0] adr,
			output reg [2:0] dout);
always @ (adr) 
 case (adr)
  2'b00: dout <= 3'b011;
  2'b01: dout <= 3'b110;
  2'b10: dout <= 3'b100;
  2'b11: dout <= 3'b010;
 endcase
endmodule
```

------

#### programmable logic array

​	PLAs are built from an AND array followed by an OR array, as shown in Figure 5.54. The inputs (in true and complementary form) drive an AND array, which produces implicants, which in turn are ORed together to form the outputs. An M $\times$ N $\times$ P-bit PLA has M inputs, N implicants, and P outputs.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/PLA.jpg" style="zoom:67%;" >

​	ROMs can be viewed as a special case of PLAs. A $2^M$-word $\times$ N-bit ROM is simply an M $\times$ 2M $\times$ N-bit PLA. The decoder behaves as an AND plane that produces all 2M minterms. The ROM array behaves as an OR plane that produces the outputs.

------

#### Design Process

​	In Figure 7.1, heavy lines are used to indicate 32-bit data busses. Medium lines are used to indicate narrower busses, such as the 5-bit address busses on the register file. Narrow blue lines are used to indicate control signals, such as the register file write enable. We will use this convention throughout the chapter to avoid cluttering diagrams with bus widths. Also, state elements usually have a reset input to put them into a known state at start-up. Again, to save clutter, this reset is not shown.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/design%20process.jpg" style="zoom:67%;" >

​	The *program counter* is an ordinary 32-bit register. Its output, PC, points to the current instruction. Its input, PC', indicates the address of the next instruction.

​	The instruction memory has a single read port.1 It takes a 32-bit instruction address input, A, and reads the 32-bit data (i.e., instruction) from that address onto the read data output, RD. 

​	The 32-element $\times$ 32-bit register file has two read ports and one write port. The read ports take 5-bit address inputs, A1 and A2, each specifying one of $2^5$ = 32 registers as source operands. They read the 32-bit register values onto read data outputs RD1 and RD2, respectively. The write port takes a 5-bit address input, A3; a 32-bit write data input, WD; a write enable input, WE3; and a clock. If the write enable is 1, the register file writes the data into the specified register on the rising edge of the clock. 

​	The data memory has a single read/write port. If the write enable, WE, is 1, it writes data WD into address A on the rising edge of the clock. If the write enable is 0, it reads address A onto RD.

------

#### Single-Cycle **Datapath**

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/register%20file.jpg" style="zoom:67%;" >

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/memory%20address.jpg" style="zoom:67%;" >

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/write%20data%20back.jpg" style="zoom:67%;" >

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/determine%20address%20of%20next%20instruction.jpg" style="zoom:67%;" >

​	

​	The processor’s actions depend on the specific instruction that was fetched. First we will work out the datapath connections for the $lw$ instruction. Then we will consider how to generalize the datapath to handle the other instructions.

For a $lw$ instruction, the next step is to read the source register containing the base address. This register is specified in the rs field of the instruction, $Instr_{25:21}$. These bits of the instruction are connected to the address input of one of the register file read ports, A1, as shown in Figure 7.3. The register file reads the register value onto RD1.

​	 The $lw$ instruction also requires an offset. The offset is stored in the immediate field of the instruction,$Instr_{15:0}$. Because the 16-bit immediate might be either positive or negative, it must be sign-extended to 32 bits, as shown in Figure 7.4. The 32-bit sign-extended value is called $SignImm$. Recall from Section 1.4.6 that sign extension simply copies the sign bit (most significant bit) of a short input into all of the upper bits of the longer output. Specifically, $SignImm_{15:0}=Instr_{15:0}$ and$SignImm_{31:16}=Instr_{15}$.

​	The processor must add the base address to the offset to find the address to read from memory. Figure 7.5 introduces an ALU to perform this addition. The ALU receives two operands, $SrcA$ and $SrcB$. $SrcA$ comes from the register file, and $SrcB$ comes from the sign-extended immediate. The ALU can perform many operations, as was described in Section 5.2.4. The 3-bit $ALUControl$ signal specifies the operation. The ALU generates a 32-bit $ALUResult$ and a Zero flag, that indicates whether $ALUResult == 0$. For a $lw$ instruction, the $ALUControl$ signal should be set to 010 to add the base address and offset. $ALUResult$ is sent to the data memory as the address for the load instruction, as shown in Figure 7.5.

​	The data is read from the data memory onto the $ReadData$ bus, then written back to the destination register in the register file at the end of the cycle, as shown in Figure 7.6. Port 3 of the register file is the write port. The destination register for the $lw$ instruction is specified in the rt field, $Instr_{20:16}$, which is connected to the port 3 address input, A3, of the register file. The $ReadData$ bus is connected to the port 3 write data input, WD3, of the register file. A control signal called $RegWrite$ is connected to the port 3 write enable input, WE3, and is asserted during a $lw$ instruction so that the data value is written into the register file. The write takes place on the rising edge of the clock at the end of the cycle.

​	While the instruction is being executed, the processor must compute the address of the next instruction, PC'. Because instructions are 32 bits = 4 bytes, the next instruction is at PC + 4. Figure 7.7 uses another adder to increment the PC by 4. The new address is written into the program counter on the next rising edge of the clock. This completes the datapath for the $lw$instruction.



<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/datapath%20enhancements%20for%20R-type%20instruction.jpg" style="zoom:67%;" >

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/datapath%20enhancements%20for%20beq%20instruction.jpg" style="zoom:67%;" >



<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/control%20unit%20internal%20structure.jpg" style="zoom:67%;" >

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/complete%20single-cycle%20mips%20processor.jpg" style="zoom:67%;" >

------

#### Multicycle Processor

​	Figure 7.17 shows that the PC is simply connected to the address input of the instruction memory. The instruction is read and stored in a new nonarchitectural Instruction Register so that it is available for future cycles. The Instruction Register receives an enable signal, called IRWrite, that is asserted when it should be updated with a new instruction.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/fetch%20instruction%20from%20memory.jpg" style="zoom:67%;" >

​	For a lw instruction, the next step is to read the source register containing the base address. This register is specified in the rs field of the instruction, $Instr_{25:21}$. These bits of the instruction are connected to one of the address inputs, A1, of the register file, as shown in Figure 7.18. The register file reads the register onto RD1. This value is stored in another nonarchitectural register, A.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/read%20source%20operand%20from%20register%20file.jpg" style="zoom:67%;" >

​	The address of the load is the sum of the base address and offset. We use an ALU to compute this sum, as shown in Figure 7.20. $ALUControl$ should be set to 010 to perform an addition. $ALUResult$ is stored in a nonarchitectural register called $ALUOut$.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/add%20base%20address%20to%20offset.jpg" style="zoom:67%;" >

​	The next step is to load the data from the calculated address in the memory. We add a multiplexer in front of the memory to choose the memory address, Adr, from either the PC or $ALUOut$, as shown in Figure 7.21. The multiplexer select signal is called $IorD$, to indicate either an instruction or data address. The data read from the memory is stored in another nonarchitectural register, called $Data$.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/load%20data%20from%20memory.jpg" style="zoom:67%;" >

​	Finally, the data is written back to the register file, as shown in Figure 7.22. The destination register is specified by the *rt* field of the instruction, $Instr_{20:16}$.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/write%20data%20back%20to%20register%20file.jpg" style="zoom:67%;" >

​	we must insert source multiplexers to choose the PC and the constant 4 as ALU inputs, as shown in Figure 7.23. A two-input multiplexer controlled by $ALUSrcA$ chooses either the PC or register A as $SrcA$. A four-input multiplexer controlled by $ALUSrcB $chooses either 4 or $SignImm$ as $SrcB$. We use the other two multiplexer inputs later when we extend the datapath to handle other instructions. (The numbering of inputs to the multiplexer is arbitrary.) To update the PC, the ALU adds $SrcA (PC)$ to $SrcB (4)$, and the result is written into the program counter register. The $PCWrite$ control signal enables the PC register to be written only on certain cycles.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/increment%20PC%20by%204.jpg" style="zoom:67%;" >

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/enhanced%20datapath%20for%20R-type%20instruction.jpg" style="zoom:67%;" >

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/enhanced%20datapath%20for%20R-type%20instruction.jpg" style="zoom:67%;" >

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/enhanced%20datapath%20for%20beq%20instruction.jpg" style="zoom:67%;" >

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/complete%20multicycle%20mips%20processor.jpg" style="zoom:67%;" >

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/complete%20multicycle%20mips%20processor.jpg" style="zoom:67%;" >

-----

#### Pipelined Processor

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/correct%20pipelined%20datapath.jpg" style="zoom:67%;" >

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/pipelined%20processor%20with%20control.jpg" style="zoom:67%;" >



***

​	Forwarding is necessary when an instruction in the Execute stage has a source register matching the destination register of an instruction in the Memory or Writeback stage. Figure 7.50 modifies the pipelined processor to support forwarding. It adds a hazard detection unit and two forwarding multiplexers. The hazard detection unit receives the two source registers from the instruction in the Execute stage and the destination registers from the instructions in the Memory and Writeback stages. 

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/abstract%20pipeline%20diagram%20illustrating%20forwarding.jpg" style="zoom:67%;" >

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/pipelined%20processor%20with%20forwarding%20to%20solve%20hazards.jpg" style="zoom:67%;" >

------

​	Stalls are supported by adding enable inputs (*EN*) to the Fetch and Decode pipeline registers and a synchronous reset/clear (*CLR*) input to the Execute pipeline register. When a `lw​` stall occurs, *StallD* and *StallF* are asserted to force the Decode and Fetch stage pipeline registers to hold their old values. *FlushE* is also asserted to clear the contents of the Execute stage pipeline register, introducing a bubble.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/abstract%20pipeline%20diagram%20illustrating%20stall%20to%20solve%20hazards.jpg" style="zoom:67%;" >

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/pipelined%20processor%20with%20stalls%20to%20solve%20lw%20data%20hazard.jpg" style="zoom:67%;" >

------

​	Figure 7.56 modifies the pipelined processor to move the branch decision earlier and handle control hazards. An equality comparator is added to the Decode stage and the *PCSrc* AND gate is moved earlier, so that *PCSrc* can be determined in the Decoder stage rather than the Memory stage. The *PCBranch* adder must also be moved into the Decode stage so that the destination address can be computed in time. The synchronous clear input (*CLR*) connected to *PCSrcD* is added to the Decode stage pipeline register so that the incorrectly fetched instruction can be flushed when a branch is taken.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/pipilined%20processor%20handling%20branch%20control%20hazard.jpg" style="zoom:67%;" />

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/pipelined%20process%20handling%20data%20dependencies%20for%20branch%20instructions.jpg" style="zoom:67%;" />

​	The function of the stall detection logic for a branch is given below. The processor must make a branch decision in the Decode stage. If either of the sources of the branch depends on an ALU instruction in the Execute stage or on a lw instruction in the Memory stage, the processor must stall until the sources are ready.

------

​	In summary, RAW data hazards occur when an instruction depends on the result of another instruction that has not yet been written into the register file. The data hazards can be resolved by forwarding if the result is computed soon enough; otherwise, they require stalling the pipeline until the result is available. Control hazards occur when the decision of what instruction to fetch has not been made by the time the next instruction must be fetched. Control hazards are solved by predicting which instruction should be fetched and flushing the pipeline if the prediction is later determined to be wrong. Moving the decision as early as possible minimizes the number of instructions that are flushed on a misprediction. You may have observed by now that one of the challenges of designing a pipelined processor is to understand all the possible interactions between instructions and to discover all the hazards that may exist. Figure 7.58 shows the complete pipelined processor handling all of the hazards.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/enhanced%20datapath%20for%20R-type%20instruction.jpg" style="zoom:67%;" />

------

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/MIPS%20single-cycle%20processor%20interfaced%20to%20external%20memory.jpg" style="zoom:67%;" />

```verilog
//SINGLE-CYCLE MIPS PROCESSOR
module mips (input clk, reset,
		    output [31:0] pc,
             input [31:0] instr,
             output memwrite,
             output [31:0] aluout, writedata,
             input [31:0] readdata);
wire memtoreg, branch,
     alusrc, regdst, regwrite, jump;
wire [2:0] alucontrol;
controller c(instr[31:26], instr[5:0], zero,
             memtoreg, memwrite, pcsrc,
             alusrc, regdst, regwrite, jump, 
             alucontrol);
datapath dp(clk, reset, memtoreg, pcsrc,
            alusrc, regdst, regwrite, jump,
            alucontrol,
            zero, pc, instr,
            aluout, writedata, readdata);
endmodule
```

```verilog
//CONTROLLER
module controller (input [5:0] op, funct,
                   input zero,
				  output memtoreg, memwrite,
                   output  pcsrc, alusrc,
                   output regdst, regwrite,
                   output jump,
                   output [2:0] alucontrol);
wire [1:0] aluop;
wire branch;
maindec md (op, memtoreg, memwrite, branch,
            alusrc, regdst, regwrite, jump,
            aluop);
aludec ad (funct, aluop, alucontrol);
    
assign pcsrc = branch & zero;
endmodule
```

```verilog
//MAIN DECODER
module maindec(input [5:0] op,
               output memtoreg, memwrite,
               output branch, alusrc,
               output regdst, regwrite,
               output jump,
               output[1:0] aluop);
   
    reg [8:0] controls;
    
    assign {regwrite, regdst, alusrc,
           branch, memwrite,
            memtoreg, jump, aluop} = controls;
    
    always @ (*)
        case (op)
            6'b000000: controls <= 9'b110000010; //Rtyp
            6'b100011: controls <= 9'b101001000; //LW
            6'b101011: controls <= 9'b001010000; //SW
            6'b000100: controls <= 9'b000100001; //BEQ
            6'b001000: controls <= 9'b101000000; //ADDI
            6'b000010: controls <= 9'b000000100; //J
            default: controls <= 9'bxxxxxxxxx; //???
        endcase
endmodule
```

```verilog
//ALU DECODER
module aludec (input [5:0] funct,
               input [1:0] aluop,
               output reg [2:0] alucontrol);
    always @ (*)
        case (aluop)
            2'b00: alucontrol <= 3'b010; //add
            2'b01: alucontrol <= 3'b110; //sub
            default: case (funct) //RTYPE
                6'b100000: alucontrol <= 3'b010; // ADD
                6'b100010: alucontrol <= 3'b110; // SUB
                6'b100100: alucontrol <= 3'b000; // AND
                6'b100101: alucontrol <= 3'b001; // OR
                6'b101010: alucontrol <= 3'b111; // SLT
                default: alucontrol <= 3'bxxx; // ???
            endcase
        endcase
endmodule
```

```verilog
//DATAPATH
module datapath(input clk, reset,
                input memtoreg, pcsrc,
                input alusrc, regdst,
                input regwrite, jump,
                input [2:0] alucontrol,
                output zero,
                output [31:0] pc,
                input [31:0] instr,
                output [31:0] aluout, writedata,
                input [31:0] readdata);
    
    wire [4:0] writereg;
    wire [31:0] pcnext, pcnextbr, pcplus4, pcbranch;
    wire [31:0] signimm, signimmsh;
    wire [31:0] srca, srcb;
    wire [31:0] result;
    
    //next PC logic
    flopr #(32) pcreg(clk, reset, pcnext, pc);
    adder pcadd1(pc, 32'b100, pcplus4);
    sl2 immsh(signimm, signimmsh);
    adder pcadd2(pcplus4, signimmsh, pcbranch);
    mux2 #(32) pcbrmux(pcplus4, pcbranch, pcsrc, pcnextbr);
    mux2 #(32) pcmux(pcnextbr, {pcplus4[31:28], instr[25:0], 2'b00}, jump, pcnext);
    
    // register file logic
	regfile rf(clk, regwrite, instr[25:21],
               instr[20:16], writereg,
               result, srca, writedata);
    mux2 #(5) wrmux(instr[20:16], instr[15:11],
                    regdst, writereg);
    mux2 #(32) resmux(aluout, readdata,
                      memtoreg, result);
    signext se(instr[15:0], signimm);
    
    // ALU logic
    mux2 #(32) srcbmux(writedata, signimm, alusrc, srcb);
    alu alu(srca, srcb, alucontrol, aluout, zero);
endmodule
```

```verilog
//REGISTER FILE
module regfile(input clk,
               input we3,
               input [4:0] ra1, ra2, wa3,
               input [31:0] wd3
               output [31:0] rd1, rd2);
    
    reg [31:0] rf[31:0];
    
    // three ported register file
    // read two ports combinationally
    // write third port on rising edge of clock
    // register 0 hardwired to 0
    
    always @(posedge clk)
        if(we3) rf[wa3] <= wd3;
    
    assign rd1 = (ra1 != 0) ? rf[ra1] : 0;
    assign rd2 = (ra2 != 0) ? rf[ra2] : 0;
endmodule
```

```verilog
//ADDER
module adder(input [31:0] a, b,
             output [31:0] y);
    
    assign y= a + b;
endmodule
```

```verilog
//LEFT SHIFT (MULTIPLY BY 4)
module sl2(input [31:0] a,
           output [31:0] y);
    
    //shift left by 2
    assign y = {a[29:01], 2'b00};
endmodule
```

```verilog
//SIGN EXTENSION
module signext(input [15:0] a,
               output [31:0] y);
    
    assign y = {{16{a[15]}}, a};
endmodule
```

```verilog
//RESETTABLE FLIP-FLOP
module flopr #(parameter WIDTH = 8)
    (input clk, reset,
     input [WIDTH-1:0] d,
     output reg[WIDTH-1:0] q);
    
    always @(posedge clk, posedge reset)
        if(reset) q <= 0;
    else q <= d;
endmodule
```

```verilog
//2:1 MULTIPLEXER
module mux2 #(parameter WIDTH = 8)
    (input [WIDTH-1:0] d0, d1,
     input s,
     output [WIDTH-1:0] y);
    
    assign y = s ? d1 : d0;
endmodule
```

------

### Memory System

​	The processor communicates with the memory system over a *memory interface*. Figure 8.1 shows the simple memory interface used in our multicycle MIPS processor. The processor sends an address over the Address bus to the memory system. For a read, *MemWrite* is 0 and the memory returns the data on the *ReadData* bus. For a write, *MemWrite* is 1 and the processor sends data to memory on the *WriteData* bus.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/The%20memory%20interface.jpg" style="zoom:67%;" >

​	Figure 8.3 summarizes the memory hierarchy of the computer system discussed in the rest of this chapter. The processor first seeks data in a small but fast cache that is usually located on the same chip. If the data is not available in the cache, the processor then looks in main memory. If the data is not there either, the processor fetches the data from virtual memory on the large but slow hard disk. 

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/A%20typical%20memory%20hierarchy.jpg" style="zoom:67%;" >

------

​	Recall that temporal locality means that the processor is likely to access a piece of data again soon if it has accessed that data recently. Therefore, when the processor loads or stores data that is not in the cache, the data is copied from main memory into the cache. Subsequent requests for that data hit in the cache.
​	Recall that spatial locality means that, when the processor accesses a piece of data, it is also likely to access data in nearby memory locations. Therefore, when the cache fetches one word from memory, it may also fetch several adjacent words. This group of words is called a cache block. The number of words in the cache block, b, is called the block size. A cache of capacity C contains B = C/b blocks.

​	

​	A cache is organized into S sets, each of which holds one or more blocks of data. The relationship between the address of data in main memory and the location of that data in the cache is called the mapping. Each memory address maps to exactly one set in the cache. Some of the address bits are used to determine which cache set contains the data. If the set contains more than one block, the data may be kept in any of the blocks in the set.

------

### Direct Mapped cache

​	A direct mapped cache has one block in each set, so it is organized into S = B sets. This mapping is illustrated in Figure 8.5 for a direct mapped cache with a capacity of eight words and a block size of one word. The cache has eight sets, each of which contains a one-word block. The bottom two bits of the address are always 00, because they are word aligned. The next $log_28 = 3 bits$ indicate the set onto which the memory address maps. Thus, the data at addresses 0x00000004, 0x00000024, . . . , 0xFFFFFFE4 all map to set 1, as shown in blue. Likewise, data at addresses 0x00000010, . . . , 0xFFFFFFF0 all map to set 4, and so forth. Each main memory address maps to exactly one set in the cache.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/Mapping%20of%20main%20memory%20to%20a%20direct%20mapped%20cache.jpg" style="zoom:67%;" >

​	Sometimes, such as when the computer first starts up, the cache sets contain no data at all. The cache uses a valid bit for each set to indicate whether the set holds meaningful data. If the valid bit is 0, the contents are meaningless.

​	Figure 8.7 shows the hardware for the direct mapped cache of Figure 8.5. The cache is constructed as an eight-entry SRAM. Each entry, or set, contains one line consisting of 32 bits of data, 27 bits of tag, and 1 valid bit. The cache is accessed using the 32-bit address. The two least significant bits, the byte offset bits, are ignored for word accesses. The next three bits, the set bits, specify the entry or set in the cache.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/Direct%20mapped%20cache%20with%208%20sets.jpg" style="zoom:67%;" >

------

### Multi-way Set Associative Cache

​	Figure 8.9 shows the hardware for a C = 8-word, N = 2-way set associative cache. The cache now has only S = 4 sets rather than 8. Thus, only $log_24 = 2$ set bits rather than 3 are used to select the set. The tag increases from 27 to 28 bits. Each set contains two ways or degrees of associativity. Each way consists of a data block and the valid and tag bits. The cache reads blocks from both ways in the selected set and checks the tags and valid bits for a hit. If a hit occurs in one of the ways, a multiplexer selects data from that way.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/Two-way%20set%20associative%20cache.jpg" style="zoom:67%;" >

-----

### Fully Associative Cache

​	A fully associative cache contains a single set with B ways, where B is the number of blocks. A memory address can map to a block in any of these ways. A fully associative cache is another name for a B-way set associative cache with one set.
​	Figure 8.11 shows the SRAM array of a fully associative cache with eight blocks. Upon a data request, eight tag comparisons (not shown) must be made, because the data could be in any block. Similarly, an 8:1 multiplexer chooses the proper data if a hit occurs. 

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/Eight-block%20fully%20associative%20cache.jpg" style="zoom:67%;" >

------

### Block Size

​	Figure 8.12 shows the hardware for a C = 8-word direct mapped cache with a b = 4-word block size. The cache now has only B = C/b = 2 blocks. A direct mapped cache has one block in each set, so this cache is organized as two sets. Thus, only $log_22 = 1$ bit is used to select the set. A multiplexer is now needed to select the word within the block. The multiplexer is controlled by the $log_24 = 2$ block offset bits of the address. The most significant 27 address bits form the tag. Only one tag is needed for the entire block, because the words in the block are at consecutive addresses.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/Direct%20mapped%20cache%20with%20two%20sets%20and%20a%20four-word%20block%20size.jpg" style="zoom:67%;" >

------

​	Caches are organized as two-dimensional arrays. The rows are called sets, and the columns are called ways. Each entry in the array consists of a data block and its associated valid and tag bits. Caches are characterized by

* capacity C

* block size b (and number of blocks, B = C/b)
* number of blocks in a set (N)

​    Table 8.2 summarizes the various cache organizations. Each address in memory maps to only one set but can be stored in any of the ways. 

​	Cache capacity, associativity, set size, and block size are typically powers of 2. This makes the cache fields (tag, set, and block offset bits) subsets of the address bits.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/Cache%20organizations.jpg" style="zoom:67%;" >

------

### VIRTUAL MEMORY

​	Virtual memory is divided into virtual pages, typically 4 KB in size. Physical memory is likewise divided into physical pages of the same size. A virtual page may be located in physical memory (DRAM) or on the disk. 

​	Figure 8.20 shows a virtual memory that is larger than physical memory. The rectangles indicate pages. Some virtual pages are present in physical memory, and some are located on the disk. The process of determining the physical address from the virtual address is called address translation. If the processor attempts to access a virtual address that is not in physical memory, a page fault occurs, and the operating system loads the page from the hard disk into physical memory.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/Virtual%20and%20physical%20pages.jpg" style="zoom:67%;" >

​	The most significant bits of the virtual or physical address specify the virtual or physical page number. The least significant bits specify the word within the page and are called the page offset.

------

​	Figure 8.21 illustrates the page organization of a virtual memory system with 2 GB of virtual memory and 128 MB of physical memory divided into 4-KB pages. MIPS accommodates 32-bit addresses. With a 2-GB = $2^{31}$-byte virtual memory, only the least significant 31 virtual address bits are used; the 32nd bit is always 0. Similarly, with a 128-MB = $2^{27}$-byte physical memory, only the least significant 27 physical address bits are used; the upper 5 bits are always 0. Because the page size is 4 KB = $2^{12}$ bytes, there are $2^{31}/2^{12}$ = $2^{19}$ virtual pages and $2^{27}$/$2^{12}$ = $2^{15}$ physical pages. Thus, the virtual and physical page numbers are 19 and 15 bits, respectively. Physical memory can only hold up to 1/16th of the virtual pages at any given time. The rest of the virtual pages are kept on disk. The least significant 12 bits of the virtual and physical addresses are the same and specify the page offset within the virtual and physical pages. Only the page number needs to be translated to obtain the physical address from the virtual address.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/physical%20and%20virtual%20pages.jpg" style="zoom:67%;" >

-------

​	Figure 8.22 illustrates the translation of a virtual address to a physical address. The least significant 12 bits indicate the page offset and require no translation. The upper 19 bits of the virtual address specify the virtual page number (VPN) and are translated to a 15-bit physical page number (PPN). 

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/Translation%20from%20virtual%20address%20to%20physical%20address.jpg" style="zoom:67%;" >

------

#### The Page Table

​	The processor uses a page table to translate virtual addresses to physical addresses. Recall that the page table contains an entry for each virtual page. This entry contains a physical page number and a valid bit. If the valid bit is 1, the virtual page maps to the physical page specified in the entry. Otherwise, the virtual page is found on disk.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/The%20page%20table%20for%20Figure%208.21.jpg" style="zoom:67%;" >

​	The page table can be stored anywhere in physical memory, at the discretion of the OS. The processor typically uses a dedicated register, called the page table register, to store the base address of the page table in physical memory.

​	<u>To perform a load or store, the processor must first translate the virtual address to a physical address and then access the data at that physical address. The processor extracts the virtual page number from the virtual address and adds it to the page table register to find the physical address of the page table entry. The processor then reads this page table entry from physical memory to obtain the physical page number. If the entry is valid, it merges this physical page number with the page offset to create the physical address. Finally, it reads or writes data at this physical address.</u> Because the page table is stored in physical memory, each load or store involves two physical memory accesses.

------

#### The Translation Lookaside Buffer

​	In general, the processor can keep the last several page table entries in a small cache called a ***translation lookaside buffer (TLB)***. The processor “looks aside” to find the translation in the TLB before having to access the page table in physical memory. 

​	A TLB is organized as a fully associative cache and typically holds 16 to 512 entries. Each TLB entry holds a virtual page number and its corresponding physical page number. The TLB is accessed using the virtual page number. If the TLB hits, it returns the corresponding physical page number. Otherwise, the processor must read the page table in physical memory.

​	Figure 8.25 shows the two-entry TLB with the request for virtual address 0x247C. The TLB receives the virtual page number of the incoming address, 0x2, and compares it to the virtual page number of each entry.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/Address%20translation%20using%20a%20two-entry%20TLB.jpg" style="zoom:67%;" >

------

#### Multilevel Page Tables

​	To conserve physical memory, page tables can be broken up into multiple (usually two) levels. The first-level page table is always kept in physical memory. It indicates where small second-level page tables are stored in virtual memory. The second-level page tables each contain the actual translations for a range of virtual pages. If a particular range of translations is not actively used, the corresponding second-level page table can be swapped out to the hard disk so it does not waste physical memory.

​	In a two-level page table, the virtual page number is split into two parts: the ***page table number*** and the ***page table offset***, as shown in Figure 8.26. The page table number indexes the first-level page table, which must reside in physical memory. The first-level page table entry gives the base address of the second-level page table or indicates that it must be fetched from disk when V is 0. The page table offset indexes the second-level page table. The remaining 12 bits of the virtual address are the page offset, as before, for a page size of $2^{12}$ = 4​ KB.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/Hierarchical%20page%20tables.jpg" style="zoom:67%;" >

------

#### MEMORY-MAPPED I/O

​	In a system with memory-mapped I/O, a load or store may access either memory or an I/O device. Figure 8.28 shows the hardware needed to support two memory-mapped I/O devices. An address decoder determines which device communicates with the processor. It uses the ***Address*** and ***MemWrite*** signals to generate control signals for the rest of the hardware. The ***ReadData*** multiplexer selects between memory and the various I/O devices. Write-enabled registers hold the values written to the I/O devices.

<img src="https://cdn.jsdelivr.net/gh/MaxKev1n/Pictures//Digital%20Design%20and%20Computer%20Architecture/Support%20hardware%20for%20memory-mapped%20IO.jpg" style="zoom:67%;" >

------

***Written By ChenZihang 2021.4.21***

---

