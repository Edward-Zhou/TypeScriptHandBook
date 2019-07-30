# Interfaces

- TypeScript's core principle is type checking focuses on the shape that values have
- interfaces fill hte role of naming these types, and are a powerful way of defining contracts within your code or outside your project

## [Simple Interface](Interfaces.ts#2)

``` ts
function printLabel(labeledObj:{ label: string}){
    console.log(labeledObj.label);
}

let myObj = {size : 10, label: "Size 10 Object"};
printLabel(myObj);
```

## [Optional Properties](Interfaces.ts#10)
``` ts
interface ISquareConfig{
    color?: string;
    width?: number;
}

function createSquare(config: ISquareConfig):{ color: string; area: number}{
    let newSquare  = { color:"white", area : 100};
    if(config.color){
        newSquare.color = config.color;
    }
    if (config.width) {
        newSquare.area = config.width * config.width;
    }
    return newSquare;
}
let mySquare = createSquare({ color:"Black"});
console.log(mySquare);
```

## [Readonly Properties](Interfaces.ts#29)

``` ts
// Readonly properties
interface IPoint {
    readonly x: number;
    readonly y: number;
}
let p1: IPoint = { x:10, y:20};
// p1.x = 5; // p1.x is readonly property
```

## readonly vs const

Variables use `const` whereas properties use readonly.

## [Function Types](Interfaces.ts#37)
``` ts
interface ISearchFunc{
    (source : string, subString: string): boolean;
}
let mySearch: ISearchFunc;
mySearch = function(source: string, subString: string){
    let result = source.search(subString);
    return result > -1;
}
console.log(mySearch("test","t"));
```

## [Indexable Types](Interfaces.ts#48)
``` ts
interface IStringArray{
    [index: number]: string;
}
let myArray : IStringArray;
myArray = ["Bob", "Jack"];
let myStr : string = myArray[0];
console.log(myStr);
```

## [Class Types](Interfaces.ts#57)
``` ts
interface IClockInterface{
    currentTime: Date;
    tick():void;
    setTime(d: Date): void;
}

class Clock implements IClockInterface{
    currentTime: Date = new Date();
    tick() {
        
    }
    setTime(d: Date){
        this.currentTime = d;
    }

    constructor(h:number, m:number){ }
}

//Difference between the static and instance sides of classes
interface IClockConstructor{
    new (hour: number, minute: number): IClockInterface;
}

// class ErrorClock implements IClockConstructor{
//     constructor(hour: number, minute: number){}
// }
function createClock(ctor: IClockConstructor, hour: number, minute: number): IClockInterface{
    return new ctor(hour, minute);
}
class DigitalClock implements IClockInterface{
    constructor(h: number, m: number){}
    currentTime: Date = new Date();
    tick(){
        console.log("Digital Clock");
    }
    setTime(d: Date){
        this.currentTime = d;
    }
}
class AnalogClock implements IClockInterface{
    constructor(h: number, m: number){}
    currentTime: Date = new Date();
    tick(){
        console.log("Analog Clock");
    }
    setTime(d: Date){
        this.currentTime = d;
    }
}
let digitalClock = createClock(DigitalClock, 12, 17);
let analog = createClock(AnalogClock, 7, 32);

//class expression
const ClockExpression: IClockConstructor = class ClockExpression implements IClockInterface{
    currentTime: Date = new Date();
    constructor(h:number, m: number){}
    tick(){
        console.log("ClockExpression Clock");
    }
    setTime(d: Date){
        this.currentTime = d;
    }
}
let clockExpression = new ClockExpression(12,12);
clockExpression.tick();
```

## [Extending Interfaces](Interfaces.ts#124)
``` ts
interface IShape{
    color: string;
}
interface ISquare extends IShape{
    sideLength: number;
}
let square = {} as ISquare;
square.color = "Blue";
square.sideLength = 10;
```
## [Hybrid Types](Interfaces.ts#135)
```ts
interface ICounter{
    (start : number): string;
    interval: number;
    reset(): void;
}
function getCounter() : ICounter{
    let counter = (function (start: number){ }) as ICounter;
    counter.interval = 123;
    counter.reset = function(){
        this.start = 0
        this.interval = 0;
    };
    return counter;
}
let c = getCounter();
c(10);
c.reset();
c.interval = 5.0;
```

## [Interfaces Extending Classes](Interfaces.ts#135)

This means that when you create an interface that extends a class with private or protected members, that interface type can only be implemented by that class or a subclass of it.
```ts
class Control{
    private state : any;
}
interface ISelectableControl extends Control{
    select(): void;
}
class Button extends Control implements ISelectableControl{
    select(){}
}
class TextBox extends Control{
    select(){}
}
// class ImageControl implements ISelectableControl
// {
//     private state: any;
//     select(){}
// }

```