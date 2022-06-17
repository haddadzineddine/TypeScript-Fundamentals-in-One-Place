# TypeScript Fundamentals in One Place

## 1- Introduction :

<img src="./images/typescript-diagram.png" width="300" />

Programming language divide into two categories :

- `Statically typed`
- `Dynamically typed`

in `Statically-typed` languages (C, Java, C#, ... ), the type of variable is set at the compile-time and cannot change later.

in `Dynamically-typed` languages (PHP, JavaScript, Python, ... ), the type of variable is determined at the run-time and can change later.

`TypeScript` is a programming language build on top of `JavaScript` ( essentially JavaScript with static typing and some additional features ) , so before we star make sure that you are familiar with this concepts in javascript:

- Variables
- Arrays
- Objects
- Functions
- Arrow Functions
- Destructuring
- ...

## 2- Fundamentals :

### Build-in Types :

as we know `JavaScript` has build-in types like :

- number
- string
- boolean
- array
- object
- undefined
- null

So `TypeScript` extend this list and introduce some new build-in types such as :

- any
- unknown
- never
- enum
- tuple

`1- The any type :` when you declare a variable and don't initialize it , the typescript compiler will assume that variable is type of `any` which means you can assign any type of data into it , here is an example :

```typescript
let anyType; // let anyType: any

anyType = 12;

console.log(typeof anyType); // output: number

anyType = "Random string";

console.log(typeof anyType); // output: string
```

`Note :` If you want to declare a variable of `number, string, ...,` or a function you just need to use this syntax :

```typescript
let numberType: number = 12;
let numberType: string = 12;

function taxe(income: number): number {
  return income * 0.2;
}
```

`2- Arrays :`

```typescript
let numbers = [1, 2, 3]; // let numbers: number[] = [1, 2, 3]

let anyTypes = []; // let anyTypes: any[]

anyType[0] = 100;
anyType[0] = "r_string";

let names = ["ahmed", "zineddine"]; // let names: string[] = ["ahmed", "zineddine"]
```

`3- Tuples :` A tuple is a typed array with a pre-defined length and types for each index.

```typescript
let employee: [number, string] = [1, "Steve"];
```

`4- Enums :`

```typescript
// const small = 1;
// const medium = 1;
// const large = 1;

const enum Size {
  Small = 1,
  medium,
  large,
}

let mySize: Size = Size.Small;

console.log(mySize); // output : 1
```

`4- Objects :`

```typescript
/*
let employee: {
  id:number,
  name:string
} = {
  id:1,
  name:'zineddine'
}
*/

let employee = {
  id: 1,
  name: "zineddine",
};

let user: {
  readonly id: number;
  name: string;
  pseudo?: string;
  retire: (date: Date) => void; // function declaration
} = {
  id: 1,
  name: "zineddine",
  retire: (date: Date) => {
    console.log(date);
  },
};

user.id = 10; // Cannot assign to 'id' because it is a read-only property
```

`5- Type Aliases :`

```typescript
type User = {
  readonly id: number;
  name: string;
  pseudo?: string;
  retire: (date: Date) => void; // function declaration
};

let user: User = {
  id: 1,
  name: "zineddine",
  retire: (date: Date) => {
    console.log(date);
  },
};
```

`6- Union Types :`

```typescript
function kgToLbs(kg: number | string): number {
  // Narrowing
  if (typeof kg === "string") {
    return parseFloat(kg) * 2.2046;
  }

  return kg * 2.2046;
}
```

`7- Intersection Types :`

```typescript
// make no sense right ?
let weight: number & string;

// let see a realistic example

type draggable = {
  drag: () => void;
};

type resizable = {
  resize: () => void;
};

let UIWidget: draggable & resizable;

UIWidget = {
  drag: () => {},
  resize: () => {},
};
```

`8- Literal Types :`

```typescript
// let quantity: 5 | 100;

type Quantity = 50 | 100;
let quantity: Quantity;

quantity = 5; // Type '5' is not assignable to type 'Quantity'

type Metric = "m" | "cm" | "mm";
```

`9- Nullable Types :`

```typescript
function greeting(name: string | null | undefined) {
  if (name) {
    return `Hello, ${name}`;
  }
  return "Hello, World";
}

greeting("John");
greeting(null);
greeting(undefined);
```

`10- Optional Chaining :`

```typescript
type User = {
  id: number;
  birthday?: Date;
};

function getUser(id: number): User | null | undefined {
  if (id === 1) {
    return {
      id,
      birthday: new Date("2000-01-01"),
    };
  }

  return null;
}

getUser(0); // output null

getUser(1); // output { id: 1, birthday: Date }

// optional property access operator
getUser(1)?.birthday?.getFullYear(); // output 2000

// optional element access operator

let employees: string[] | null = null;
employees?.[0];

// optional function call operator

let log: any = null;

log?.("hello"); // return undefined
```

`11- Nullish Coalescing operator :`

```typescript
let speed: number | null = null;

let ride = {
  // Falsy values ( false, 0, '', null, undefined )
  // speed: speed || 30, if speed is falsy, set it to 30 , but 0 is falsy
  // speed: speed != null ? speed : 30,
  speed: speed ?? 30,
};
```

`12- Type Assertions :`

```typescript
let phone = document.getElementById("phone");

phone.value; // Property 'value' does not exist on type 'HTMLElement'

// let email = <HTMLInputElement> document.getElementById('email');

let email = document.getElementById("email") as HTMLInputElement;

email.value;
```

`13- The unknown Type :`

```typescript
function render(document: any) {
  // no compile error , but runtime error
  document.whatEver();
}

function render(document: unknown) {
  /*
  compile error, now the compîler forces us to check the type of the argument before using it

  */

  document.whatEver();

  if (document instanceof String) {
    document.toLocaleLowerCase();
  }
}
```

`13- The never Type :`

```typescript
function reject(message: string): never {
  throw new Error(message);
}

function processEvent(): never {
  while (true) {
    // ...
  }
}

processEvent();

/*

 => this code will never be executed , but the compiler don't tell us , so we have to use the `never` type.

 => now the compiler will tell us that the function will never return : Unreachable code detected.

 */

console.log("Hello World!");
```
