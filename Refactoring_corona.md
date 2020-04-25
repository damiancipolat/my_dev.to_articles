## Refactoring excercise.
I'm going to make this article to show various ways of how we can refactor a validation, we will see step by step 
how we can simplify the expression until we see how we can reach scenarios that we do'nt need to use 
the if operator, the focus of the code is 100% functional .

##Code origin:

<img src="https://github.com/damiancipolat/handle_money_in_js/blob/master/doc/corona.png?raw=true" align="center" width="700px"/>

This code comes from the application of the Argentine government to make a corona virus self-diagnosis.
In LinkedIn part of its source code was filtered and went viral due to the excess of validations when performing the if.

The idea here is to use the code in the image, as example to explore the way of how to make a refactor.

**Note:**
I will show many combinations of doing this. Sure some will like more than others. It is for practical purposes, 
everyone is free to choose the one they prefer or to propose a new one.


### **Option 1 - Direct solution, agroup and split in and / OR.**
```js
if ((bodyTemperature >= 38)&&(difficultyBreathing||diabetes||cancer||isPregnant||isOver60yearsOld||hepatic||kidneyDisease||respiratoryDisease)){
	history.replace(`/diagnostico/${provincia}`);
} else if (hasFever){
	history.replace(`/cuarentena/`);
} else if (!hasFever){
	history.replace(`/diagnostico_bueno/`);
} else {
	history.replace(`/diagnostico_bueno/`);
}
```

### **Option 2 - split in two functions.**
We split the logic conditions into functions.

```js
const isRiskCondition = ()=> diabetes||
cancer||
isPregnant|
isOver60yearsOld||
hepatic||
kidneyDisease||
respiratoryDisease;

const hasFever = bodyTemperature >= 38;

//Check corona virus
if ((hasFever() && difficultyBreathing)||(hasFever() and isRiskCondition()) || (hasFever() && isRiskCondition() && difficultyBreathing)){
	history.replace(`/diagnostico/${provincia}`);
} else if (hasFever){
	history.replace(`/cuarentena/`);
} else if (!hasFever){
	history.replace(`/diagnostico_bueno/`);
} else {
	history.replace(`/diagnostico_bueno/`);
}
```

### **Option 3 - split in two functions and simplify logic.**
We can try to simplify this loginc following this rules.

**Rules**:
- fever and breatheBad
- fever and riskcondition
- fever and riskcondition and breatheBad

Simplificated expression: fever and (breatheBad or riskcondition)

```js
const isRiskCondition = ()=> diabetes||
cancer||
isPregnant||
isOver60yearsOld||
hepatic||
kidneyDisease||
respiratoryDisease;

const hasFever = bodyTemperature >= 38;

//Check corona virus
if (hasFever() && (difficultyBreathing || isRiskCondition())){
	history.replace(`/diagnostico/${provincia}`);
} else if (hasFever){
	history.replace(`/cuarentena/`);
} else if (!hasFever){
	history.replace(`/diagnostico_bueno/`);
} else {
	history.replace(`/diagnostico_bueno/`);
}
```

### **Option 4 - split into functions + create a corona detectiob function**
Simplify and encapsulate the corona virus detection in one function.

```js
const isRiskCondition = ()=> diabetes||
cancer||
isPregnant||
isOver60yearsOld||
hepatic||
kidneyDisease||
respiratoryDisease;

const hasFever = bodyTemperature >= 38;
const isPossitive = ()=> hasFever() && (difficultyBreathing || isRiskCondition());

if (isPossitive()) {
	history.replace(`/diagnostico/${provincia}`);
} else if (hasFever){
	history.replace(`/cuarentena/`);
} else {
	history.replace(`/diagnostico_bueno/`);
}
```

### **Option 5 - unificate risk conditions & avoid use else / else if.**
The idea here is show, how avoid use else / else if, and unificate the risk conditions in only one function.

```js
const isRiskCondition = ()=> difficultyBreathing||
diabetes||
cancer||
isPregnant||
isOver60yearsOld||
hepatic||
kidneyDisease||
respiratoryDisease;

const hasFever = bodyTemperature >= 38;
const isPossitive = ()=> hasFever && isRiskCondition;

if (isPossitive())
	history.replace(`/diagnostico/${provincia}`);

if (!isPossitive() && hasFever())
	history.replace(`/cuarentena/`);

if (!isPossitive() && !hasFever())
	history.replace(`/diagnostico_bueno/`);
```

### **Option 6 - Avoid use IF / else / else if.**
The idea here is do'nt use if-else-else if, so we will create a special function that detect the success logical condition
and return the parameter number, the purpose of it is to have a index to map a value in an array.

```js
const isRiskCondition = ()=> difficultyBreathing||
diabetes||
cancer||
isPregnant||
isOver60yearsOld||
hepatic||
kidneyDisease||
respiratoryDisease;

const hasFever = bodyTemperature >= 38;
const isPossitive = ()=> hasFever && isRiskCondition;

//Urls map.
const urls = [
	`/diagnostico/${provincia}`,
	`/cuarentena/`,
	`/diagnostico_bueno/`
];

//Create a inline boolean validator, that return a index.
const triple = (a,b,c)=> a?0:(b?1:(c?2:null));

//Get the url from the map.
const path = triple(isPossitive(),(!isPossitive() && hasFever()),(!isPossitive() && !hasFever()))||2;

//Redirect.
history.replace(path);
```

### **Option 7 - (if - else - else) if as function.**
The idea in this point to create a special function that make the (if-else-else if) as function, using the ternary operator.

```js
const isRiskCondition = ()=> difficultyBreathing|
 diabetes||
 cancer||
 isPregnant||
 isOver60yearsOld||
 hepatic||
 kidneyDisease||
 respiratoryDisease;

const hasFever = bodyTemperature >= 38;
const isPossitive = ()=> hasFever && isRiskCondition;

//Return a index to use in a map.
const elseIf = (a,b)=>a?0:(b?1:(!a&!b)?2:null);

//Urls map.
const urls = [
	`/diagnostico/${provincia}`,
	`/cuarentena/`,
	`/diagnostico_bueno/`
];

//Get the url from the map.
const path = elseIf(isPossitive(),(!isPossitive() && hasFever()))||2;

//Redirect.
history.replace(path);
```