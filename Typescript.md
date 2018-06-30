# Basic Types

## Array

TypeScript, like JavaScript, allows you to work with arrays of values. Array types can be written in one of two ways. In the first, you use the type of the elements followed by `[]` to denote an array of that element type:

```ts
let list: number[] = [1, 2, 3];
```

## Tuple

Tuple types allow you to express an array where the type of a fixed number of elements is known, but need not be the same. For example, you may want to represent a value as a pair of a `string` and a `number`:(different with the python's tuple's idea)

```ts
// Declare a tuple type
let x: [string, number];
// Initialize it
x = ["hello", 10]; // OK
// Initialize it incorrectly
x = [10, "hello"]; // Error
```

## Enum

A helpful addition to the standard set of datatypes from JavaScript is the `enum`. As in languages like C#, an enum is a way of giving more friendly names to sets of numeric values. (a handy way to get a set of defined values)

```ts
enum Color {Red, Green, Blue}
let c: Color = Color.Green;
// or
enum Color {Red = 1, Green = 2, Blue = 4}
```

## Any

We may need to describe the type of variables that we do not know when we are writing an application. These values may come from dynamic content, e.g. from the user or a 3rd party library. In these cases, we want to opt-out of type-checking and let the values pass through compile-time checks. To do so, we label these with the `any` type:(just like a plain javascript)

```ts
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // okay, definitely a boolean
```

## Void

`void` is a little like the opposite of `any`: the absence of having any type at all. You may commonly see this as the return type of functions that do not return a value: (most used in function definition meaning the function returns none)

```ts
function warnUser(): void {
    alert("This is my warning message");
}
```

## Type assertions

*Type assertions* are a way to tell the compiler “trust me, I know what I’m doing.” A type assertion is like a type cast in other languages, but performs no special checking or restructuring of data. It has no runtime impact, and is used purely by the compiler. TypeScript assumes that you, the programmer, have performed any special checks that you need.

```ts
let someValue: any = "this is a string";
// use 'as' as the type assertion indicator
let strLength: number = (someValue as string).length;
```

# Interfaces

One of TypeScript’s core principles is that type-checking focuses on the *shape*  that values have. This is sometimes called “duck typing” or “structural subtyping”. In TypeScript, interfaces fill the role of naming these types, and are a  powerful way of defining contracts within your code as well as  contracts with code outside of your project.

```ts
// basic usage
interface LabelledValue {
    label: string;
}

function printLabel(labelledObj: LabelledValue) {
    console.log(labelledObj.label);
}

// Optional Properties (the object may not contain all the properties in the interface)
// using '?' to indicate
interface SquareConfig {
    color?: string;
    width?: number;
}

// readonly Properties (the property can not be changed)
// with keyword 'readonly'
interface Point {
    readonly x: number;
    readonly y: number;
}

// indicate some extra properties that can be included in the interface
interface SquareConfig {
    color?: string;
    width?: number;
    [propName: string]: any;
}
```

## Function Types

To describe a function type with an interface, we give the interface a  call signature. This is like a function declaration with only the parameter list and  return type given. Each parameter in the parameter list requires both  name and type.

```ts
interface SearchFunc {
    (source: string, subString: string): boolean;
}

let mySearch: SearchFunc;
mySearch = function(src, sub){
    let result = src.search(sub);
    return result > -1;
}
```

## Indexable Types

Indexable types have an *index signature* that describes the types we can use to index into the object, along with the corresponding return types when indexing

```ts
interface StringArray {
    [index: number]: string;
}

let myArray: StringArray;
myArray = ["Bob", "Fred"];

let myStr: string = myArray[0];

// with readonly
interface ReadonlyStringArray {
    readonly [index: number]: string;
}
let myArray: ReadonlyStringArray = ["Alice", "Bob"];
myArray[2] = "Mallory"; // error!
```

## Class Types

```ts
interface ClockInterface {
    currentTime: Date;
    setTime(d: Date);
}

class Clock implements ClockInterface {
    currentTime: Date;
    setTime(d: Date) {
        this.currentTime = d;
    }
    constructor(h: number, m: number) { }
}
```

When a class implements an interface, only the instance side of the class is checked. Since the constructor sits in the static side, it is not included in this check. Instead, you would need to work with the static side of the class directly.

```ts
// the interface for the static site
interface ClockConstructor {
    new (hour: number, minute: number): ClockInterface;
}
// the interface for the instance site
interface ClockInterface {
    tick();
}

function createClock(ctor: ClockConstructor, hour: number, minute: number): ClockInterface {
    return new ctor(hour, minute);
}

class DigitalClock implements ClockInterface {
    constructor(h: number, m: number) { }
    tick() {
        console.log("beep beep");
    }
}
class AnalogClock implements ClockInterface {
    constructor(h: number, m: number) { }
    tick() {
        console.log("tick tock");
    }
}

let digital = createClock(DigitalClock, 12, 17);
let analog = createClock(AnalogClock, 7, 32);
```

## Extending Interfaces

```ts
interface Shape {
    color: string;
}

interface PenStroke {
    penWidth: number;
}

interface Square extends Shape, PenStroke {
    sideLength: number;
}

let square = <Square>{};
square.color = "blue";
square.sideLength = 10;
square.penWidth = 5.0;
```