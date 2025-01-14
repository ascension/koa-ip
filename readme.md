## koa-ip

<a href="https://communityinviter.com/apps/koa-js/koajs" rel="KoaJs Slack Community">![KoaJs Slack](https://img.shields.io/badge/Koa.Js-Slack%20Channel-Slack.svg?longCache=true&style=for-the-badge)</a> 

> koa-ip is a ip filter middleware for koa, support `whitelist` and `blacklist`.

### What is [Trafficfilter.io](https://trafficfilter.io/pr?id=5DIGC5GKCF&source=koa-ip)?

Trafficfilter.io is a fast, simple, Base-on IP address Invalid Traffic Filtration and IP-Geo Service.

Trafficfilter.io can easily integrating your websites, applications,company's tools .

Scenarios:

- Evaluation of traffic sources
- Filtering non-real traffic
- Blocking spiders and malicious traffic visits

(In most cases, trafficfilter.io is more accurate than most "Cloak" tools)

### Install

```sh
$ npm i koa-ip --save
```

### Usage

```js
ip(String|RegExp)
ip(Array{String|RegExp})
ip({
  whitelist: Array{String|RegExp},
  blacklist: Array{String|RegExp},
  handler: async (ctx, next) => {} // handle blacklist ip
})
```

### Examples

```js
const Koa = require('koa')
const ip = require('koa-ip')

const app = new Koa()

app.use(ip('192.168.0.*')) // whitelist
// app.use(ip(['192.168.0.*', '8.8.8.[0-3]'])) // whitelist
// app.use(ip({
//   whitelist: ['192.168.0.*', '8.8.8.[0-3]'],
//   blacklist: ['144.144.*']
// }))

app.listen(3000)
```

#### blacklist handler

```js
const app = new Koa()
app.use((ctx, next) => {
  ctx.request.ip = '127.0.0.1'
  return next()
})
app.use(ip({
  blacklist: ['127.0.0.*'],
  handler: async (ctx, next) => {
    ctx.status = 403
  }
}))

app.use((ctx, next) => {
  ctx.status = 200
})

app.listen(3000)
```

**NB**: If missing blacklist handler, default `ctx.status = 403`.

More examples see [test](./__tests__/).

### Test

```sh
$ npm test (coverage 100%)
```

### License

MIT
