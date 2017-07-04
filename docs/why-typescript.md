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

어떤 언어에서는 (특히 명목적 타입의 언어들에서는) 정적 타입 변화가 불필요한 격식을 필요로 하기도 합니다. 왜냐하면 *당신은* 그런 격식 없이도 코드가 잘 동작할 것이란 걸 알지만, 언어 의미 구조상 그렇게 해야하기 때문입니다. 그렇기 때문에 C#에서는 [오토 매퍼](http://automapper.org) 같은 것이 *필수적*입니다. 하지만 우리는 JavaScript 개발자에게 최소한의 인지 과부하를 주고 싶었기 때문에 TypeScript에서는 타입을 *구조적*으로 하였습니다. 이것은 *덕 타입*이 특급 언어 구조체란 것을 의미합니다. 다음과 같은 예제를 생각해 보십시오. `iTakePoint2D` 함수는 `x`와 `y`를 포함하기만 하면 모두 받아들일 것입니다.

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

iTakePoint2D(point2D); // 정확히 매치가 맞으므로 괜찮음
iTakePoint2D(point3D); // 추가로 매개변수가 있는 것은 괜찮음
iTakePoint2D({ x: 0 }); // 에러: `y`가 필요함
```

### 타입 에러가 있어도 JavaScript를 내놓는다

TypeScript는 당신의 JavaScript 코드를 TypeScript로 쉽게 변환할 수 있도록 컴파일 에러가 있어도 최대한 할 수 있는 한도 내에서 *유효한 JavaScript를 내놓을 것입니다*.

```ts
var foo = 123;
foo = '456'; // 에러: `number` 타입에 `string`을 할당할 수 없음
```

위 코드는 아래의 js코드로 나올 것입니다:

```ts
var foo = 123;
foo = '456';
```

그러므로 당신의 JavaScript 코드를 조금씩 점진적으로  TypeScript로 변환할 수 있습니다. 다른 언어의 컴파일러들 과는 꽤 다릅니다. 이런 것이 TypeScript를 쓸 또 다른 이유입니다.

### 타입은 앰비언트(ambient)할 수 있다

TypeScript의 중요한 설계 목표 중 하나는 기존의 JavaScript 라이브러리를 쉽고 안전하게 사용할 수 있게 하는 것입니다. TypeScript는 이것을 *선언(declaration)*을 통해서 합니다. 당신이 선언 구문에 얼마나 노력을 기울이냐에 따라서 코드 안전성과 코드 완성 기능의 수준이 결정됩니다. 유명한 JavaScript 라이브러리는 대부분 이미 [DefinitelyTyped community](https://github.com/borisyankov/DefinitelyTyped)에 정의되어 있음을 알아두세요. 그러므로 대부분의 경우,

1. 선언 정의(definition) 파일은 대부분 존재합니다.
1. 아니면, 이미 잘 검토된 방대한 양의 TypeScript 선언 템플릿이 존재합니다.

어떻게 선언 파일을 만드는 지에 대한 간단한 예로 [jquery](https://jquery.com/)를 사용한 예제를 보겠습니다. 기본적으로 TypeScript는 (훌륭한 JavaScript 코드가 대부분 그러하듯이) `var` 등을 사용해 선언하지 않고 변수를 사용하는 것을 금합니다.

```ts
$('.awesome').show(); // 에러: `$`를 찾을 수 없음
```

이것을 고치는 빠른 방법은 TypeScript한테 어딘가에 `$`이 정말로 있다고 알려주는 것입니다.

```ts
declare var $:any;
$('.awesome').show(); // 오케이!
```

이 기본적인 정의에서 조금 더 정보를 주면 에러를 막는데 도움이 될 수 있습니다.

```ts
declare var $:{
    (selector:string): any;
};
$('.awesome').show(); // 오케이!
$(123).show(); // 에러: $() 안에 문자열이 와야 함
```

기존 JavaScript 코드를 위해 TypeScript 선언 구문을 만드는 자세한 방법에 대해서는 `interface`나 `any` 등과 같은 TypeScript 기본에 대해서 먼저 배우고 난 이후에 다루도록 하겠습니다.

## 미래의 JavaScript를 현재로

TypeScript는 ES6에 예정된 많은 기능들을 현재의 (ES5만 지원하는) JavaScript 엔진에서 지원합니다. TypeScript 팀은 이런 기능들을 적극적으로 집어넣고 있습니다. 그리고 이런 기능들의 수는 계속 늘어나고 있습니다. 자세한 내용은 각 해당 섹션에서 다루겠습니다만, 맛보기로 예제를 조금만 보겠습니다.

```ts
// 클래스 사용
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

```ts
// 화살표 함수 사용
var inc = (x)=>x+1;
```

### 정리

우리는 이 섹션에서 TypeScript이 만들어진 이유와 목표에 대해 알아보았습니다. 이제 TypeScript의 상세한 내용들을 낱낱이 파보겠습니다.

[](Interfaces are open ended)
[](Type Inferernce rules)
[](Cover all the annotations)
[](Cover all ambients : also that there are no runtime enforcement)
[](.ts vs. .d.ts)
