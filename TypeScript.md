# TypeScript CheatSheet

## Contents
[Installation](#installation)  
[TSC (TypeScript Compiler) options](#tsc)  
[Basic Types](#basic)  
[Enum](#enum)  
[Objects](#objects)  
[Type Assertion](#type-assertion)  
[Functions](#functions)  
[Interfaces](#interfaces)  
[Classes](#classes)  

---

## Installation <a id="installation"></a>

```bash
npm i typescript
```

## TSC (TypeScript Compiler) options <a id="tsc"></a>

Compile the `index.ts` file to `index.js`:

```bash
tsc index
```

Watch `index.ts` file changes:

```bash
tsc --watch index
```

Set up a TSC configuration file:

```bash
tsc --init
```

## Basic types <a id="basic"></a>

```typescript
let num: number = 5;
let name: string = 'John';
let isRegistered: boolean = true;

// A variable can hold anything
let anyValue: any = 'Hello';
anyValue = 3;
```

An array of numbers:

```typescript
let nums: number[] = [1, 2, 3];
```

Tuple:

```typescript
let person: [number, string, boolean] = [1, 'John', true]
person = [2, 'Mary', 2] // invalid
```

Tuple array:

```typescript
let goods: [string, number][];
goods = [
  ['Ball', 3],
  ['Socks', 10],
  ['Trainers', 6]
]
```

Multiple types (union):

```typescript
// string or number
let id: string | number = 3;
id = '3';
```

## Enum <a id="enum"></a>

```typescript
/* In the following case, by default, Up === 0, Down === 1,
   Left === 2, Right === 3 */
enum Direction {
  Up,
  Down,
  Left,
  Right
}

console.log(Direction.Down) // 1

// We can start from 1 to 4 respectively
enum SecondDirection {
  Up = 1,
  Down,
  Left,
  Right
}

console.log(SecondDirection.Down) // 2

// It's also possible to use string values
enum ThirdDirection {
  Up = 'Up',
  Down = 'Down',
  Left = 'Left',
  Right = 'Right'
}

console.log(ThirdDirection.Down) // 'Down'
```

## Objects <a id="objects"></a>

Types inside object declaration:

```typescript
const user: {
  id: number,
  name: string
} = {
  id: 1,
  name: 'John'
}
```

Specify the type separately:

```typescript
type User = {
	id: number;
  name: string;
}

const user: User = {
  id: 1,
  name: John
}
```

## Type Assertion <a id="type-assertion"></a>

```typescript
let cid: any = 1;
let customedId = <number>cid;

customerId = '1'; // invalid

// Second option to define that `customerId` has a number type
let customerId = cid as number;
```

## Functions <a id="functions"></a>

The function takes a `number` as an argument and returns `boolean`:

```typescript
function greaterThanZero(x: number): boolean {
  return x > 0;
}
```

Function that doesn't return anything:

```typescript
function log(message: string | number): void {
  console.log(message);
}
```

## Interfaces <a id="interfaces"></a>

```typescript
interface UserInterface {
  id: number;
  name: string;
}

const user: UserInterface = {
  id: 1,
  name: 'John'
}
```

Difference between `type` and `interface`:

```typescript
// Valid `type` usage
type ID = number | string;
const cid: ID = 1;

// Invalid: interfaces couldn't be used with primitives and unions
interface ID = number | string;
const cid: ID = 1;
```

Optional and read only properties:

```typescript
interface Book {
  author: string;
  year?: number; // an optional property
  readonly publisher: string; // a read only property
}
```

An interface for a function that takes numbers as arguments and returns a number:

```typescript
interface SumFunc {
  (x: number, y: number): number;
}

const sum: SumFunc = (x, y) => x + y;
```

## Classes <a id="classes"></a>

```typescript
class Person {
  id: number;
  name: string;
  private age: number; // Could be accessed only within this class using `this`
  protected sex: string; // Could be accessed only within this class or any child classes using `this`
  
  constructor(id: number, name: string, age: number, sex: string) {
    this.id = id;
    this.name = name;
    this.age = age;
    this.sex = sex;
  }
}

const user = new Person(1, 'John');
console.log(user.age) // invalid as `age` is private
console.log(user.sex) // invalid as `sex` is protected
```

Child class:

```typescript
class Employee extends Person {
  position: string;
  
  constructor(id: number, name: string, age: number, sex: string, position: string) {
    super(id, name, age, sex);
    this.position = position;
  }
  
  showSex(): void {
    console.log(this.sex) // protected parameters could be used inside extended classes
  }
}
```

An interface for the class:

```typescript
interface PersonInterface {
  id: number;
  name: string;
  register(): string;
}

class Person implements PersonInterface {
  constructor(id: number, name: string) {
    this.id = id;
    this.name = name;
  }
  
  register() {
    return `${this.name} is registered`;
  }
}
```

## Generics <a id="generics"></a>

```typescript
// `<T>` is a placeholder instead of `any` to define a type later
function getArray<T>(items: T[]): T[] {
  return new Array().concat(items);
}

let numArray = getArray<number>([1, 2, 3]);
let strArray = getArray<string>(['John', 'Mary', 'Tom']);
```