# 왜 TypeScript인가

TypeScript에는 2가지 목표가 있습니다:
* 첫번째는 JavaScript에 *타입 시스템* 을 *선택적* 으로 제공하는 것이고,
* 두번째는 미래에 JavaScript에서 구현하기로 계획된 기능들을 현재의 JavaScript 엔진에서 미리 제공하는 것입니다.

이러한 목표는 아래와 같은 이유에서 시작되었습니다.

## TypeScript 타입 시스템

당신은 어쩌면 "**왜 JavaScript에 타입을 넣겠다는 거야?**"라고 생각할지도 모르겠습니다.

타입 시스템은 코드의 질을 향상하고 이해도를 높이는 것으로 증명되었습니다. Google, Microsoft, Facebook 같은 큰 기업들이 다들 이와 같은 결론에 도달하였습니다. 특히,

* 타입은 리팩토링을 할 때 당신을 기민하게 해줍니다. *런타임 때 프로그램이 깨지는 것보다 컴파일 때 에러가 나는 편이 낫습니다.*
* 문서화 작업을 할 때 최고의 형식이 타입을 쓰는 것입니다. *마치 function signature는 이론이고 function body는 증명이라고 비유할 수 있습니다.*.

그렇지만 타입은 지나치게 격식을 갖춘 듯한 느낌도 있습니다. 그래서 TypeScript는 입문 장벽을 최대한 낮추려고 다음과 같이 설계되었습니다.

### 당신의 JavaScript는 이미 TypeScript입니다

TypeScript는 컴파일 시 타입 확인을 합니다. 이름부터 TypeScript니깐 이건 당연하죠. TypeScript의 좋은 점은 이러한 타입 사용이 완전히 선택적이라는 점입니다. `.js` JavaScript 파일을 `.ts`로 바꾸고 컴파일을 해도 원본과 동등한 유효한 `.js`가 나올 것입니다. 이것은 TypeScript가 선택적인 타입 체킹이 추가된 *의도적이고도* 엄격한 JavaScript의 대집합이기 때문입니다.

### Types can be Implicit
TypeScript will try to infer as much of the type information as it can in order to give you type safety with minimal cost of productivity during code development. For example, in the following example TypeScript will know that foo is of type `number` below and will give an error on the second line as shown:

```ts
var foo = 123;
foo = '456'; // Error: cannot assign `string` to `number`

// Is foo a number or a string?
```
This type inference is well motivated. If you do stuff like shown in this example, then, in the rest of your code, you cannot be certain that `foo` is a `number` or a `string`. Such issues turn up often in large multi-file code bases. We will deep dive into the type inference rules later.

### Types can be Explicit
As we've mentioned before, TypeScript will infer as much as it can safely, however you can use annotations to:
1. Help along the compiler, and more importantly document stuff for the next developer who has to read your code (that might be future you!).
1. Enforce that what the compiler sees, is what you thought it should see. That is your understanding of the code matches an algorithmic analysis of the code (done by the compiler).

TypeScript uses postfix type annotations popular in other *optionally* annotated languages (e.g. ActionScript and F#).

```ts
var foo: number = 123;
```
So if you do something wrong the compiler will error e.g.:

```ts
var foo: number = '123'; // Error: cannot assign a `string` to a `number`
```

We will discuss all the details of all the annotation syntax supported by TypeScript in a later chapter.

### Types are structural
In some languages (specifically nominally typed ones) static typing results in unnecessary ceremony because even though *you know* that the code will work fine the language semantics force you to copy stuff around. This is why stuff like [automapper for C#](http://automapper.org/) is *vital* for C#. In TypeScript because we really want it to be easy for JavaScript developers with a minimum cognitive overload, types are *structural*. This means that *duck typing* is a first class language construct. Consider the following example. The function `iTakePoint2D` will accept anything that contains all the things (`x` and `y`) it expects:

```ts
interface Point2D {
    x: number;
    y: number;
}
interface Point3D {
    x: number;
    y: number;
    z: number;
}
var point2D: Point2D = { x: 0, y: 10 }
var point3D: Point3D = { x: 0, y: 10, z: 20 }
function iTakePoint2D(point: Point2D) { /* do something */ }

iTakePoint2D(point2D); // exact match okay
iTakePoint2D(point3D); // extra information okay
iTakePoint2D({ x: 0 }); // Error: missing information `y`
```

### Type errors do not prevent JavaScript emit
To make it easy for you to migrate your JavaScript code to TypeScript, even if there are compilation errors, by default TypeScript *will emit valid JavaScript* the best that it can. e.g.

```ts
var foo = 123;
foo = '456'; // Error: cannot assign a `string` to a `number`
```

will emit the following js:

```ts
var foo = 123;
foo = '456';
```

So you can incrementally upgrade your JavaScript code to TypeScript. This is very different from how many other language compilers work and yet another reason to move to TypeScript.

### Types can be ambient
A major design goal of TypeScript was to make it possible for you to safely and easily use existing JavaScript libraries in TypeScript. TypeScript does this by means of *declaration*. TypeScript provides you with a sliding scale of how much or how little effort you want to put in your declarations, the more effort you put the more type safety + code intelligence you get. Note that definitions for most of the popular JavaScript libraries have already been written for you by the [DefinitelyTyped community](https://github.com/borisyankov/DefinitelyTyped) so for most purposes either:

1. The definition file already exists.
1. Or at the very least, you have a vast list of well reviewed TypeScript declaration templates already available

As a quick example of how you would author your own declaration file, consider a trivial example of [jquery](https://jquery.com/). By default (as is to be expected of good JS code) TypeScript expects you to declare (i.e. use `var` somewhere) before you use a variable
```ts
$('.awesome').show(); // Error: cannot find name `$`
```
As a quick fix *you can tell TypeScript* that there is indeed something called `$`:
```ts
declare var $:any;
$('.awesome').show(); // Okay!
```
If you want you can build on this basic definition and provide more information to help protect you from errors:
```ts
declare var $:{
    (selector:string): any;
};
$('.awesome').show(); // Okay!
$(123).show(); // Error: selector needs to be a string
```

We will discuss the details of creating TypeScript definitions for existing JavaScript in detail later once you know more about TypeScript (e.g. stuff like `interface` and the `any`).

## 미래의 JavaScript를 현재로
TypeScript provides a number of features that are planned in ES6 for current JavaScript engines (that only support ES5 etc). The typescript team is actively adding these features and this list is only going to get bigger over time and we will cover this in its own section. But just as a specimen here is an example of a class:

```ts
class Point {
    constructor(public x: number, public y: number) {
    }
    add(point: Point) {
        return new Point(this.x + point.x, this.y + point.y);
    }
}

var p1 = new Point(0, 10);
var p2 = new Point(10, 20);
var p3 = p1.add(p2); // {x:10,y:30}
```

and the lovely fat arrow function:

```ts
var inc = (x)=>x+1;
```

### 정리
우리는 이 섹션에서 TypeScript이 만들어진 이유와 목표에 대해 알아보았습니다. 이제 TypeScript의 상세한 내용들을 낱낱이 파보겠습니다.

[](Interfaces are open ended)
[](Type Inferernce rules)
[](Cover all the annotations)
[](Cover all ambients : also that there are no runtime enforcement)
[](.ts vs. .d.ts)
