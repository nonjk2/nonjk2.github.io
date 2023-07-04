---
layout: post
title: Eslint 와 Prettier
category: Study
description: >
  Eslint와 Prettier 정리
tags: other TIL
image:
  path: /assets/img/TIL.png
permalink: Study/ComputerScience/eslintPrettier
---
<!--more-->

# Eslint 와 Prettier
{:.no_toc}

* this list will be replaced by the toc
{:toc}


## Eslint

ESLint의 이름은 "ES"와 "Lint" 를 합친것이다.

"ES"는 "`ECMAScript`"의 준말로, `JavaScript` 언어의 표준 사양. `ECMAScript`는 `JavaScript`의 스크립팅 언어로서의 기본 사양을 정의하는 국제 표준이라고한단다.

"Lint"는 소프트웨어 개발에서 원래 "lint"라는 도구의 이름에서 유래한 용어. 초기에는 C 언어의 코드 문제를 체크하기 위해 사용되었단다. "Lint"는 코드의 오류, 잠재적인 버그, 스타일 규칙 위반 등을 식별해서 사용자에게 경고하기위한 도구를 말한다.

따라서 "ESLint"는 `JavaScript` 의 코드의 문제를 식별하고 보완하는 린트 도구를 의미한다. 이는 `JavaScript` 코드를 분석하여 오류를 찾고, 스타일 가이드에 맞지 않는 부분을 경고하며, 코드의 품질을 향상시키는 엄청난 도움을 준다.

## Prettier

그렇다면 같이 쓰는`Prettier`는 뭘까

프리티어(`Prettier`)는 코드 포맷터(code formatter)로 사용되는 도구다. 얘는 코드의 형식을 일관성 있게 정리하고, 들여쓰기, 줄 바꿈, 공백 등의 스타일 규칙을 자동으로 적용해 코드를 더 가독성 있게 만들어 준다.

프리티어는 다양한 프로그래밍 언어를 지원한다. 코드를 수정하거나 저장할 때 자동으로 동작하도록 설정할 수 있어, 사용자는 코드 스타일을 일일이 수동으로 정리할 필요 없이 프리티어를 사용하여 코드를 깰꼼하게 정리 할 수 있다.

ESLint와 프리티어는 서로 비슷한거 같지만 완전 다르다. ESLint는 코드의 문제를 식별하고 보완하여 코드 품질을 향상시키는 린트 도구이고, 프리티어는 코드의 형식을 일관성 있게 정리하여 가독성을 높이는 코드 포맷터다.

그래서 서로 같이 사용하는경우가 많다.

## 사용

```shell
npm i -D eslint eslint-config-airbnb eslint-config-prettier eslint-plugin-import eslint-plugin-jsx-a11y eslint-plugin-prettier eslint-plugin-react eslint-plugin-react-hooks
```

1. `eslint-config-airbnb`: Airbnb의 `JavaScript` 스타일 가이드를 ESLint에 적용시키는 설정이다. Airbnb의 스타일 가이드는 매우 인기 있으며, 많은 회사와 개발자들이 쓴다.

2. `eslint-config-prettier`: Prettier와 충돌하는 ESLint 규칙을 비활성화하는 설정이다. Prettier는 코드 포매팅 도구로, 코드를 일관되게 보기 좋게 만드는 데 사용한다. 이 설정을 사용하면 ESLint와 Prettier가 서로 충돌하는 것을 방지할 수 있다.

3. `eslint-plugin-import`: `import`/`export` 구문과 관련된 ESLint 규칙을 제공하는 플러그인이다. 이를 사용하면 `import`/`export` 구문의 문제를 더 잘 찾아낼 수 있다.

4. `eslint-plugin-jsx-a11y`: JSX 요소에 대한 접근성 문제를 찾아내는 ESLint 규칙을 제공하는 플러그인이다.

5. `eslint-plugin-prettier`: Prettier를 ESLint에서 실행하게 해주는 플러그인이다.

6. `eslint-plugin-react`: React와 JSX에 대한 ESLint 규칙을 제공하는 플러그인이다.

7. `eslint-plugin-react-hooks`: React Hooks의 규칙을 강제하는 ESLint 플러그인이다.

`.prettierrc.`

```json
{
  "singleQuote": true,
  "semi": true,
  "useTabs": false,
  "tabWidth": 2,
  "trailingComma": "all",
  "printWidth": 80,
  "arrowParens": "always",
  "orderedImports": true,
  "bracketSpacing": true,
  "jsxBracketSameLine": false
}
```

`.eslintrc.js`

```js
module.exports = {
  parser: "@typescript-eslint/parser", // 코드를 파싱하기 위해 사용할 파서를 정의
  plugins: ["@typescript-eslint", "prettier"], //사용할 플러그인의 목록 TypeScript 관련 ESLint 규칙을 제공하는 @typescript-eslint와 Prettier를 ESLint에 연결하는 prettier 사용
  extends: [
    //여러 플러그인들의 추천방향과 유명한 airbnb플러그인 사용
    "airbnb", // or airbnb-base
    "plugin:react/recommended",
    "plugin:jsx-a11y/recommended", // 설치 한경우
    "plugin:import/errors", // 설치한 경우
    "plugin:import/warnings", // 설치한 경우
    "plugin:@typescript-eslint/recommended",
    "plugin:prettier/recommended",
  ],
  rules: {
    //ESLint 규칙을 덮어씌우거나 추가하는곳. 숫자는 규칙의 엄격성을 의미하고, 0은 규칙을 끄는 것, 1은 경고만 표시하고, 2는 오류를 반환
    "linebreak-style": 0,
    "import/prefer-default-export": 0,
    "import/extensions": 0,
    "no-use-before-define": 0,
    "import/no-unresolved": 0,
    "react/react-in-jsx-scope": 0,
    "import/no-extraneous-dependencies": 0,
    // 테스트 또는 개발환경을 구성하는 파일에서는 devDependency 사용을 허용
    "no-shadow": 0,
    "react/prop-types": 0,
    "react/jsx-filename-extension": [2, { extensions: [".js", ".jsx", ".ts", ".tsx"] }],
    "jsx-a11y/no-noninteractive-element-interactions": 0,
    "@typescript-eslint/explicit-module-boundary-types": 0,
  },
  settings: {
    //: ESLint 설정의 추가 정보를 제공하는 곳. 여기서는 import/resolver 설정을 통해 다양한 파일 확장자를 지원하도록 설정.
    "import/resolver": {
      node: {
        extensions: [".js", ".jsx", ".ts", ".tsx"],
      },
    },
  },
};
```

많이 쓰는 eslint 규칙

1. `"indent": [2, 2]`: 들여쓰기를 2 스페이스로 강제.

2. `"quotes": [2, "single"]`: 문자열을 작성할 때 따옴표 대신 홑따옴표를 사용하도록 강제.

3. `"semi": [2, "always"]`: 항상 세미콜론을 사용하도록 강제.

4. `"no-unused-vars": 2`: 사용되지 않는 변수가 있을 경우 오류를 반환.

5. `"no-console": 2`: console.log() 등 콘솔 사용을 금지. 개발 중에는 유용하지만, 배포 버전의 코드에는 남아있지 않아야함 .

6. `"eqeqeq": 2`: 등호를 사용하는 대신 항상 삼중 등호를 사용하도록 강제. 이는 예기치 않은 타입 변환을 방지.

`tsconfig.json`
