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

<br/>

## npm run serve random port

- https://stackoverflow.com/questions/57536785/vue-npm-run-serve-starts-on-random-port

### Option1: Patching

In order to use the port 8080, I patched the file `node_modules/@vue/cli-service/lib/commands/serve.js` adding line 322 `portfinder.highestPort  = portfinder.basePort + 1`

```javascript
portfinder.basePort = args.port || process.env.PORT || projectDevServerOptions.port || defaults.port
portfinder.highestPort  = portfinder.basePort + 1
const port = await portfinder.getPortPromise()
```

### Option2: Install portfinder previous to the behaviour change

Another way to woraround waiting for portfinder/vue-cli to choose a solution is to install old portfinder with :

```sh
$ npm install portfinder@1.0.21
```

<br/>

## Parsing error: Unexpected token

### index.html

Parsing error: Unexpected token eslint(prettier/prettier) [1,2]

<!DOCTYPE html>


