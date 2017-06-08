* [TypeScript 시작하기](#getting-started-with-typescript)
* [TypeScript 버전](#typescript-version)

# TypeScript 시작하기

TypeScript는 JavaScript로 컴파일됩니다. 브라우져든 서버든 상관없이 실제로 실행되는 코드는 JavaScript입니다. 그래서 다음과 같은 도구가 필요합니다.

* TypeScript 컴파일러 ([소스코드](https://github.com/Microsoft/TypeScript/) 혹은  [NPM](https://www.npmjs.com/package/typescript))
* TypeScript 편집기 (메모장을 이용해도 되지만 저는 [alm 🌹](http://alm.tools)을 씁니다. 그 외에도 [많은 다른 IDE]( https://github.com/Microsoft/TypeScript/wiki/TypeScript-Editor-Support)가 있습니다.)


![](https://raw.githubusercontent.com/alm-tools/alm-tools.github.io/master/screens/main.png)


## TypeScript 버전

저는 이 책에서 *안정적인* TypeScript 컴파일러만 쓰지는 않을 것입니다. 새로운 기능들을 특별히 버전을 언급하지 않고 설명하겠습니다. 저는 흔히 비안정적인 최신 버전(nightly version)을 쓰라고 추천합니다. 왜냐하면 **컴파일러를 테스트할 때는, 시간이 더 흘러야만 더 많은 버그가 해결되기 때문입니다**.

커맨드 라인에서 아래와 같이 설치하면,

```
npm install -g typescript@next
```

이후로 `tsc`를 쓰면 항상 최신 버전을 이용하게 됩니다. 많은 IDE가 이를 지원합니다. 예를들어,

* `alm`은 항상 최신 TypeScript 버전을 씁니다.
* 비주얼 스튜디오 코드에서는 `.vscode/settings.json` 파일을 아래와 같이 생성하면 됩니다.
```json
{
  "typescript.tsdk": "./node_modules/typescript/lib"
}
```

## 소스 코드 받기
이 책에 쓰인 소스 코드는 https://github.com/basarat/typescript-book/tree/master/code 에서 받으실 수 있습니다. 대부분의 코드 예제는 alm에서 그대로 실행할 수 있습니다. 만약 (npm 모듈같은) 추가적인 설치가 필요한 코드 예제는 아래와 같이 코드 위에 링크를 표기해두겠습니다.

`this/will/be/the/link/to/the/code.ts`
```ts
// This will be the code under discussion
```

개발 환경이 셋업되었으면 TypeScript 문법을 보겠습니다.
