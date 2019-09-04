# egg-apollo

[![NPM version][npm-image]][npm-url]
[![build status][travis-image]][travis-url]
[![Test coverage][codecov-image]][codecov-url]
[![David deps][david-image]][david-url]
[![Known Vulnerabilities][snyk-image]][snyk-url]
[![npm download][download-image]][download-url]

[npm-image]: https://img.shields.io/npm/v/egg-apollo.svg?style=flat-square
[npm-url]: https://npmjs.org/package/egg-apollo
[travis-image]: https://img.shields.io/travis/eggjs/egg-apollo.svg?style=flat-square
[travis-url]: https://travis-ci.org/eggjs/egg-apollo
[codecov-image]: https://img.shields.io/codecov/c/github/eggjs/egg-apollo.svg?style=flat-square
[codecov-url]: https://codecov.io/github/eggjs/egg-apollo?branch=master
[david-image]: https://img.shields.io/david/eggjs/egg-apollo.svg?style=flat-square
[david-url]: https://david-dm.org/eggjs/egg-apollo
[snyk-image]: https://snyk.io/test/npm/egg-apollo/badge.svg?style=flat-square
[snyk-url]: https://snyk.io/test/npm/egg-apollo
[download-image]: https://img.shields.io/npm/dm/egg-apollo.svg?style=flat-square
[download-url]: https://npmjs.org/package/egg-apollo

## 使用场景

用于接入 apollo 配置中心，基于[ctrip-apollo](https://github.com/kaelzhang/ctrip-apollo)扩展，默认可以通过像获取 egg 配置一样的调用方式直接获取 apollo 配置，也可以通过`app.apollo`来获取 apollo 实例，自行调用 api 获取配置。详见后续说明。

## 依赖说明

### 依赖的 egg 版本

| egg-apollo 版本 | egg 1.x |
| --------------- | ------- |
| 1.x             | 😁      |
| 0.x             | ❌      |

## 开启插件

```js
// config/plugin.js
exports.apollo = {
  enable: true,
  package: '@zijin/egg-apollo'
};
```

## 详细配置

```
  host: '', // 配置中心地址
  appId: '', // appId
  cluster: 'default', // 默认集群
  namespaces: [ 'application' ], // 默认命名空间
  cachePath: '/tmp/apollo_cache', // 默认缓存目录
  enableUpdateNotification: true, // 默认开启推送更新
  enableFetch: true, // 默认开启定时拉取
  fetchInterval: 5 * 60 * 1000, // 定时拉取间隔
  retry: 10, // 初始化重试次数
  mergeNamespace: '', // 将特定namespace合入本地配置 默认不做
  mountConfig: true, // 将namespaces挂载到本地配置 默认开启
```

请到 [config/config.default.js](config/config.default.js) 查看详细配置项说明。

## 使用参考

当`apollo`的某个`appId`下的`application`命名空间配置如下时

| key    | value            |
| ------ | ---------------- |
| name   | apollo           |
| config | {"key": "value"} |
| a.b    | 1                |
| a.c.d  | 2                |

通过`app.conig`直接获取配置信息。

```js
console.log(app.config.application);
// {
//   name: "apollo",
//   config: {
//     key: "value"
//   },
//   a: {
//     b: 1,
//     c: {
//       d: 2
//     }
//   }
// }
```

通过这种方式获得配置数据会默认进行 key 的对象合并和 value 的 JSON 格式化，所以推荐使用这种方式,具体的合并方式可以参考[properties]("https://github.com/gagle/node-properties#namespaces")对 namespaces 合并的规则，但是一般来说不推荐使用 key 合并，而是直接把 value 设置为 JSON 对象更好。

第二种是直接通过`app.apollo`获取，这种获取方式拿到的配置不会对数据进行任何处理，所以得到的是原始字符串。

```js
app.apollo.namespace().config(); // 获取默认namespace下的全部配置
app.apollo.namespace().get(key); // 获取默认namespace下的某个配置
```

## 提问交流

请到 [egg issues](https://github.com/eggjs/egg/issues) 异步交流。

## License

[MIT](LICENSE)
