<img src="https://github.com/damiancipolat/Functional_programming_in_JS/blob/master/doc/fp.png?raw=true" width="125px" align="right" />

# Memoization
Is an optimization **technique** used primarily to **speed up computer** programs by storing the results of expensive function calls and returning the cached result when the same inputs occur again.

### Memoizing
Memoizing in simple terms means memorizing or storing in memory. A memoized function is usually faster because if the function is called subsequently with the previous value(s), then instead of executing the function, we would be fetching the result from the cache.

### Pure funtions
A function is a process which takes some input, called arguments, and produces some output called a return value.

### Is same as caching?
Yes, Memoization is actually a specific type of caching. While caching can refer in general to any storing technique (like HTTP caching) for future use, memoizing specifically involves caching the return values of a function.

### When to memoize your functions?
- Only pure functions.
- Api calls.
- Heavy computational functions.

### Memoize js code:
This memoize store the functions arguments in a internal cache, using bas64 to encode the parameters to create a hash. We are following the principle, that f(a) = b and alway when function is calle with a returns b.

```js
//Memoize function: high order, curryng.
const memoize = (fn) => {

  let cache = {};
  
  return (...args) => {
    
    //Create hash.
    const n = btoa(args);

    //Find in cache or store new values.
    if (n in cache)      
      return cache[n];
    else {    
      let result = fn(n);
      cache[n] = result;

      return result;
    }

  }

}

//Function to be stored.
const sum = (x,y) =>x+y;

//Wrapp a function.
const memoizeSum = memoize(add);

//Tests
console.log(memoizeSum(3,1));  // calculated
console.log(memoizeSum(3,1));  // cached
console.log(memoizeSum(4,4));  // calculated
console.log(memoizeSum(4,4));  // cached
```

### Memoize js + timeout:
The same memoize function, but using a timeout to expire the cache after the time finish.

```js
const memoizeTimeout = (fn,time) => {

  let cache = {};
  let timeId;

  return (...args) => {

      //Erase cache.
      timeId = setTimeOut(()=>{
        cache={};
        clearInterval(timeId);
      });

      //Create hash.
      const n = btoa(args);

      //Find in cache or store new values.      
      if (n in cache){        
        return cache[n];
      } else {    
        let result = fn(n);        
        cache[n] = result;

        return result;
      }

    },time);    

  }

}

//Function to be stored.
const sum = (x,y) =>x+y;

//Wrapp a function.
const memoizeSum = memoizeTimeout(sum,1000);

//Tests
console.log(memoizeSum(3,1));  // calculated
console.log(memoizeSum(3,1));  // cached
console.log(memoizeSum(4,4));  // calculated
console.log(memoizeSum(4,4));  // cached
```

## Resources:
- https://www.freecodecamp.org/news/understanding-memoize-in-javascript-51d07d19430e/
- https://codeburst.io/understanding-memoization-in-3-minutes-2e58daf33a19

Writted with 💖