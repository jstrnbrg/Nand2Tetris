# Nand2Tetris


## Week 1
Every digital device is based on a set of chips to store and process information. All these chips are made of elementary logic gates. 

### Truth tables
…..
### Boolean Expressions
…..
### Canonical Represantation
Starting from the truth table, we focus on all rows in which the function has a vale of 1. For each of these rows w e construct a term by and-ing together Variables (or their negation). Then we Or-together all these terms (where the function had a value of 1). That gives us a Boolean expression that is equivalent to the truth table. Then we try to shorten it. 
—>Conclusion:  Every Boolean function, no matter how complex, can be expressed using three Boolean operators only: And, Or, and Not. 

*The number of Boolean functions for n inputs is 2^2^n*

### Gates
A gate is a physical device that implements a Boolean function. 
…..

### NAND gate
The NAND (not And) and the XOR (exclusive or) gate have an intersting property. Both on their own are sufficient to construct the AND, OR  and NOT Gate, by wiring  them  together in different ways. 
—> AND, OR, NOT can be built from NAND (or XOR)
—> And since every Boolean function can be constructed from And, Or, and Not operations using the  canonical representation method, it follows that every Boolean function can be constructed from NAND operations alone. 
—> we can implement any boolean function just from NAND gates

### Logic design
Simply put, logic design is the art of interconnecting gates in order to implement more complex functionality, leading to the notion of composite gates. 
The art of logic design can be described as follows: Given a gate specification (interface), find an efficient way to implement it using other gates that were already implemented. 


### Two perspectives
When looking at a gate we can view it from two different perspectives.
External and internal.
Internal: shows the gates architecture, I.e. what other gates the gates is constructed of.
External shows the gate’s interface, i.e. the input and output pins


### Basic logic gates: 
- NOT: converts the input from 1 to 0 and vice versa.
	- The first and last rows of the NAND gate are a NOT gate.
```
/**
 * Not gate. out = not in.
 */

CHIP Not {

    IN  in;
    OUT out;

    PARTS:
    Nand(a=in, b=in, out=out);
}
```
- AND: returns 1 when all inputs are 1, otherwise returns 0
	- An AND gate is a negated NAND gate. 
```
/**
 * And gate: out = a and b.
 */

CHIP And {

    IN  a, b;
    OUT out;

    PARTS:
    Nand(a=a, b=b, out=c);
    Nand(a=c, b=c, out=out);
}
```
- OR: returns 1, when at least one of the inputs is 1, otherwise returns 0
	- Negating the inputs and then using a NAND gate equals the OR function. 
```
/**
 * Or gate. out = a or b
 */

CHIP Or {

    IN  a, b;
    OUT out;

    PARTS:
    Nand(a=a, b=a, out=c); //negate a
    Nand(a=b, b=b, out=d); //negate b
    Nand(a=c, b=d, out=out);
}


```
- XOR: returns 1, when both inputs have opposing values. Otherwise returns 0.
	- NAND returns 0 when both inputs are 1. Or Returns 1 when both inputs are 1. Therefore we can AND the outputs of these gates and the resulting gate will only output 1 when just one of the inputs is 1.
```
/**
 *  Exclusive-or gate.  out = a xor b.
 */

CHIP Xor {

    IN  a, b;
    OUT out;

    PARTS:
    Nand(a=a, b=b, out=c);
    Or(a=a, b=b, out=d);
    And(a=c, b=d, out=out);
}
```
- Multiplexor: Three input gate, where one of the inputs is used to select, which of the other two inputs should be output.
	- In this case:  if selector is 0, the output equals a, if the selector is 1, the output equals b.
	- Canonical Represantation:  Output 1 if  a is 1 and selector is 0 or if  b is 1 and selector is 1
```
/** 
 * Multiplexor.  If sel=0 then out = a else out = b.
 */

CHIP Mux {

    IN  a, b, sel;
    OUT out;

    PARTS:
    Not(in=sel, out=nsel);
    And(a=a, b=nsel, out=c); //if a=1 and sel=0
    And(a=b, b=sel, out=d); //if b=1 and sel=1
    Or(a=c, b=d, out=out);
}
```

- Demultiplexor: Has one data input and one selector input  and forwards the data based on the value of the selector.
	- If input is 0 and sel is 0, then we don’t need to do anything.
	- If input is 1, then:

```
/** 
 * Demultiplexer. If sel = 0 then {a = in; b = 0} else {a = 0; b = in}

in		sel		a		b
0		0		0		0 	//nothing to do here
0		1		0		0	//these two lines represent an XOR gate where the output at a is equivalent to in.	
1		0 		1		0 									
1		1		0		1	//AND gate. Output sent to b
 */

CHIP DMux {

    IN  in, sel;
    OUT a, b;

    PARTS:
    Xor(a=in, b=sel, out=c); //line 2&3
    And(a=in, b=c, out=a); //comparing output to in. 
    And(a=in, b=sel, out=b); //last line
}
```


### n-bit Gates: 
- Multi bit arrays are called buses. 
- Multi bit Gates apply the function on each bit (pair) individually. 
- Example: 
```
/**
 * 16-bit negation gate: for i=0..15 out[i] = not in[i]
 */

CHIP Not16 {

    IN  in[16];
    OUT out[16];

    PARTS:
    Nand(a=in[0], b=in[0], out=out[0]);
    Nand(a=in[1], b=in[1], out=out[1]);
    Nand(a=in[2], b=in[2], out=out[2]);
    Nand(a=in[3], b=in[3], out=out[3]);
    Nand(a=in[4], b=in[4], out=out[4]);
    Nand(a=in[5], b=in[5], out=out[5]);
    Nand(a=in[6], b=in[6], out=out[6]);
    Nand(a=in[7], b=in[7], out=out[7]);
    Nand(a=in[8], b=in[8], out=out[8]);
    Nand(a=in[9], b=in[9], out=out[9]);
    Nand(a=in[10], b=in[10], out=out[10]);
    Nand(a=in[11], b=in[11], out=out[11]);
    Nand(a=in[12], b=in[12], out=out[12]);
    Nand(a=in[13], b=in[13], out=out[13]);
    Nand(a=in[14], b=in[14], out=out[14]);
    Nand(a=in[15], b=in[15], out=out[15]);
}
```

### n-Way Gates
- Accept n number of inputs., either as is single bits or as buses.
- Compares the output to the next input and so on.
- e.g. Multi way OR: outputs 1 if at least one input is 1, if none is 1 it returns 0.
```
/**
 * 8-way Or gate.  out = in[0] or in[1] or ... or in[7]
 */

CHIP Or8Way {

    IN  in[8];
    OUT out;

    PARTS:
    Or(a=in[0], b=in[1], out=out01);
    Or(a=in[2], b=in[3], out=out23);
    Or(a=in[4], b=in[5], out=out45);
    Or(a=in[6], b=in[7], out=out67);
    Or(a=out01, b=out23, out=out03);
    Or(a=out45, b=out67, out=out47);
    Or(a=out03, b=out47, out=out);
}
```



## Week 2
### Binary numbers
```
Decimal Binary
	0		0
	1		1
	2		10
	3		11
	4		100
	5		101
	6		110
	7		111

Deciamal: 789 means 7x100 + 8x10 + 9x1 = 7x10^2 + 8x10^1 + 9x10^0
Binary: 110110 means  1x32 + 1x16 + 0x8 + 1x4 + 1x2 + 0x1 = 1x2^5 + 1x2^4 + 0x2^3 + 1x2^2 + 1x2^1 + 0x2^0
```

What is the biggest decimal number that  can be represented with k binary digits?		 2^k-1 e.g 2^8-1 = 255

### Binary addition
```
      1 1 1
	0 0 0 1 0 1 0 1
+	0 1 0 1 1 1 0 0
-----------------------
	0 1 1 1 0 0 0 1

0 + 0 = 0
0 + 1 = 1
1 + 1 = 0 and carry 1
1 + 1 + 1 = 1 and carry 1
```

### Negative Numbers
```
0000	0
0001	1
0010	2
0011	3
0100	4
0101	5
0110	5
0111	7

1000	-8	(8)
1001	-7	(9)
1010	-6	(10)
1011	-5	(11)
1100	-4	(12)
1101	-3	(13)
1110	-2	(14)
1111	-1	(15)
```

**Twos complement:** 
	positive numbers: 	from 0 to (2^n-1)-1
	negative numbers: 	from -1 to -2^n-1

The main advantage of using twos complement over other methods (e.g. using the first bit as a sign bit) is that we can use the same circuit for addition and subtraction. See:
```
	-2	-->		14	-->		1110
+ -3	-->		+13	-->	  +	1101
-------------------------------
							11011 
11011 --> 27, but overflow	
Without overflow it 	is 1011 --> 11
And 11 is -5 in twos complement!!
Because we throw the overflow bit away, our addition is modulo  2^n.
```

## Week 3
### Sequential Logic
All the Boolean and arithmetic chips that we built in chapters 1 and 2 were combinational. Combinational chips compute functions that depend solely on combinations of their input values. They cannot maintain state. Since computers must be able to not only compute values but also store and recall values, they must be equipped with memory elements that can preserve data over time. These memory elements are built from sequential chips.
In order to build chips that “remember” information, we must first develop some standard means for representing the progression of time.

### The clock
In most computers, the passage of time is represented by a master clock that delivers a continuous train of alternating signals. The exact hardware implementation is usually based on an oscillator that alternates continuously between two phases labeled 0-1, low-high, tick-tock, etc. The elapsed time between the beginning of a “tick” and the end of the subsequent “tock” is called cycle. Using the hardware’s circuitry, the signal is simultaneously broadcasted to every sequential chip.

###  Combinatorial Logic vs. Sequential Logic 
Combinatorial: 	out[t] = function(in[t]) 			//output at time t only depends on input at time t
Sequential:		out[t] = function(in[t-1]) 		//output at time t depends on the last input at time t-1.
—> based on the input in the previous time step we compute the output 


### Flip flops
The most elementary sequential element in a computer is a flip flop. Here we will use data flip flops, DFF.
Input: Single Bit data input
Clock input: changes according to master clock signal
Output: Single Bit data output 
Therefore the DFF implements out[t] = in[t-1]
The DFF outputs the input from the previous time unit.

```
Time 		1	2	3	4	5	6
IN			1	0	0	1	0	0	
OUT		-	1	0	0	1	0
```


### 1-Bit Register
IN: in load
OUT: out
If load(t-1), then out(t)  =in(t-1)		//if load was 1 at t-1, then out(t) is the input at t-1
Else out(t) = out(t-1)				//output the old (unchanged state)
```
TIME		1	2	3	4	5	6
LOAD		1	0	0	1	1	0
IN			1	0	0	0	1	0
OUT		-	1	1	1	0	1
```

Mux(a=in,  b=out , sel=load, out=out1);
Bit (in=out1, out=out)