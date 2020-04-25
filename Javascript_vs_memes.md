I am going to analyze several internet memes that make fun of javascript. It is a good opportunity to explain each one of them and that the results obtained are not language errors, but there are very specific and serious explanations behind it.

Is important to study first a concept called "type coercion", is about the conversion of
data types in different situations. Continue reading in this link:
https://2ality.com/2019/10/type-coercion.html#what-is-type-coercion%3F

## Thanks for JS - meme
Here there are a lot of concept to study.

<img src="https://github.com/damiancipolat/js_vs_memes/blob/master/doc/js_thanks.png?raw=true" align="center" width="400px"/>

#### 1) typeof NaN = "number"
Is true, in JS the NaN is a number and can't be compared with any other NaN.
Some read: https://developer.mozilla.org/es/docs/Web/JavaScript/Referencia/Objetos_globales/NaN
Example: NaN==NaN - false

#### 2) 9999999999999999 -> 10000000000000000
Javascript doesn't have integers, only 64-bit floats - and you've ran out of floating-point precision represented with the
constant Number.MAX_SAFE_INTEGER constant.

#### 3) 0.5+0.1==0.6 - true
Is a classic operation, 0.5+0.1 is realized and the result is 0.6 and the value is the same so is true.

#### 4) 0.1+0.2==0.3 - false
- 0.1+0.2 = 0.30000000000000004
- 0.30000000000000004 = 0.6 - false

#### 5) Math.max() = - Infinity
The result is “-Infinity” if no arguments are passed and the result is NaN if at least 
one of the arguments cannot be converted to a number.

#### 6) Math.min() = Infinity
The smallest of the given numbers. If any one or more of the parameters cannot be converted into a number, NaN is returned. The result is Infinity if no parameters are provided.

#### 7) []+[]=""
This happen, because the empty array is casted to "" so ""+"" is equal "".

#### 8) []+{}="[object object]"
The [] is in the left so is casted as string.
- []+{}
- ""+{}
- "[object object]""

#### 9) {}+[]=0
The {} is in the left, so the empty structure is casted to number.
- {} is casted to 0.
- [] is casted to 0.
- 0+0 = 0.

#### 10) true+true+true === 3 - true
true is casted to 1 when is used with the + operator, so is converted to 1+1+1.

#### 11) true-true = 0
true is casted to 1 for the minus operator, so is converted to 1-1.

#### 12) true==1 - true
1 is casted to boolean, Boolean(1) = true, so true==true.

#### 13) true===1 - false
This is beacuse the == operator don't make conversion so the boolean and number are'nt differnt data types.

#### 14) (!+[]+[]+![]).length = 9
Some thins to analize.
- []+[]="".
- ![] = false.
- (!+[]): true, ([]+![]): "false" as string.
- "truefalse" two string concatenated.
- "truefalse".length = 9

#### 15) 9+"1"
"91" the nine is casted to string, so is string+string concatenation = "91".

#### 15) 91-"1" = 90
The 1 is casted from string to number for the use of the operator minus "-". 91-1 = 90.

#### 16) []==0 - true
Array to number conversion, Number([]) is 0, so 0==0.

## Fav language JS - meme
Classic meme, here there are one concept.

<img src="https://github.com/damiancipolat/js_vs_memes/blob/master/doc/fav_meme.jpg?raw=true" align="center" width="400px"/>

#### 1) "11"+1 = "111"
Concatenation between string and number the, last is casted to string, easy.

#### 2) "11"-1 = 10
OK, in this case there a string and a number with the minus operator, the string is casted to number, and later
a normal arithmetic operation between two numbers.

- "11" - 1
- 11   - 1
- 10

## Patricio - meme
I will analyze this meme, has 3 points.

<img src="https://github.com/damiancipolat/js_vs_memes/blob/master/doc/patricio.png?raw=true" align="center" width="400px"/>

#### 1) 0 == "0" - true
Converts the variable values to the same type before performing comparison, for this reason the "0" is cast
from string to number and later compare 0 == 0 is the same.

#### 2) 0 == [] - true
hahaha this is very strange, but the explanation is the next:

- The left operand is number.
- The rigth operand is casted to array to number.
- Number([]) = false, the false is casted to number.
- Number(false) = 0.
- 0 == 0 is true.

#### 3) Here they assume that there is a transitivity between 1 and 2. 
The logic is if **0 == "0"** and **0 == [] then "0" = []**, the problem is that the "0" is a string casted to int when is compared,
for this reason the transitive property can't be applyed here.

#### 4) "0" == [] - false
Is correct because the [] is casted to string, String([])="" so "0" == "" is false.

## Mind explosion JS - meme
I will analyze this meme, has 4 points.

<img src="https://github.com/damiancipolat/js_vs_memes/blob/master/doc/mind_js.jpg?raw=true" align="center" width="300px"/>

#### 1) 2+2 = 4
A very normal arithmetic operation, nothing strange.

#### 2) "2"+"2"="22"
A string concatenation, happen when the "+" is used with STRING+STRING.

#### 3) 2+2-2 = 2
Another arithmetic operation, in this case all number are used.
- (2+2)-2
- 4-2
- 2

#### 4) "2"+"2"-"2" = 20
We mix **string concatenation** and **type coercion**.

- "2"+"2"  = "22 string concatenation
- "22"-"2" = 20
- Type coercion to int, caused by the minus operator.
- "22" - "2", cast "string-string" to "int-int".
- 22 - 2 = 20