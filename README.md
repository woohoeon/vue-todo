# vue-todo
I created a To-do apps with vue js.

## I used cli

### Create project

``` bash
vue init webpack-simple
```

### Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```

## Jest를 이용하여 테스트 환경 만들기

### Install jest

``` bash
npm install --save-dev jest
```

### Install vue-test-utils vue-jest
``` bash
npm install --save-dev vue-test-utils vue-jest
```

### package.json에 jest 설정 내용 추가
```
  "jest": {
    "moduleFileExtensions": [
      "js",
      "json",
      // tell Jest to handle *.vue files
      "vue"
    ],
    "transform": {
      // process *.vue files with vue-jest
      ".*\\.(vue)$": "<rootDir>/node_modules/vue-jest"
    },
    "mapCoverage": true
  }
```

### Handling webpack aliases

webpack config에서 resolve alias를 사용하고 있다면, Jest 에서도 설정해 주어야 합니다.

```
{
  // ...
  "jest": {
    // ...
    // support the same @ -> src alias mapping in source code
    "moduleNameMapper": {
      "^@/(.*)$": "<rootDir>/src/$1"
    }
  }
}
```

### Configuring Babel for Jest

babel을 사용하고 있다면, babel-jest도 설치가 필요합니다.

``` bash
npm install --save-dev babel-jest
```

기본적으로 babel-jest는 자동으로 설치된다고 합니다. 하지만, 우리가 *.vue를 추가 하면서 babel-jest을 명시적으로 추가해 주어야 합니다

```
{
  // ...
  "jest": {
    // ...
    "transform": {
      // ...
      // process js with babel-jest
      "^.+\\.js$": "<rootDir>/node_modules/babel-jest"
    },
    // ...
  }
}
```

.babelrc에 env.test를 설정합니다.

```
{
  "presets": [
    ["env", { "modules": false }]
  ],
  "env": {
    "test": {
      "presets": [
        ["env", { "targets": { "node": "current" }}]
      ]
    }
  }
}
```

async/await을 사용할 경우 plugins에 transform-runtime를 추가합니다.

```
{
  "plugins": [
    ["transform-runtime", {
      "polyfill": false,
      "regenerator": true
    }]
  ]
}
```

### Snapshot Testing

vue-server-renderer를 사용해서 Jest snapshot testing을 수행할 수 있습니다.

``` bash
npm install --save-dev jest-serializer-vue
```

그리고 package.json에 설정합니다.

```
{
  // ...
  "jest": {
    // ...
    // serializer for snapshots
    "snapshotSerializers": [
      "<rootDir>/node_modules/jest-serializer-vue"
    ]
  }
}
```