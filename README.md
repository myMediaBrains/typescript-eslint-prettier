# [TypeScript](https://khalilstemmler.com/blogs/typescript/node-starter-project/)

## 설치 및 셋업

| no  | 구분                                                                                                                                                | 설명                                                                                   |
| --- | --------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| 1   | $ mkdir typescript-eslint-prettier && cd typescript-eslint-prettier                                                                                 | 프로젝트 폴더 생성                                                                     |
| 2   | $ npm init -y                                                                                                                                       | package.json                                                                           |
| 3   | $ npm i -D typescript                                                                                                                               | TypeScript 설치                                                                        |
| 4   | $ npm i -D @types/node                                                                                                                              | ambient types 는 글로벌 execution scope 에 추가되는 types 이다.                        |
| 5   | $ touch tsconfig.json                                                                                                                               | root 폴더                                                                              |
| 6   | $ npx tsc --init --rootDir src --outDir build --esModuleInterop --resolveJsonModule --lib es6 --module commonjs --allowJs true --noImplicitAny true | tsconfig.json 자동 config                                                              |
|     | \--rootDir src                                                                                                                                      | TypeScript의 root 디렉토리: src/ 폴더 지정                                             |
|     | \--outDir build                                                                                                                                     | TypeScript의 compiled code 저장 디렉토리: build/ 폴더 지정                             |
|     | \--esModuleInterop                                                                                                                                  | (true) commonjs 사용                                                                   |
|     | \-- resolveJsonModule                                                                                                                               | (true) JSON 사용                                                                       |
|     | \--lib es6                                                                                                                                          | es6 언어 기능들을 utilize할 수 있고, 단, 모두 es5로 컴파일된다.                        |
|     | \--module commonjs                                                                                                                                  | 스탠다드 Node module system                                                            |
|     | \--allowJs true                                                                                                                                     | old JavaScript 를 TypeScript로 변환할 때, .ts 파일들에 .js 파일들을 포함하는 것을 허용 |
|     | \--noImplicitAny true                                                                                                                               | TypeScript 파일에서, a type이 불명확하게 지정되어지는 것을 허용하지 않는다.            |
|     |                                                                                                                                                     | 반드시 특정 type을 가지거나, 명시적으로 any를 선언해야 한다.                           |
| 7   | $ tsconfig.json                                                                                                                                     | 6) 번 명령 실행하면 자동으로 설정                                                      |

tsconfig.json

```json
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "lib": ["es6"],
    "allowJs": true,
    "outDir": "build",
    "rootDir": "src",
    "strict": true,
    "noImplicitAny": true,
    "esModuleInterop": true,
    "resolveJsonModule": true
  }
}
```

## index.ts

| no  | 구분                 | 설명          |
| --- | -------------------- | ------------- |
| 8   | $ mkdir src          | src 폴더 생성 |
| 9   | $ touch src/index.ts |               |

src/index.ts

```tsx
console.log('Hello World!');
```

## Compile

| no  | 구분      | 설명                            |
| --- | --------- | ------------------------------- |
| 10  | $ npx tsc | build 폴더에 index.js 파일 생성 |

build/index.js

```tsx
'use strict';
console.log('Hello world!');
```

## Cole Reloading 셋업

| no  | 구분                       | 설명                                       |
| --- | -------------------------- | ------------------------------------------ |
| 11  | $ npm i -D ts-node nodemon | 콜드 리로딩을 위한 ts-node 및 nodemon 설치 |
| 12  | $ touch nodemon.json       | nodemont 에 대한 config 를 추가한다.       |

nodemon.json

- nodemon 이 호출되면, ts-node ./src/index.ts 가 실행되고, .ts, .js 로 확장자가 바뀐다.

```tsx
{
  "watch": ["src"],
  "ext": ".ts,.js",
  "ignore": [],
  "exec": "ts-node ./src/index.ts"
}
```

| no  | 구분                 | 설명                                                         |
| --- | -------------------- | ------------------------------------------------------------ |
| 13  | $ npm i -D rimraf    | production 으로 build 할 때, 이전 ./build 를 clean 하기 위함 |
| 14  | $ touch package.json | “start:dev”, “build”, “start” 명령을 다음과 같이 작성한다.   |

package.json

```json
{
  "name": "typescript-eslint-prettier",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "build": "rimraf ./build && tsc",
    "start:dev": "nodemon",
    "start": "npm run build && node build/index.js"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "@types/node": "^16.4.13",
    "nodemon": "^2.0.12",
    "ts-node": "^10.2.0",
    "typescript": "^4.3.5"
  }
}
```

## npm run

| no  | 구분                | 설명                    |
| --- | ------------------- | ----------------------- |
| 15  | $ npm run start:dev | 개발 모드로 app 시작    |
| 16  | $ npm run build     | build 폴더를 clean 한다 |
|     | $ npm run start     | 프로덕션으로 app 시작   |

# [ESLint](https://khalilstemmler.com/blogs/typescript/eslint-for-typescript/)

## 설치 및 셋업

| no  | 구분                                                                         | 설명                               |
| --- | ---------------------------------------------------------------------------- | ---------------------------------- |
| 1   | $ npm i -D eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin | typescript eslint 관련 패키지 설치 |
| 2   | $ touch .eslintrc                                                            | eslint config 파일 생성            |

.eslintrc

```tsx
{
  "root": true,
  "parser": "@typescript-eslint/parser",
  "plugins": [
    "@typescript-eslint"
  ],
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/eslint-recommended",
    "plugin:@typescript-eslint/recommended"
  ]
}
```

| no  | 구분                  | 설명                               |
| --- | --------------------- | ---------------------------------- |
| 3   | $ touch .eslintignore | ESLint 하지 않는 폴더 및 파일 작성 |

.eslintignore

```tsx
node_modules;
dist;
```

| no  | 구분                 | 설명             |
| --- | -------------------- | ---------------- |
| 4   | $ touch package.json | “lint” 명령 추가 |

package.json

```json
{
  ...
  "scripts": {
    ...
    "lint": "eslint . --ext .ts",
  }
}
```

## rules 설정

| no  | 구분              | 설명                                  |
| --- | ----------------- | ------------------------------------- |
| 5   | $ touch .eslintrc | **rules** 속성 추가                   |
|     |                   | rules 모드: 0(off), 1(warn), 2(error) |

.eslintrc

```tsx
....
	"rules": {
    "no-console": 2
  }
...
```

| no  | 구분           | 설명                  |
| --- | -------------- | --------------------- |
| 6   | $ npm run lint | console.log 에러 발생 |

```sh
➜  typescript-eslint-prettier npm run lint

> typescript-eslint-prettier@1.0.0 lint
> eslint . --ext .ts


/Users/catchhub/forme_c/typescript-eslint-prettier/src/index.ts
  1:1  error  Unexpected console statement  no-console

✖ 1 problem (1 error, 0 warnings)
```

| no  | 구분              | 설명             |
| --- | ----------------- | ---------------- |
| 7   | $ touch .eslintrc | silence 업데이트 |

.eslintrc

```tsx
...
"rules": {
    "no-console": 0
  }
...
```

| no  | 구분           | 설명                                                    |
| --- | -------------- | ------------------------------------------------------- |
| 8   | $ npm run lint | silence 실행                                            |
|     |                | console.log .. 실행 안하고, warn 또는 error 표시가 없음 |

```sh
➜  typescript-eslint-prettier npm run lint

> typescript-eslint-prettier@1.0.0 lint
> eslint . --ext .ts

➜  typescript-eslint-prettier
```

| no  | 구분                                       | 설명                                                                                                       |
| --- | ------------------------------------------ | ---------------------------------------------------------------------------------------------------------- |
| 9   | [Husky](https://github.com/typicode/husky) | (참조) 터미널에서 code 를 commit 하거나 code 를 push 하기전에 ESLint 에게 먼저 code 를 체크하는 라이브러리 |

## plugins 설정

| no  | 구분                              | 설명                                                                                             |
| --- | --------------------------------- | ------------------------------------------------------------------------------------------------ |
| 10  | $ npm i -D eslint-plugin-no-loops | no-loops 라는 plugin 을 설치                                                                     |
|     |                                   | code 에서 for 문이나 while loop 이 있는 경우에는 에러를 발생시켜, map 과 forEach 를 쓰도록 한다. |
| 11  | $ touch .eslintrc                 | plugins 에 “no-loops” 를 추가하고, rules 에 “no-loops” 속성을 설정한다.                          |
| 12  | $ touch src/index.ts              | for loop 를 사용하여, ESLint 에서 어떠한 결과가 나오는지를 체크한다.                             |

.eslintrc

```json
{
  "root": true,
  "parser": "@typescript-eslint/parser",
  "plugins": ["@typescript-eslint", "no-loops"],
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/eslint-recommended",
    "plugin:@typescript-eslint/recommended"
  ],
  "rules": {
    "no-console": 1,
    "no-loops/no-loops": 2
  }
}
```

src/index.ts

```tsx
console.log('Hello world!');

for (let i = 0; i < 10; i++) {
  console.log(i);
}
```

| No  | 구분           | 설명                                |
| --- | -------------- | ----------------------------------- |
| 13  | $ npm run lint | 2개의 warning과 1개의 error 가 발생 |

```sh
➜  typescript-eslint-prettier npm run lint

> typescript-eslint-prettier@1.0.0 lint
> eslint . --ext .ts


/Users/catchhub/forme_c/typescript-eslint-prettier/src/index.ts
  1:1  warning  Unexpected console statement  no-console
  3:1  error    loops are not allowed         no-loops/no-loops
  4:3  warning  Unexpected console statement  no-console

✖ 3 problems (1 error, 2 warnings)

➜  typescript-eslint-prettier
```

## extends 설정

ESLint는 다른 base config (예: [Shopify's](https://github.com/Shopify/eslint-plugin-shopify))를 extends 하여 사용할 수 있다.

| no  | 구분                             | 설명                    |
| --- | -------------------------------- | ----------------------- |
| 14  | $ npm i -D eslint-plugin-shopify | shopify 라이브러리 설치 |
| 15  | $ touch .eslintrc                | extends 에 shopify 설정 |

.eslintrc

```json
{
  "root": true,
  "parser": "@typescript-eslint/parser",
  "plugins": ["@typescript-eslint"],
  "extends": ["plugin:shopify/esnext"],
  "rules": {
    "no-console": 1
  }
}
```

| no  | 구분           | 설명                                                 |
| --- | -------------- | ---------------------------------------------------- |
| 16  | $ npm run lint | shopify 의 lint 에 따라 다음과 같은 메시지가 나온다. |

```sh
➜  typescript-eslint-prettier npm run lint

> typescript-eslint-prettier@1.0.0 lint
> eslint . --ext .ts


/Users/catchhub/forme_c/typescript-eslint-prettier/src/index.ts
  1:1   warning  Unexpected console statement                   no-console
  1:1   error    'console' is not defined                       no-undef
  4:3   warning  Unexpected console statement                   no-console
  4:3   error    'console' is not defined                       no-undef
  4:17  error    Missing semicolon                              babel/semi
  4:17  error    Missing semicolon                              semi
  5:2   error    Newline required at end of file but not found  eol-last

✖ 7 problems (5 errors, 2 warnings)
  3 errors and 0 warnings potentially fixable with the `--fix` option.

➜  typescript-eslint-prettier
```

## Fixing 하기: Prettier 로 대체할 수 있다.

| no  | 구분                 | 설명                     |
| --- | -------------------- | ------------------------ |
| 17  | $ touch package.json | lint-and-fix 명령어 추가 |

package.json

```json
{
  "scripts": {
    ...
    "lint": "eslint . --ext .ts",
    "lint-and-fix": "eslint . --ext .ts --fix"
  },
}
```

| no  | 구분                   | 설명                          |
| --- | ---------------------- | ----------------------------- |
| 18  | $ npm run lint-and-fix | semi 에러 메시지가 안 보인다. |

# [Prettier](https://khalilstemmler.com/blogs/tooling/prettier/)

code convention definition의 역할은 ESLint 에게, Prettier에게는 formatting의 역할을 준다.

## 설치 및 셋업

| no  | 구분                | 설명             |
| --- | ------------------- | ---------------- |
| 1   | $ npm i -D prettier | prettier 설치    |
| 2   | $ touch .prettierrc | config 파일 생성 |

.prettierrc

```json
{
  "semi": true,
  "trailingComma": "none",
  "singleQuote": true,
  "printWidth": 80
}
```

| no  | 구분                                             | 설명                      |
| --- | ------------------------------------------------ | ------------------------- |
| 3   | [here](https://prettier.io/docs/en/options.html) | options 참조              |
| 4   | $ touch package.json                             | prettier-format 명령 설정 |

package.json

``` json
{
  "scripts": {
    ...
    "prettier-format": "prettier --config .prettierrc 'src/**/*.ts' --write"
  }
}
```

| no  | 구분                      | 설명              |
| --- | ------------------------- | ----------------- |
| 5   | $ npm run prettier-format | src/index.ts 체크 |

```sh
➜  typescript-eslint-prettier (main) ✗ npm run prettier-format

> typescript-eslint-prettier@1.0.0 prettier-format
> prettier --config .prettierrc 'src/**/*.ts' --write

src/index.ts 235ms
➜  typescript-eslint-prettier (main) ✗
```

| no  | 구분                 | 설명 |
| --- | -------------------- | ---- |
| 6   | $ touch src/index.ts | 확인 |

src/index.ts

- prettierrc 설정에 따라 ; 이자동으로 붙는다.

```tsx
console.log('Hello world!');
```

## ESLint로 작동하도록 Prettier 를 config

| no  | 구분                                                     | 설명                                                          |
| --- | -------------------------------------------------------- | ------------------------------------------------------------- |
| 7   | $ npm i -D eslint-config-prettier eslint-plugin-prettier |                                                               |
|     | eslint-config-prettier                                   | Prettier rules 에 간섭하는 모든 ESLint rules 를 turn off 한다 |
|     | els lint-plugin-prettier                                 | Prettier rules 를 ESLint rules 로 전환한다.                   |
| 8   | $ touch .eslintrc                                        | 업데이트                                                      |

.eslintrc

```json
{
  "root": true,
  "parser": "@typescript-eslint/parser",
  "plugins": ["@typescript-eslint", "prettier"],
  "extends": [
    "eslint:recommended",
    "plugin:@typescript-eslint/eslint-recommended",
    "plugin:@typescript-eslint/recommended",
    "prettier"
  ],
  "rules": {
    "no-console": 1, // Means warning
    "prettier/prettier": 2 // Means error
  }
}
```

| no  | 구분           | 설명              |
| --- | -------------- | ----------------- |
| 9   | $ npm run lint | src/index.ts 체크 |

```sh
➜  typescript-eslint-prettier (main) ✗ npm run lint

> typescript-eslint-prettier@1.0.0 lint
> eslint . --ext .ts


/Users/catchhub/forme_c/typescript-eslint-prettier/src/index.ts
  1:1   warning  Unexpected console statement  no-console
  1:29  error    Insert `⏎`                    prettier/prettier

✖ 2 problems (1 error, 1 warning)
  1 error and 0 warnings potentially fixable with the `--fix` option.

➜  typescript-eslint-prettier (main) ✗
```

| no  | 구분                      | 설명        |
| --- | ------------------------- | ----------- |
| 10  | $ npm run prettier-format | format 실행 |

src/index.ts

```tsx
console.log('Hello world!');
```

## (참조) VSCode extension 활용하기

<u>**셋업**</u>

| no  | 구분                                     | 설명     |
| --- | ---------------------------------------- | -------- |
| 1   | CMD + SHIFT + P                          |          |
|     | preferences open settings                | 검색창   |
|     | Preferences: Open Default Settings(JSON) | 선택     |
|     | settings.json                            | 업데이트 |

settings.json

- 다음 내용을 추가
- 새로운 code 를 paste 할 때와 prettier가 이해하는 어떠한 파일 확장자에 대한 코드를 save 할 때, 이러한 setting으로 당신의 코드를 format 한다.

```json
...
"editor.formatOnPaste": true,
"editor.formatOnSave": true,
....
```

예제: pase 또는 save 할 때, TypeScript 코드를 format 하지 말고, 다른 언어에 대해서는 pase 또는 save 할 때, 해당 코드를 format 하려는 경우에는 settings.json 에서 다음과 같이 작성한다.

settings.json

```json
"[typescript]": {
  "editor.formatOnPaste": false,
  "editor.formatOnSave": false,
},
"editor.formatOnPaste": true,
"editor.formatOnSave": true,
```

**<u>(참조) commit 하기전에 format 하기</u>**

개발자들이 서로 다른 editor 를 사용하고, Prettier VS Code extension 을 모두가 사용하지 않고 있는 경우에는 [Husky Pre-commit Hooks 로 coding conventions](https://khalilstemmler.com/blogs/tooling/enforcing-husky-precommit-hooks/) 를 강제할 수 있다.

**<u>(참조) an filesystem watcher 를 사용하여 format 하기</u>**

onchange 패키지를 설치하여, 당신의 소스 코드에 변경이 발생할 때 filesystem을 watch 한 다음, 변경된 파일들에 대하여 Prettier CLI 을 실행한다.

| no     | 구분                 | 설명          |
| ------ | -------------------- | ------------- |
| 참조 1 | $ npm i -D onchange  | onchange 설치 |
| 참조 2 | $ touch package.json |               |

package.json

- npm run prettier-watch 명령을 실행하면, src 폴더에서 코드를 편집하면, 마치 VSCode로 Prettier 를 설치한 것과 같이 동일한 observable outcome 을 제공한다.

```json
"scripts": {
  "prettier-watch": "onchange 'src/**/*.ts' -- prettier --write {{changed}}"
  }
```
