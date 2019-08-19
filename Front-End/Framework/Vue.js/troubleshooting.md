# Vue.js 사용중 발생한 문제들에 대한 해결 방안 메모

<br/>

## Unexpected console statement (no-console)

console.log() 사용시 발생.

`package.json`

```json
  ...
  "eslintConfig": {
    ...
    "rules": {
      "no-console": "off"
    },
    ...
  },
  ...
```

<br/>

## 2 warnings potentially fixable with the `--fix` option.

```sh
PS C:\DEV\Vue.js-Example\vue-todo-list-example> .\node_modules\.bin\eslint src --fix
PS C:\DEV\Vue.js-Example\vue-todo-list-example> npm run serve
```
