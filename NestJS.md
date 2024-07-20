# NestJS 세팅

## NestJS CLI 설치

nestjs로 개발할때는 CLI를 사용하면 편하게 개발할수 있다.

`node` 및 `npm` 환경일때는 다음 명령어를 사용하면 설치할수 있다.
`sudo npm i -g @nestjs/cli`

- `i`는 install의 약자
- `-g`는 글러볼 설치

`Bun`에서는 다음과 같은 명령어로 설치할수 있다.
`sudo bun i -g @nestjs/cli`

### eslint 설정하기

```javascript
module.exports = {
  parser: '@typescript-eslint/parser',
  parserOptions: {
    project: 'tsconfig.json',
    tsconfigRootDir: __dirname,
    sourceType: 'module',
  },
  plugins: ['@typescript-eslint/eslint-plugin'],
  extends: [
    'plugin:@typescript-eslint/recommended',
    'plugin:prettier/recommended',
  ],
  root: true,
  env: {
    node: true,
    jest: true,
  },
  ignorePatterns: ['.eslintrc.js'],
  rules: {
    '@typescript-eslint/interface-name-prefix': 'off',
    '@typescript-eslint/explicit-function-return-type': 'off',
    '@typescript-eslint/explicit-module-boundary-types': 'off',
    '@typescript-eslint/no-explicit-any': 'off',
    eqeqeq: [2, 'allow-null'], // == 금지
    'brace-style': [2, '1tbs', { allowSingleLine: true }], // 중괄호 스타일
    'function-paren-newline': ['error', 'consistent'], // 함수의 인자가 여러줄일 경우, 첫번째 인자는 첫줄에, 나머지는 각각 한줄씩
    "object-property-newline": ["error", { "allowAllPropertiesOnSameLine": false }], // 객체의 프로퍼티가 여러줄일 경우, 첫번째 프로퍼티는 첫줄에, 나머지는 각각 한줄씩
  },
};
```

## 프로젝트 생성하기

nestjs cli를 사용하면 프로젝트를 간편하게 시작할수 있다.

`nest new {프로젝트 이름} [.]`

`.`는 옵션으로, `.`을 사용하면 현재 폴더에 프로젝트가 생성되고, `.` 없이 이름만 입력하면 `{프로젝트 이름}`의 폴더가 생성된다.
