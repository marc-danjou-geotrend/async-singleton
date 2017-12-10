# async-singleton

make async/promise function execute only one at a time.

- https://github.com/shokai/async-singleton
- https://www.npmjs.com/package/async-singleton

[![CircleCI](https://circleci.com/gh/shokai/async-singleton.svg?style=svg)](https://circleci.com/gh/shokai/async-singleton)


## Usage

```js
const asyncSingleton = require('async-singleton')

const delay = msec => new Promise(resolve => setTimeout(resolve, msec))

async function countUp (n = 0) {
  for (let i = 0; i < 5; i++) {
    console.log(n + i)
    await delay(100)
  }
}
```

```js
const singleCountUp = asyncSingleton(countUp)

singleCountUp(0) // run
singleCountUp(10) // skip this
await singleCountUp(20) // skip this

singleCountUp(30) // run this
```

result

```
0
1
2
3
4
30
31
32
33
34
```

### trailing

When the function being executed is finished, it is executed only once at the last.


```js
const singleCountUp = asyncSingleton(countUp, {trailing: true})

singleCountUp(0) // run
singleCountUp(10) // skip
singleCountUp(20) // skip
singleCountUp(30) // run after "sinleCountUp(0)"
```

result

```
0
1
2
3
4
30
31
32
33
34
```