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
# Class

## Public, private, protected and readonly modifiers

1. In TypeScript, each member is `public` by default.
2. When a member is marked `private`, it cannot be accessed from outside of its containing class
3. The `protected` modifier acts much like the `private` modifier with the exception that members declared `protected` can also be accessed within deriving classes
4. Readonly properties must be initialized at their declaration or in the constructor.

## Abstract Classes

Abstract classes are base classes from which other classes may be derived. They may not be instantiated directly. Unlike an interface, an abstract class may contain implementation details for its members

Methods within an abstract class that are marked as abstract do not  contain an implementation and must be implemented in derived classes. Abstract methods share a similar syntax to interface methods. Both define the signature of a method without including a method body. However, abstract methods must include the `abstract` keyword and may optionally include access modifiers.

```ts
abstract class Department {

    constructor(public name: string) {
    }

    printName(): void {
        console.log("Department name: " + this.name);
    }

    abstract printMeeting(): void; // must be implemented in derived classes
}

class AccountingDepartment extends Department {

    constructor() {
        super("Accounting and Auditing"); // constructors in derived classes must call super()
    }

    printMeeting(): void {
        console.log("The Accounting Department meets each Monday at 10am.");
    }

    generateReports(): void {
        console.log("Generating accounting reports...");
    }
}
```

# Functions

## Optional and Default Parameters

```ts
// optional parameters with ? which can be not offered
function buildName(firstName: string, lastName?: string) {
    if (lastName)
        return firstName + " " + lastName;
    else
        return firstName;
}
// default parameter can be assigned directly
function buildName(firstName: string, lastName = "Smith") {
    return firstName + " " + lastName;
}
```

## Rest Parameters

```ts
// with ...
function buildName(firstName: string, ...restOfName: string[]) {
    return firstName + " " + restOfName.join(" ");
}
```

# Generics

In languages like C# and Java, one of the main tools in the toolbox for creating reusable components is *generics*, that is, being able to create a component that can work over a variety of types rather than a single one. This allows users to consume these components and use their own types

 we will use a *type variable*, a special kind of variable that works on types rather than values.

```ts
function identity<T>(arg: T): T {
    return arg;
}
```