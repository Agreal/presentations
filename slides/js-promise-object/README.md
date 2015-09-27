# Promise Object



## Agenda
1. 解决什么问题
1. 怎么使用
1. Angular.js 的 $q



## Promise Object
用来进行延迟（deferred）和异步（asynchronous）计算


ECMAScript 2015（ES6）规范

[MDN - Promise Object](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise)


### 回调地狱（callback hell）

```javascript
doAsync1(function () {
  doAsync2(function () {
    doAsync3(function () {
      doAsync4(function () {
    })
  })
})
```


* 无法使用 ```return``` 和 ```throw``` 关键词
* 无法查看堆栈信息


### Promise 的意义在于

给回了在函数式语言里面异步时所丢失的

```return```，```throw``` 和 堆栈


Example
```javascript
 searchTwitter('#IE10', function (data1) {
     searchTwitter('#IE9', function (data2) {
         /* Reshuffle due to date */
         var totalResults = concatResults(data1.results, data2.results);
         totalResults.forEach(function (tweet) {
             var el = document.createElement('li');
             el.innerText = tweet.text;
             container.appendChild(el);
         });
     }, handleError);
 }, handleError);
```


使用 Promise
```javascript
searchTwitter(term)
	.then(filterResults)
	.then(displayResults);
```



### Promise 进阶


有三种状态

* *pending*: 初始状态, 非 *fulfilled* 或 *rejected*
* *fulfilled*: 成功的操作
* *rejected*: 失败的操作


![promises](promises.png)



## Promise in Angular.js


### $q

是 promises/deferred objects 的一个实现

灵感来源于 *Kris Kowal* [Q.js](https://github.com/kriskowal/q)


```javascript
function asyncGreet(name) {
  var deferred = $q.defer();

  setTimeout(function() {
    deferred.notify('About to greet ' + name + '.');

    if (okToGreet(name)) {
      deferred.resolve('Hello, ' + name + '!');
    } else {
      deferred.reject('Greeting ' + name + ' is not allowed.');
    }
  }, 1000);

  return deferred.promise;
}

var promise = asyncGreet('Robin Hood');
promise.then(
  function(greeting) {
    alert('Success: ' + greeting);
  },
  function(reason) {
    alert('Failed: ' + reason);
  },
  function(update) {
    alert('Got notification: ' + update);
  }
);
```

Note:
1. Defer对象代表 异步执行体
2. Promise对象代表 回调执行体


### Deferred API

```javascript
$q.defer();

resolve(value);

reject(reason);

notify(value);

deferred.promise;
```


### Promise API

```javascript
then(successCallback, errorCallback, notifyCallback)

catch(errorCallback)

finally(callback, notifyCallback)
```


### $q API

```javascript
$q(resolver);

defer();

reject(reason);

resolve(value, [successCallback], [errorCallback], [progressCallback]);
when(value, [successCallback], [errorCallback], [progressCallback]);

all(promises);
```



### [栗子](example.html)




### 参考
* [WIKI - Futures and promises](https://en.wikipedia.org/wiki/Futures_and_promises)
* [MDN - Promise](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Promise)
* [Callback Hell](http://callbackhell.com/)
* [Angular - $q](https://code.angularjs.org/1.4.6/docs/api/ng/service/$q)
* [Blog: Promises](http://blog.getify.com/promises-part-1/), [(翻译)](http://segmentfault.com/a/1190000000586666)
* [We have a problem with promises](http://pouchdb.com/2015/05/18/we-have-a-problem-with-promises.html), [(翻译)](http://web.jobbole.com/82601/)
* [Other Implementations](https://promisesaplus.com/implementations)
