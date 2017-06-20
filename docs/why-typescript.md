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

### 타입은 암시적일 수 있다

TypeScript는 당신의 개발 생산성에는 영향을 최소한으로 줄이면서 타입 안전성을 위해 최대한 타입 정보를 유추하려고 할 것입니다. 예를 들어, 다음 예제에서 TypeScript는 foo가 `number` 타입임을 알고 두번째 라인에서 에러를 낼 것입니다.

```ts
var foo = 123;
foo = '456'; // 에러: `number` 타입에 `string`을 할당할 수 없음

// foo는 이제 무슨 타입이지?
```

이런 타입 유추는 좋은 의도로 만들어졌습니다. 당신이 만약 위의 예제처럼 한다면, 그 이후의 코드에서 `foo`가 `number`인지 `string`인지 확신할 수 없을 것입니다. 이런 문제들은 다수의 파일이 있는 큰 코드 기반에서 자주 발생합니다. 타입 유추 규칙에 대해서는 나중에 더 깊히 파보겠습니다.

### 타입은 명시적일 수 있다

이미 언급했듯이, TypeScript는 최대한 안전하게 타입을 유추합니다. 그러나 당신이 어노테이션(annotation)을 달면 다음과 같은 장점이 있습니다.

1. 먼저 컴파일러를 도와주기 위해서이기도 하지만, 더욱 중요한 것은 당신의 코드를 읽어야 하는 다음 개발자(혹은 미래의 당신)를 위해서 문서화한다는 의미가 있습니다.

1. 당신이 컴파일러가 볼 것이라고 생각하는 것과 컴파일러가 실제로 보는 것이 같게끔 합니다. 즉, 당신이 이해한 코드가 (컴파일러가 분석한) 알고리즘과 매치되게끔 해줍니다.

TypeScript는 다른 *선택적(optional)* 어노테이션 언어(ActionScript나 F# 등)에서 자주 쓰이는 후치(postfix) 타입 어노테이션 문법이 쓰입니다.

```ts
var foo: number = 123;
```

이제 뭔가 잘못되면 컴파일러가 에러를 냅니다.

```ts
var foo: number = '123'; // 에러: `number` 타입에 `string`을 할당할 수 없음
```

Typescript가 지원하는 어노테이션 문법에 대해서는 이후의 장에서 상세히 설명하겠습니다.

### 타입은 구조적이다

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

iTakePoint2D(point2D); // 정확히 매치가 맞으므로 괜찮다
iTakePoint2D(point3D); // 추가로 매개변수가 있는 것은 괜찮다
iTakePoint2D({ x: 0 }); // 에러: `y`가 필요함
```

### 타입 에러가 있어도 JavaScript를 내놓는다

To make it easy for you to migrate your JavaScript code to TypeScript, even if there are compilation errors, by default TypeScript *will emit valid JavaScript* the best that it can. e.g.

```ts
var foo = 123;
foo = '456'; // 에러: `number` 타입에 `string`을 할당할 수 없음
```

위 코드는 아래의 js코드로 나올 것입니다:

```ts
var foo = 123;
foo = '456';
```

So you can incrementally upgrade your JavaScript code to TypeScript. This is very different from how many other language compilers work and yet another reason to move to TypeScript.

### 타입은 앰비언트(ambient)할 수 있다

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
