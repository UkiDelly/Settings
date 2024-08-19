# NestJS 세팅

## NestJS CLI 설치

nestjs로 개발할때는 CLI를 사용하면 편하게 개발할수 있다.

`node` 및 `npm` 환경일때는 다음 명령어를 사용하면 설치할수 있다.
`sudo npm i -g @nestjs/cli`

- `i`는 install의 약자
- `-g`는 글로벌 설치

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
    'prettier/prettier': [
      'warn',
      {
        endOfLine: 'auto',
        printWidth: 120,
        overrides: [
          {
            files: '*.ts',
            options: {
              importOrder: ['^[./]'],
              importOrderSeparation: true,
              importOrderSortSpecifiers: true,
              printWidth: 250, // import 문에 대해서는 줄바꿈을 하지 않도록 설정
            },
          },
        ],
      },
    ],
    '@typescript-eslint/interface-name-prefix': 'off',
    '@typescript-eslint/explicit-function-return-type': 'off',
    '@typescript-eslint/explicit-module-boundary-types': 'off',
    '@typescript-eslint/no-explicit-any': 'off',
    '@typescript-eslint/no-unused-vars': 'warn',
    // thenable을 사용할때 await를 사용하라
    '@typescript-eslint/await-thenable': 'error',
    // 불필요한 생성자 파라미터 할당을 금지
    // '@typescript-eslint/no-unnecessary-parameter-property-assignment': 'error',
    // for of 사용
    '@typescript-eslint/prefer-for-of': 'error',
    // 함수타입을 정의할대 interface 대신 type을 사용하라
    '@typescript-eslint/prefer-function-type': 'error',
    // param이 여러개 일때 자동 줄바꿈 off
    // '@typescript-eslint/indent': 'off',
  },
};
```

### prettier 설정하기

```json
{
  "singleQuote": true,
  "trailingComma": "es5",
  "arrowParens": "avoid",
  "printWidth": 120
}
```

## 프로젝트 생성하기

nestjs cli를 사용하면 프로젝트를 간편하게 시작할수 있다.

`nest new {프로젝트 이름} [.]`

`.`는 옵션으로, `.`을 사용하면 현재 폴더에 프로젝트가 생성되고, `.` 없이 이름만 입력하면 `{프로젝트 이름}`의 폴더가 생성된다.

## 꼭 설치해야하는 패키지 목록

1. class-validator, class-transformer
2. @nestjs/swagger
3. @nestjs/typeorm, typeorm, [사용하는 db 드라이버]
