---
title: compliers
date: 2022-07-08 15:16:53
tags:
- computer science
categories:
- learn in NJUST
---

# 第一章

SpeedCoding(1953，John Backus)：占用大量的内存



Fortran(1954-1957)：1958年时约50%的程序使用Fortran编写

1. **Lexical Analysis**: Lexical analysis divides program text into "words" or "tokens"
2. **Parsing**: diagraming sentences(The diagram is a tree)
3. **Semantic Analysis**: to catch inconsistencies
4. **Optimization**: automatically modify programs so that they run faster and use less memory
5. **Code Generation**: Produces assembly code(usually); A translation into another language

---

<!-- more -->

**为什么有很多种编程语言？**

> Application domains have distinctive/conflicting needs

科学计算(Fortran)：

1. 优秀的浮点计算
2. 优秀的阵列计算
3. 并行性



商业应用(SQL)：

1. 持续性
2. 报表生成工具
3. 并发性



系统编程(C/C++)：

1. 资源管理
2. 实时控制



**为什么会有新的编程语言？**

> Claim: Programmer training is the dominant cost for programming language

1. 广泛使用的语言很难去改变(Widespread language are slow to change)
2. 开始一种新的语言比较容易(easy to start a new language)
3. 语言会被用来填补空缺(languages adopted to fill a void)



**什么是好的编程语言？**

> There is no universally accepted metric for language design

---

# 第二章

Cool: Classroom Object Oriented Language, designed to be implementable in a short time.

* Abstraction
* Static typing
* Reuse(inheritance)
* Memory management
* And more...

```cool
class Main {
	main() : Int {
		1
	};
};

class Main {
	i : IO <- new IO;
	main() : IO {
		i.out_string("hello world!")
	};
};

class Main {
	main() : Object {
		(new IO).out_string("hello world!")
	};
};

class Main {
	main() : Object {
		(new IO).out_string((new IO).in_string().concat("\n"))
	};
};

class Main inherits A2I {
	main() : Object {
		(new IO).out_string(i2a(a2i((new IO).in_string())+1).concat("\n"))
	};
};

class Main inherits A2I {
	main() : Object {
		(new IO).out_string(i2a(fact(a2i((new IO).in_string()))).concat("\n"))
	};
	
	fact(i : Int) : Int {
		i+1
	};
};

class Main inherits A2I {
	main() : Object {
		(new IO).out_string(i2a(fact(a2i((new IO).in_string()))).concat("\n"))
	};
	
	fact(i : Int) : Int {
		if (i = 0) then 1 else i * fact(i-1) fi
	};
};

class Main inherits A2I {
	main() : Object {
		(new IO).out_string(i2a(fact(a2i((new IO).in_string()))).concat("\n"))
	};
	
	fact(i : Int) : Int {
		let fact: Int <- 1 in{
			while(not(i = 0)) loop
				{
					fact <- fact * i;
					i <- i - 1;
				}
			pool;
			fact;
		}
	};
};

class Main inherits IO {
	main() : Object {
		let hello : String <- "Hello ",
			world : String <- "World!",
			newline : String <- "\n"
		in
			out_string(hello.concat(world.concat(newline)))
	}
}


class List {
	item: String;
	next: List;
	
	init(i: String, n: List){
		{
			item <- i;
			next <- n;
			self;
		}
	};
	
	fatten(): String {
		if(isvoid next) then 
			item
		else
			item.conccat(next.flatten())
		fi
	};
};
class Main inherits IO {
	main() : Object {
		let hello : String <- "Hello ",
			world : String <- "World!",
			newline : String <- "\n",
			nil: List,
			list: List <- (new List).init(hello, (new List).init(world, (new List).init(newline, nil)))
		in
			out_string(list.flatten())
	}
}


class List inherits A2I {
	item: String;
	next: List;
	
	init(i: String, n: List){
		{
			item <- i;
			next <- n;
			self;
		}
	};
	
	fatten(): String {
		let String: String <- 
			case item of
				i: Int => i2a(i);
				s: String => s;
				o: Object => {abort();"";};
			esac
		in
			if(isvoid next) then 
				string
			else
				string.conccat(next.flatten())
			fi
	};
};
class Main inherits IO {
	main() : Object {
		let hello : String <- "Hello ",
			world : String <- "World!",
			i: Int <- 42,
			newline : String <- "\n",
			nil: List,
			list: List <- (new List).init(hello, (new List).init(i, (new List).init(newline, nil)))
		in
			out_string(list.flatten())
	}
}
```

# 第三章

**Token class**:

* Identifier: 由字母或数字组成的字符串，以字母为首字符
* Integer: 非空的数字字符串
* Keyword: 关键字
* Whitespace: 一个由blanks,newlines,tabs组成的非空集合



* Classify program substrings according to role

* Communicate tolens to the parser

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/1.png)

---

example:

```
if(i==j)
	z=0;
else
	z=1
	
\tif(i==j)\n\t\tz=0;\n\telse\n\t\tz=1;
```

* whitespace: \t	\n\t\t	\n\t	\n\t\t
* kewords: if	else
* Identifiers: i	j	z	z
* numbers: 0	1
* operator: ==	=	=
* (	)	;

---

An implementation must do two things

1. Recognize substrings cooresponding to tokens
2. Identify the tolen class of each lexeme

$$
<token\quad class,\quad lexeme>\quad is\quad token
$$

---

LA Examples:

Fortran rule: whitespace是不重要的

```
DO 5 I = 1,25
DO 5 I = 1.25
```

由于whitespace被忽略，因此产生分歧，第一个式子为循环，第二个式子为将1.25赋值给DO5I，但是在逗号与点之前具有相同的字符串序列，直至逗号和点才能得出结果。这就需要**lookhead**。

1. The goal is to partition the string. This is implemented by reading left-to-right, recognizing one token at a time.
2. "Lookhead" may be required to decide where one token ends and the next token begins.



PL/I keywords are not reserved

```
IF ELSE THEN THEN = ELSE;ELSE ELSE = THEN
```



* The goal of lexical analysis is to
  * Partion the input string into lexemes
  * Identify the token of each lexeme
* Left-to-right scan => lookahead sometimes required

---

**Regular languages**

*正则表达式(regular expression)*

* Single character

  'c' = {"c"}

* epsilon

  $\epsilon$ = {""}

* Union

  A + B ={a|a$\in$A}$\bigcup${b|b$\in$B}

* Concatenation

  AB = {ab|a$\in$A $\bigwedge$ b$\in$B}

* Iteration

  $A^*$=$\underset{i\geq0}{\bigcup}A^i$

> Def.The regular expressions over $\Sigma$ are the smallest set of expression including

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/2.png)

* Regular expressions specify regular languages
* Five constructs
  * Two base cases: empty and 1-character strings
  * Three compound expressions: union, concatenation, iteration

---

**Formal Languages**

> Def.Let $\Sigma$ be a set of characters.A language over $\Sigma$ is a set of strings of characters drawn from $\Sigma$

Meaning function *L* maps syntax to senmantics
$$
L:\quad expression\rightarrow Set\quad of\quad Strings
$$
![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/3.png)

**为什么使用meaning function？**

- Makes clear what is syntax, what is semantics.
- Allows us to consider notation as a separate issue
- Because expressions and meanings are not one to one correspondings.

 

> Meaning is many to one(**Never one to many!**)

---

**Lexical Specifications**

关键字："if" or "else" or "then" or ...

'i' 'f' + 'e' 'l' 's' 'e'

"if" + "else"



整数：一个非空的数字字符串

digit = 'o'+'1'+'2'+'3'+'4'+'5'+'6'+'7'+'8'+'9'
$$
digit(digit^*)=digit^+
$$


标识符：以字母开头的字母或者数字串

letter = [a-z A-Z]
$$
letter(letter+digit)^*
$$


Whitespace：a non-empty sequence of blanks, newlines, and tabs
$$
('\ ' + '\backslash' + '\backslash+')^+
$$

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/5.png)

Example:

*anyone@cs.stanford.edu*

$letter^+$'@'$letter^+$'.'$letter^+$'.'$letter^+$

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/4.png)

* Regular expressions describe many useful languages
* Regular languages are a language specification
  * We still need an implementation

---

# 第四章

1. Write a rexp for the lexemes of each token class
   * Number = $digit^+$
   * Keyword = 'if' + 'else' + ...
   * Identifier = $letter(letter + digit)^*$
   * OpenPar = '('
   * ...

2. Construct *R*, matching all lexemes for all tokens

3. Let input be $x_1...x_n$

   For $1\leq i\leq n$ check $x_1...x_i\in L(R)$

4. If success, then we know that $_1...x_i\in L(R_j)$ for some *j*

5. Remove $X_1...x_i$ from input adn go to (3)



**How much input is used?**

当面对不同长度的前缀时，通常选择更长的前缀（它被称为*Maximal Munch*）



**Which token is used?**

按照优先级排序，选择排序在第一个的token

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/6.png)



**What if no rules matches?**

编写一个新的正则表达式，对于所有可能出现的错误字符串都将作为无效字符串出现，并且放在优先级的最后面

> *ERROR=[all strings not in the lexical spec]*, put it last in priority.



* Regular expressions are a concise notation for string patterns
* use in lexical analyisis requires small extensions
  * To resolve ambiguities
    1. matches as long as possible
    2. highest priority match
  * to handle errors
* Good algorithms known
  * Require only single pass over the input
  * Few operations per character(table lookup)

---

A finite automation(*有限自动机*) consists of

* An input alphabet $\Sigma$
* A set of states $S$
* A start state $n$
* A set of accepting states $F\subseteq S$
* A set of transitions $state\rightarrow ^{input}state$

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/7.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/8.png)

> Language of a FA = set of accepted strings

* Another kind of transition: $\epsilon-moves$ 机器可以在不消耗任何输入的情况下进行状态转换



* Deterministic Finite Automate(DFA)
  * One transition per input per state
  * No $\epsilon-moves$

* Bibdeterministic Finite Automate(NFA)
  * Can have multiple transitions for one input in a given state
  * Can have $\epsilon-moves$

> A DFA takes only one path through the state graph
>
> An NFA accepts if some choices lead to an accepting state



* NFAs and DFAs recognize the same set of languages
  * regualr languges
* DFAs are faster to execute
  * There are no choices to consider

* NFAs are, in general, smaller

---

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/9.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/10.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/11.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/12.png)

---

**NFA to DFA**

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/13.png)
$$
\epsilon-closure(B)=\{B,C,D\}\\\epsilon-closure(G)=\{A,B,C,D,G,H,I\}
$$


**一共有多少种不同的状态？**

若有N个状态，则存在$2^N-1$个非空子集(finite set of possible configurations)



NFA

* states $S$
* start state $s\in S$
* final state $F\in S$
* 转移函数：$a(x)=\{y|x\in X_n\quad x\stackrel{a}{\longrightarrow}1\}$
* $\epsilon-closure()$

DFA

* states: subsets of $S$(except the empty set)
* start state: $\epsilon-closure(s)$
* final state: $\{x|x\cap F\neq\varphi\}$
* $X\stackrel{a}{\longrightarrow}Y$ if $Y=\epsilon-closure(a(X))$

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/14.png)

---

**Implementing FA**

* A DFA can be implemented by a 2D table T
  * One dimension is *states*
  * Other dimension is *input symbol*
  * For every transition $S_i\rightarrow ^{a}S_k$ define $T[i,a]=k$

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/15.png)

缺点：表中有很多重复的行

---

这个表将包含一个指向特定状态的vector of moves的指针

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/16.png)

缺点：these pointers actually take time to your reference

---

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/17.png)

节省了大量的表格空间，但是运行速度会比DFA慢得多

---

* NFA $\rightarrow$ DFA conversion is key
* Tools trade between speed and space

| DFAs | faster, less compact |
| :--: | :------------------: |
| NFAs |   slower, concise    |

---

# 第五章

> 由于正则表达式无法表示所有的语言，我们需要Parser



* *Input*: sequence of tokens from lexer
* *Output*: parse tree of the program

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/18.png)

| Phase | Input                | Output                      |
| :---: | -------------------- | --------------------------- |
| Lexer | String of characters | String of tokens            |
| Parse | String of tokens     | Parse tree(may be implicit) |

---

**Context-Free Grammars**

* A CFG consists of
  * A set of terminals *T*
  * A set of non-terminals *N*
  * A start symbol *s* ($s\in N$)
  * A set of productions $X\rightarrow Y_1..Y_n$, $x\in N$ , $Y_i\in N\cup T\cup\{\epsilon\}$



1. Begin with a string with only the start sysmbol *S*
2. Replace any non-terminal *X* in the string by the right-hand side of some production $X\rightarrow Y_1...Y_n$
3. Repeat (2) until there are no non-terminals


$$
S\rightarrow...\rightarrow\alpha_0\rightarrow\alpha_1\rightarrow\alpha_2\rightarrow...\rightarrow\alpha_n\\\alpha_0\stackrel{*}{\longrightarrow}\alpha_n\quad(in\geq0\ steps)
$$
![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/19.png)

The idea of a CFG is a big step. But:

* Membership in a lanugage is "yes" or "no"; alse need parse tree of the input
* Must handle errors gracefully
* Need an implementations of CFG's (e.h., bison)

---

**Derivations**

A derivcation can be drawn as a tree

* Start symbol is the tree's root
* For a production $X\rightarrow Y_1...Y_n$ add children $Y_1...Y_n$ to node $X$

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/20.png)

 

* 一个解析树有
  * 叶子节点为终结符
  * 内部节点为非终结符
* An in-order traversal of the leaves is the original input
* 解析树展示了操作符之间的关联，但是输入字符串并不会



* 上图的案例为最左推导(*left-most derivation*)：对于每一步，替换最左侧的非终结符
* There is an quivalent notion of a right-most derivation

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/21.png)

> 最右推导和最左推导有相同的解析树

---

> 如果对于一个字符串有多个解析树，那么这个语法就是*ambiguous*

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/22.png)

* Instead of rewriting the grammar
  * Use the more natural(ambiguous) grammar
  * Along with disambiguating declarations
* Most tools allow precedence and associativity declarations to disambiguate grammars

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/23.png)

---

# 第六章

许多种类的可能错误

| Error kind  |        Example        | Detected by... |
| :---------: | :-------------------: | :------------: |
|   Lexical   |        ...$...        |     Lexer      |
|   Syntax    |       ...x*%...       |     Parser     |
|  Semantic   | ...int x;y = x(3);... |  Type checker  |
| Correctness | your favorite program |  Tester/User   |

* 错误处理应该
  * 准确并且清晰地报错错误
  * 快速地从错误中恢复
  * 不会减慢有效代码的编译



* Panic mode
  * When an error is dected:
    * Discard tokens until one with a clear role is found
    * Continue from there
  * Looking for *synchronizing* tokens
    * typically the statementt or expression terminators
* Error productions
* Automatic local or global correction



![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/24.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/25.png"   >

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/26.png"   >

---

**Abstract Syntax Trees**

---

**Recursive Descent**

* Define boolean functions that check for a match of:
  * A given token terminal:`bool term(TOKEN tok){return *next++ == tok;}`
  * The nth production of S:`bool Sn(){...}`
  * Try all productions of S:`bool S(){...}`



* For production $E\rightarrow T$

  * `bool E1() {return T();}`

* Fo production $E\rightarrow T+E$

  * `bool E2() {return T() && term(PLUS) && E();}`

* For all productions of E(with backtracking)

  * ```
    bool E() {
    
    	Token *save = next;
    
    	return (next = save, E1()) || (next = save, E2());
    
    }
    ```

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/27.png)

---

**Left Recursion**

对于产生式$S\rightarrow Sa$

* `bool S1() {return S() && term(a);}`
* `bool S() {return S1();}`

`S()`将会进入无限循环

* A left-recursive grammar has a non-terminal S $S\rightarrow ^{+}S\alpha$ for some $\alpha$



对于left-recursive grammar$S\rightarrow S\alpha | \beta$，$S$生成的所有字符串以$\beta$为首，并且跟随任意数量的$\alpha$

* 可以使用right-recursion重写
  * $S\rightarrow\beta S'$
  * $S'\rightarrow\alpha S'|\epsilon$



* In general	$S\rightarrow S\alpha_1|...|S\alpha_n|\beta_1|...|\beta_m$
* Rewrite as
  * $S\rightarrow\beta_1S'|...|\beta_mS'$
  * $S'\rightarrow\alpha_1S'|...|\alpha_nS'|\epsilon$



* Recursive descent
  * Simple and general parsing stratrgy
  * Left-recursion must be eliminatied first
  * ... but that can be done automatically

---

# 第七章

**Predictive Parsers**

* Parser能够预测使用哪一条产生式
  * By looking at the next few tokens
  * No backtracking
* Predictive parsers accept LL(k) grammars(*left-to-right left-most derivation(k tokens lookahead)*)



$E\rightarrow T+E|T$

$T\rightarrow int|int*T|(E)$

改写为

$E\rightarrow TX$
$X\rightarrow+E|\epsilon$

$T\rightarrow intT|(E)$

$Y\rightarrow*T|\epsilon$

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/28.png)

* 方法类似于recursive descent，除了
  * For the leftmost non-terminal *S*
  * We look a the next input token *a*
  * And choose the production shown at *[S,a]*

* A stack records fromtier of parse tree
  * Non-terminals that have yet to be expanded
  * Terminals that have yet to matched against the input
  * Top of stack = leftmost pending terminal or non-terminal

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/29.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/30.png)

---

**First Sets**

* Consider non-termianl *A*, production$A\rightarrow\alpha$, token *t*
* $T[A,t]=\alpha$ in two cases:
  * if $\alpha\rightarrow^{*}t\beta$
    * $\alpha$ can derive a *t* in the first position, We say that $t\in First(\alpha)$
  * if$A\rightarrow\alpha$ and $\alpha\rightarrow^{*}\epsilon$ and $S\rightarrow^{*}\beta At\delta$
    * we say $t\in Follow(A)$



*Definition*
$$
First(X)=\{t|X\rightarrow^{*}t\alpha\}\cup\{\epsilon|X\rightarrow^{*}\epsilon\}
$$
*Algotithm sketch*

1. $First(t)=\{t\}$
2. $\epsilon\in First(X)$
   * if $X\rightarrow\epsilon$
   * if $X\rightarrow A_1...A_n$ and $\epsilon\in First(A_i)$ for $1\leq i\leq n$
3. $First(\alpha)\subseteq First(X)$ if $X\rightarrow A_1...A_n\alpha$
   * and $\epsilon\in First(A_i)$ for $1\leq i\leq n$

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/31.png)

---

**Follow Sets**

*Definition*
$$
Follow(X)=\{t|S\rightarrow^{*}\beta Xt\delta\}
$$
*Intuition*

* if $X\rightarrow AB$ then $First(B)\subseteq Follow(A)$ and $Follow(X)\subseteq Follow(B)$
  * if $B\rightarrow^{*}\epsilon$ then $Follow(X)\subseteq Follow(A)$
* If *S* is the start symbol then $\$\in Follow(S)$

*Algorithm sketch*

1. $\$\in Follow(S)$
2. $Firset(\beta)-\{\epsilon\}\subseteq Follow(X)$
   * For each porduction $A\rightarrow\alpha X\beta$
3. $Follow(A)\subseteq Follow(X)$
   * For each production $A\rightarrow\alpha X\beta$ where $\epsilon\in First(\beta)$

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/32.png)

---

**LL(1) Parsing Tables**

For each production $A\rightarrow\alpha$ in CFG *G* do:

* for each terminal $t\in First(\alpha)$ do $T[A,t]=\alpha$
* if $\epsilon\in First(\alpha)$, for each $t\in Follow(A)$ do $T[A,t]=\alpha$
* if $\epsilon\in First(\alpha)$ and $\$\in Follow(A)$ do $T[A,\$]=\alpha$

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/33.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/34.png)

* If any entry is multiply defined then G is not LL(1)

---

**Bottom-Up Parsing**

Bottom-up parsing reduces a string to the start symbol by inverting productions

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/35.png)

> A bottom-up parser traces a rightmost derivation in reverse

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/36.png)

---

**Shift-Reduce Parsing**

* Shift(移进): Move *|* one place to the right, Shift a terminal to the left string
  * $ABC|xyz\Longrightarrow ABCx|yz$

* Reduce(归约): Apply an inverse production at the right end of the left string
  * If $A\rightarrow xy$ is a production, then $Cbxy|ijk\Longrightarrow CbA|ijk$



* Left string can be implemented by a stack: Top of the stack is the *|*
* Shift pushes a terminal on the stack
* Reduce
  * pops symbols off of the stack(production rhs)
  * pushed a non-terminal on the stack(production lhs)



* 如果移进或者归约都合法，将会存在*shift-reduce conflict*(移进-归约冲突)
* 如果可以使用两个不同的产生式进行归约，将会存在*reduce-reduce conflict*(归约-归约冲突)

---

# 第八章

**Handles**

Intuition: Want to reduce only if the result can still be reduced to the start symbol

* Assume a rightmost derivation: $S\rightarrow^{*}\alpha X\omega\rightarrow\alpha\beta\omega$
  * Then $\alpha\beta$ is a handle of $\alpha\beta\omega$

> A handle is a reduction that also allows further reductions back to the start symbol

> In shift-reduce parsing, handles appear only at the top of the stack, never inside

---

**Recognizing Handles**

$\alpha$ is a viable prefix if there is an $\omega$ such that $\alpha|\omega$ is a state of a shift-reduce parser

PS:$\alpha$ is stack and $\omega$ is rest of input;

> For any grammar, the set of viable prefixes is a regular language



* An item is a production with a "." somewhere on the rhs
* Items are often called "LR(0) items"



Example:

$E\rightarrow T+E|T$

$T\rightarrow int*T|int|(E)$

* Then $(E|)$ is a state of a shift-reduce parse
* $(E$ is a prefix of the rhs of $T\rightarrow(E)$, will be reduced after the next shift
* Item $T\rightarrow(E.)$ says that so far we have seen $(E$ of this production and hope to see $)$



![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/37.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/38.png)

To recognize viable prefixes, we must

* Recognize a sequence of partial rhs's of productions, where
* Each partial rhs can eventually reduce to part of the missing suffix of its prdecessor

---

**Recognizing Viable Prefix**

1. Add a dummy production $S'\rightarrow S$ to *G*
2. The NFA states are the items of *G*
3. For item $E\rightarrow\alpha.X\beta$ add transition, $E\rightarrow\alpha.X\beta\rightarrow\alpha X.\beta$
4. For item $E\rightarrow\alpha. X\beta$ and production $X\rightarrow\gamma$ add $E\rightarrow\alpha .X\beta\rightarrow.\gamma$
5. Every state is an accepting state
6. Start state is $S'\rightarrow S$

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/39.png)

---

**Valid Items**

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/40.png)

---

**SLR Parsing**

* LR(0) Parsing: Assume
  * stack contains $\alpha$
  * next input is *t*
  * DFA on input $\alpha$ terminates in state *s*
* Reduce by $X\rightarrow\beta$ if *s* contains item $X\rightarrow\beta$
* Shift if
  * *s* contains item $X\rightarrow\beta .t\omega$
  * equivalent to saying *s* has a transition labeled *t*



*Assume*

* stack contains $\alpha$
* next input is *t*
* DFA on input $\alpha$ terminates in state *s*



* Reduce by $X\rightarrow\beta$ if
  * *s* contains item $X\rightarrow\beta$
  * $t\in Follow(X)$
* Shift if
  * *s* contains item $X\rightarrow\beta.t\omega$

    

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/41.png)

* If there is a conflict in the last step, grammar is not SLR(k), k is the amount of lookahead.

---

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/42.png)

---

**SLLR improvement**

* Define *goto[i,A]=j* jf $state_i\rightarrow^{A}state_j$
* *goto* is just the transition function of the DFA



* Shift *x*
  * Push *<a,x>* on the stack
  * *a* is current input
  * *x* is a DFA state
* Reduce $X\rightarrow\alpha$
  * As before
* Accept
* Error

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/43.png)

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/44.png)

* LR(1) is more powerful
  * Build lookahead into the items
  * An LR(1) item is a pair: LR(0) item x lookahead
  * $[T\rightarrow .int*T,\$]$ means
    * After seeing $T\rightarrow int*T$ reduce if lookahead is $\$$
  * More accurate than just using follow sets
  * Take a look at the LR(1) automaton for your parser

---

**SLR Examples**

![](https://raw.githubusercontent.com/MaxKev1n/Pictures/main/Compilers/45.png)

**因此，该语法不是SLR(1)**

---