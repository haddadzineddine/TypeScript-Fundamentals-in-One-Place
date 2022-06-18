# TypeScript Fundamentals in One Place

## 1- Introduction

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

## 2- Built-in Types

as we know `JavaScript` has built-in types like :

- number
- string
- boolean
- array
- object
- undefined
- null

So `TypeScript` extend this list and introduce some new built-in types such as :

- any
- unknown
- never
- enum
- tuple

`1- The Any Type :` when you declare a variable and don't initialize it , the typescript compiler will assume that variable is type of `any` which means you can assign any type of data into it , here is an example :

```typescript
let anyType; // let anyType: any

anyType = 12;

console.log(typeof anyType); // output: number

anyType = "Random string";

console.log(typeof anyType); // output: string
```

`Note :` To declare `variables, functions` you just need to follow this syntax :

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

`11- Nullish Coalescing Operator :`

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

`13- The Unknown Type :`

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

`13- The Never Type :`

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

## 3- Object-oriented Programming

As we know `JavaScript` does'nt have the concept of `classes` like other programming languages such as ( PHP, Java, C++, C# ... ).

with `ES6` you can defined `classes` but it's just a syntactic sugar for creating `constructor function` and `prototypal inheritance`.

Let's see `OOP` in `TypeScript` :

`1- Creating Classes and objects :`

```typescript
class Account {
  id: number;
  owner: string;
  balance: number;

  constructor(id: number, owner: string, balance: number) {
    this.id = id;
    this.owner = owner;
    this.balance = balance;
  }

  deposit(amount: number): void {
    if (amount > 0) {
      this.balance += amount;
    }

    throw new Error("Invalid amount");
  }
}

let account = new Account(1, "zineddine", 100);

account.deposit(100);

console.log(typeof account); // object
console.log(account instanceof Account); // true

/*

always make sure to use instanceof property to check if 
an object is an instance of a class

*/
```

`Note :` You can't use the `function` keyword inside a `class` to declare a `function`, use it only when you declare a `stand-alone function`.

`2- Read-only and Optional Properties :`

```typescript
class User {
  readonly id: number;
  name: string;
  email: string;
  nickname?: string; // optional property

  constructor(id: number, name: string, email: string) {
    this.id = id;
    this.name = name;
    this.email = email;
  }
}

let user = new User(1, "zineddine", "hz_haddad@esi.dz");
user.id = 12; // Cannot assign to 'id' because it is a read-only property
```

`3- Access Control Keywords :`

```typescript
class Account {
  /*
    public # by default
    private
    protected
  */

  id: number;
  private _balance: number;

  constructor(id: number, balance: number) {
    this.id = id;
    this._balance = balance;
  }

  deposit(amount: number): void {
    if (amount > 0) {
      // assume we want also to log the transaction
      this._balance += amount;
    }

    throw new Error("Invalid amount");
  }

  private calculateTax(amount: number): number {
    return amount * 0.1;
  }

  getBalance(): number {
    return this._balance;
  }
}

let account = new Account(1, 100);
account._balance -= 50; // Property '_balance' is private and only accessible within class 'Account'
```

`4- Parameter Properties and Getters & Setters :`

```typescript
class Account {
  nickname?: string; // optional property

  constructor(
    public readonly id: number,
    public owner: string,
    private _balance: number
  ) {}

  get balance(): number {
    return this._balance;
  }

  set balance(value: number) {
    if (value < 0) {
      throw new Error("Balance cannot be negative");
    }
    this._balance = value;
  }
}

let account = new Account(1, "zineddine", 100);

console.log(account.balance); // 100
account.balance = -100; // throws error
account.balance = 100; // OK
```

`5- Index Signatures :` Index Signatures are just a fancy name for `dynamic properties`

```typescript
class NameByNumber {
  // index signature property
  [name: string]: number;
}

let nameByNumber = new NameByNumber();

nameByNumber.John = 1;
// nameByNumber.['John'] = 1;
// nameByNumber.John = '1'; Type 'string' is not assignable to type 'number'
nameByNumber.Jane = 2;
nameByNumber.Bob = 3;

console.log(nameByNumber.John); // 1
```

`6- Static Members :`

```typescript
class Ride {
  private static _activeRides: number = 0;

  start() {
    Ride._activeRides++;
  }

  end() {
    Ride._activeRides--;
  }

  static get activeRides() {
    return Ride._activeRides;
  }
}

let ride1 = new Ride();
let ride2 = new Ride();

ride1.start();
ride2.start();

console.log(Ride.activeRides); // 2
```

`7- Inheritance and Methods Overriding :`

```typescript
class Person {
  constructor(public firstName: string, public lastName: string) {}

  get fullName() {
    return this.firstName + " " + this.lastName;
  }

  walk() {
    console.log("Walking");
  }
}

class Student extends Person {
  constructor(firstName: string, lastName: string, public id: number) {
    super(firstName, lastName);
  }

  override walk() {
    super.walk();
    console.log("Walking on the stairs");
  }

  override get fullName() {
    return "Student : " + super.fullName;
  }
}

let student = new Student("John", "Doe", 123);

console.log(student.fullName);
student.walk();

/*

  Walking
  Walking on the stairs

*/

console.log(student instanceof Person); // true
```

`8- Polymorphism :`

```typescript
// parent class , base class , super class
class Person {
  protected steps: number = 0;

  constructor(public firstName: string, public lastName: string) {}

  get fullName() {
    return this.firstName + " " + this.lastName;
  }
}

// child class , sub class , derived class
class Student extends Person {
  constructor(firstName: string, lastName: string, public id: number) {
    super(firstName, lastName);
  }

  override get fullName() {
    return "Student : " + super.fullName;
  }
}

class Teacher extends Person {
  constructor(firstName: string, lastName: string, public id: number) {
    super(firstName, lastName);
  }

  override get fullName() {
    return "Teacher : " + super.fullName;
  }
}

function printName(persons: Person[]) {
  for (let person of persons) {
    console.log(person.fullName);
  }
}

printName([
  new Person("John", "Doe"),
  new Student("Jane", "Doe", 123),
  new Teacher("John", "Doe", 123),
]);

/*

John Doe
Student : Jane Doe
Teacher : John Doe

*/
```

`9- Abstract Classes :`

```typescript
abstract class Shape {
  constructor(public color: string) {}

  abstract render(): void;
}

class Circle extends Shape {
  constructor(public radius: number, color: string) {
    super(color);
  }

  override render(): void {
    console.log("Circle");
  }
}

let shape = new Shape("red"); // Cannot create an instance of an abstract class
```

`10- Interfaces :`

```typescript
interface Calender {
  name: string;
  addEvent(event: string): void;
  removeEvent(event: string): void;
}

interface CloudCalender extends Calender {
  sync(): void;
}

class GoogleCalender implements CloudCalendar {
  constructor(public name: string) {}

  addEvent(event: string): void {
    console.log(`Adding ${event} to GoogleCalendar`);
  }
  removeEvent(event: string): void {
    console.log(`Removing ${event} from GoogleCalendar`);
  }
  sync(): void {
    console.log("Syncing GoogleCalendar");
  }
}
```

`Note :` In `TypeScript`, `interfaces` and `type aliases` can be used interchangeably.
Both can be used to describe the shape of an object

```typescript
interface Person {
  name: string;
}

let person: Person = {
  name: "Zineddine",
};

type User = {
  name: string;
};

let user: User = {
  name: "Zineddine",
};
```

## 4- Generics

`1- Generic Classes and The Keyof Operator :`

```typescript
class KeyValuePair<K, V> {
  constructor(public key: K, public value: V) {}
}

let kvp = new KeyValuePair<string, number>("name", 10);
/*

you can also use this syntax :
  let kvp = new KeyValuePair('name', 10); 
  the compiler will refer the type of the key and value for us

*/
```

`2- Generic Functions :`

```typescript
class ArrayUtils {
  static wrapInArray<T>(value: T) {
    return Array.isArray(value) ? value : [value];
  }
}

let numbers = ArrayUtils.wrapInArray(1);
let strings = ArrayUtils.wrapInArray("hello");

console.log(numbers); // [1]
console.log(strings); // ["hello"]
```

`3- Generic Interfaces :`

```typescript
interface Result<T> {
  data: T | null;
  error: string | null;
}

interface User {
  userName: string;
}

interface Product {
  productName: string;
}

function fetch<T>(url: string): Result<T> {
  return {
    data: null,
    error: null,
  };
}

fetch<User>("url").data?.userName;
fetch<Product>("url").data?.productName;
```

`4- Generic Constraints :`

```typescript
class Person {
  constructor(public name: string) {}
}

class Student extends Person {}

function echo<T extends Person>(arg: T): T {
  return arg;
}

echo(new Person("John"));
echo(new Student("Zineddine"));
echo(10); // Argument of type 'number' is not assignable to parameter of type 'Person'
```

`5- Extending Generic Classes :`

```typescript
class Product {
  constructor(public name: string, public price: number) {}
}

class Store<T> {
  protected _objects: T[] = [];

  addObject(object: T) {
    this._objects.push(object);
  }

  find(property: keyof T, value: unknown): T[] {
    return this._objects.filter((o) => o[property] === value);
  }
}

// pass on the generic type parameter
class CompressibleStore<T> extends Store<T> {
  compress() {
    this._objects.forEach((object) => {
      console.log(object);
    });
  }
}

let compressibleStore = new CompressibleStore<Product>();
compressibleStore.addObject(new Product("Product 1", 100));

compressibleStore.compress(); // Product { name: 'Product 1', price: 100 }

// Restrictions the generic type parameter
class SearchableStore<T extends { name: string }> extends Store<T> {
  search(searchTerm: string) {
    this._objects.find((object) => {
      return object.name === searchTerm;
    });
  }
}

// Fix the generic type parameter
class ProductStore extends Store<Product> {}

let store = new Store<Product>();

store.addObject(new Product("Product 1", 100));
store.addObject(new Product("Product 2", 200));

store.find("name", "Product 1"); // [Product { name: 'Product 1', price: 100 }]

store.find("name", "Product 3"); // []

store.find("nonExistingProperty", "Product 3"); // Argument of type '"nonExistingProperty"' is not assignable to parameter of type 'keyof Product'
```

`6- Type mapping :`

```typescript
interface Product {
  name: string;
  price: number;
}

type ReadOnly<T> = {
  readonly [K in keyof T]: T[K];
};

type Optional<T> = {
  [K in keyof T]?: T[K];
};

type Nullable<T> = {
  [K in keyof T]: T[K] | null;
};
```
