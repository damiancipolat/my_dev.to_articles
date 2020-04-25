It is very common languages ​​such as java and c # to create custom exceptions, to differentiate error situations from one another. In JS there is the error object and several other types, but they are for very limited uses.

For that reason there is also the possibility of creating our types of exceptions. To simulate that there is a typing for them. We can do this in a 100% OOP or functional way.

## **Using CLASS**:
We can create custom exceptions for our programs, in this case using OOP class in JS.

**Demostration code**
```js
class ValidationError extends Error {
  constructor(message) {
    super(message)
    this.name = 'VALIDATION_ERROR'
    this.message = message
  }
}

class PermissionError extends Error {
  constructor(message) {
    super(message)
    this.name = 'PERMISSION_ERROR'
    this.message = message
  }
}

class ExecutionError extends Error {
  constructor(message) {
    super(message)
    this.name = 'EXECUTION_ERROR'
    this.message = message
  }
}

module.exports = {
  ValidationError,
  PermissionError,
  DatabaseError
}

```

**How to use it?**
```js
function myThrow(input) {

   if (!input)
     throw new ExecutionError('A execution error');

   return input
}

```

## **Using FUNCTIONS**:
We can create custom exceptions using a functional programming style.

**Demostration code**
```js
const ValidationError = (message)=>({
  error: new Error(message),
  code: 'VALIDATION_ERROR'
});

const PermissionError = (message)=>({
  error: new Error(message),
  code: 'PERMISSION_ERROR'
});

const ExecutionError = (message)=>({
  error: new Error(message),
  code: 'EXECUTION_ERROR'
});

```

**Example**
```js
const {
  ValidationError,
  PermissionError,
  DatabaseError
} = require('./exceptions.js');

function myThrow(input) {

   if (!input)
     throw ExecutionError('A execution error');

   return input
}
```

Extending the exception methods: 
**"Serializing Error objects"**, I use function to can use the constructor later.

```js
//Define exceptions.
function MyError(message){
  const internal = {
    error: new Error(message),
    code: 'MY_CUSTOM_ERROR'
  };

  return {
    ...internal,
    toJSON:()=>({
      code: internal.code,
      stack: internal.error.stack,
      message
    })
  }
}

MyError.prototype = Object.create(Error.prototype);
```

**Example:**
```js
//Launch
throw new MyError('So bad configuration');

//Capturing.
try{

  //.....
  throw new MyError('So bad configuration');  
  //.....

} catch(err){
  console.log('Error',err.toJSON());
}
```

## **Real life examples**:
Having read the previous examples, it is time to find real examples of exceptions that we could create. I am going to propose some that I have used in past projects.

### **HTTP error codes**
- Bad Request
- Unauthorized
- Not Found
- Internal Server Error
- Bad Gateway

```js
//Definif exceptions.
const BadRequest = ()=>({
  message: 'Bad Request',
  code:400
});

const Unauthorized = ()=>({
  message: 'Unauthorized',
  code:401
});

const NotFound = ()=>({
  message: 'Not Found',
  code: 404
});

const InternalServerError = ()=>({
  message: 'Internal Server Error',
  code: 500
});

const BadGateWay = ()=>({
  message: 'Bad Gateway',
  code: 502
});

//Define exceptions map.
const exceptionMap = {
  502: BadGateway,
  500: InternalServerError,
  404: NotFound,
  401: Unauthorized,
  400: BadRequest
};

//Using it.
const getUser = async(userId)=>{
  
  //Make request, this line is just an example, use a real rest client.
  const response = await fetch('http://user.mock/'+userId);

  //Get httpcode.
  const {
    status,
    body
  } = response;

  //Get the exception.
  const exception = exceptionMap[status];

  if (!exception)
    throw exception();
  else
    return body;
}
```

### **Bussiness operations**
- Loan rejected
- Loan exced
- Loan pending

```js
//We have this custom exceptions
const LoanRejected = (motive, clientId)=>({
  message: 'The loan was rejected',
  code:'LOAN_REJECTED',
  motive,
  clientId
});

const LoanExced = (clientId)=>({
  message: 'The loan ammount exced the limits',
  code:'LOAN_EXCED',
  clientId
});

const LoanPending = ()=>({
  message: 'The client has a loan pending for payment',
  code:'LOAN_PENDING'
});

//I simulate a loan process.
const processate = async(clientId,ammount)=>{
 
  const status = await getLoanStatus(clientId,ammount);

  //Detect status to reject the loan.
  if (status.code=='REJECTED')
    throw LoanRejected('Status not ready to calc',clienId);

  if (status.code=='UNAVAILABLE')
    throw LoanRejected('Clien banned',clienId);

  if (status.code=='EXCED')
    throw LoanExced();

  //If the client has debts.
  if (status.code=='PENDING')
    throw LoanPending();

  const loanId = await createLoan(clientId);

  return loanId;

}

//And we detect the type of exceptions.
const loanFlow = async (clientId,ammount)=>{
  
  try{

    const loanId = procesate(clientId,ammount);
    console.log('New loan create',loanId);

  } catch(err){

    if (err.code.includes['LOAN_REJECTED','LOAN_EXCED','LOAN_PENDING'])
      console.log('Loan rejected!!');
    else
      console.log('Problem in process try again later...');

  }

}
```