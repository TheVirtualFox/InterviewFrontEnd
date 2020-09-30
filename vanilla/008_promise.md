```javascript
function loadScript(src, callback) {
  let script = document.createElement('script');
  script.src = src;
  script.onload = () => callback(null, script);
  script.onerror = () => callback(new Error(`Не удалось загрузить скрипт ${src}`));
  document.head.append(script);
}
```
```javascript
loadScript('/my/script.js', function(error, script) {
  if (error) {
    // обрабатываем ошибку
  } else {
    // скрипт успешно загружен
  }
});
```
```javascript
loadScript('1.js', function(error, script) {
  if (error) {
    handleError(error);
  } else {
    // ...
    loadScript('2.js', function(error, script) {
      if (error) {
        handleError(error);
      } else {
        // ...
        loadScript('3.js', function(error, script) {
          if (error) {
            handleError(error);
          } else {
            // ...и так далее, пока все скрипты не будут загружены (*)
          }
        });

      }
    })
  }
});
```

начале "pending" («ожидание»), потом меняется на "fulfilled" («выполнено успешно») при вызове resolve или на "rejected" («выполнено с ошибкой») при вызове reject.

```javascript
let promise = new Promise(function(resolve, reject)) {
  setTimeout(() => resolve("done"), 1000);
});
```

```javascript
let promise = new Promise(function(resolve, reject)) {
  setTimeout(() => reject(new Error("Err")), 1000);
});
```

```javascript
let promise = new Promise(function(resolve, reject) {
  resolve("done");
  reject(new Error("…")); // игнорируется
  setTimeout(() => resolve("…")); // игнорируется
});
```
```javascript
promise.then(
  function(result) { /* обработает успешное выполнение */ },
  function(error) { /* обработает ошибку */ }
);

promise.catch(alert);
```

Обработчик, вызываемый из finally, не имеет аргументов. В finally мы не знаем, как был завершён промис. И это нормально, потому что обычно наша задача – выполнить «общие» завершающие процедуры.

Обработчик finally «пропускает» результат или ошибку дальше, к последующим обработчикам.

Например, здесь результат проходит через finally к then:

```javascript
new Promise((resolve, reject) => {
  setTimeout(() => resolve("result"), 2000)
})
  .finally(() => alert("Промис завершён"))
  .then(result => alert(result)); // <-- .then обработает результат


  new Promise((resolve, reject) => {
  throw new Error("error");
})
  .finally(() => alert("Промис завершён"))
  .catch(err => alert(err));  // <-- .catch обработает объект ошибки
```

```javascript
function loadScript(src) {
  return new Promise(function(resolve, reject) {
    let script = document.createElement('script');
    script.src = src;

    script.onload = () => resolve(script);
    script.onerror = () => reject(new Error(`Ошибка загрузки скрипта ${src}`));

    document.head.append(script);
  });
}
let promise = loadScript("https://cdnjs.cloudflare.com/ajax/libs/lodash.js/4.17.11/lodash.js");

promise.then(
  script => alert(`${script.src} загружен!`),
  error => alert(`Ошибка: ${error.message}`)
);

promise.then(script => alert('Ещё один обработчик...'));
```






```javascript
new Promise(function(resolve, reject) {

  setTimeout(() => resolve(1), 1000); // (*)

}).then(function(result) { // (**)

  alert(result); // 1
  return result * 2;

}).then(function(result) { // (***)

  alert(result); // 2
  return result * 2;

}).then(function(result) {

  alert(result); // 4
  return result * 2;

});
```



```javascript
new Promise(function(resolve, reject) {

  setTimeout(() => resolve(1), 1000);

}).then(function(result) {

  alert(result); // 1

  return new Promise((resolve, reject) => { // (*)
    setTimeout(() => resolve(result * 2), 1000);
  });

}).then(function(result) { // (**)

  alert(result); // 2

  return new Promise((resolve, reject) => {
    setTimeout(() => resolve(result * 2), 1000);
  });

}).then(function(result) {

  alert(result); // 4

});
```

```javascript
loadScript("/article/promise-chaining/one.js")
  .then(script => loadScript("/article/promise-chaining/two.js"))
  .then(script => loadScript("/article/promise-chaining/three.js"))
  .then(script => {
    // скрипты загружены, мы можем использовать объявленные в них функции
    one();
    two();
    three();
  });
```



```javascript
// catch
fetch('/article/promise-chaining/user.json')
  .then(response => response.json())
  .then(user => fetch(`https://api.github.com/users/${user.name}`))
  .then(response => response.json())
  .then(githubUser => new Promise((resolve, reject) => {
    let img = document.createElement('img');
    img.src = githubUser.avatar_url;
    img.className = "promise-avatar-example";
    document.body.append(img);

    setTimeout(() => {
      img.remove();
      resolve(githubUser);
    }, 3000);
  }))
  .catch(error => alert(error.message));
```




```javascript
new Promise((resolve, reject) => {
  throw new Error("Ошибка!");
}).catch(alert); // Error: Ошибка!
…Работает так же, как и этот:

new Promise((resolve, reject) => {
  reject(new Error("Ошибка!"));
}).catch(alert); // Error: Ошибка!
```
Это работает не только в функции промиса, но и в обработчиках. Если мы бросим ошибку (throw) из обработчика (.then), то промис будет считаться отклонённым, и управление перейдёт к ближайшему обработчику ошибок.

Пример:
```javascript
new Promise((resolve, reject) => {
  resolve("ок");
}).then((result) => {
  throw new Error("Ошибка!"); // генерируем ошибку
}).catch(alert); // Error: Ошибка!
```






```javascript
// the execution: catch -> then
new Promise((resolve, reject) => {

  throw new Error("Ошибка!");

}).catch(function(error) {

  alert("Ошибка обработана, продолжить работу");

}).then(() => alert("Управление перейдёт в следующий then"));
```




```javascript
// the execution: catch -> catch -> then
new Promise((resolve, reject) => {

  throw new Error("Ошибка!");

}).catch(function(error) { // (*)

  if (error instanceof URIError) {
    // обрабатываем ошибку
  } else {
    alert("Не могу обработать ошибку");

    throw error; // пробрасывает эту или другую ошибку в следующий catch
  }

}).then(function() {
  /* не выполнится */
}).catch(error => { // (**)

  alert(`Неизвестная ошибка: ${error}`);
  // ничего не возвращаем => выполнение продолжается в нормальном режиме

});
```







```javascript
window.addEventListener('unhandledrejection', function(event) {
  // объект события имеет два специальных свойства:
  alert(event.promise); // [object Promise] - промис, который сгенерировал ошибку
  alert(event.reason); // Error: Ошибка! - объект ошибки, которая не была обработана
});

new Promise(function() {
  throw new Error("Ошибка!");
}); // нет обработчика ошибок
```






```javascript
Promise.all([
  new Promise(resolve => setTimeout(() => resolve(1), 3000)), // 1
  new Promise(resolve => setTimeout(() => resolve(2), 2000)), // 2
  new Promise(resolve => setTimeout(() => resolve(3), 1000))  // 3
]).then(alert); // когда все промисы выполнятся, результат будет 1,2,3
// каждый промис даёт элемент массива
```





```javascript
let urls = [
  'https://api.github.com/users/iliakan',
  'https://api.github.com/users/remy',
  'https://api.github.com/users/jeresig'
];

// Преобразуем каждый URL в промис, возвращённый fetch
let requests = urls.map(url => fetch(url));

// Promise.all будет ожидать выполнения всех промисов
Promise.all(requests)
  .then(responses => responses.forEach(
    response => alert(`${response.url}: ${response.status}`)
  ));
```







```javascript
  let names = ['iliakan', 'remy', 'jeresig'];

let requests = names.map(name => fetch(`https://api.github.com/users/${name}`));

Promise.all(requests)
  .then(responses => {
    // все промисы успешно завершены
    for(let response of responses) {
      alert(`${response.url}: ${response.status}`); // покажет 200 для каждой ссылки
    }

    return responses;
  })
  // преобразовать массив ответов response в response.json(),
  // чтобы прочитать содержимое каждого
  .then(responses => Promise.all(responses.map(r => r.json())))
  // все JSON-ответы обработаны, users - массив с результатами
  .then(users => users.forEach(user => alert(user.name)));
```



```javascript
Promise.all([
  new Promise((resolve, reject) => setTimeout(() => resolve(1), 1000)),
  new Promise((resolve, reject) => setTimeout(() => reject(new Error("Ошибка!")), 2000)),
  new Promise((resolve, reject) => setTimeout(() => resolve(3), 3000))
]).catch(alert); // Error: Ошибка!
```


```javascript
  Promise.all([
  new Promise((resolve, reject) => setTimeout(() => resolve(1), 1000)),
  new Promise((resolve, reject) => setTimeout(() => reject(new Error("Ошибка!")), 2000)),
  new Promise((resolve, reject) => setTimeout(() => resolve(3), 3000))
]).catch(alert); // Error: Ошибка!
```







```javascript
let urls = [
  'https://api.github.com/users/iliakan',
  'https://api.github.com/users/remy',
  'https://no-such-url'
];

Promise.allSettled(urls.map(url => fetch(url)))
  .then(results => { // (*)
    results.forEach((result, num) => {
      if (result.status == "fulfilled") {
        alert(`${urls[num]}: ${result.value.status}`);
      }
      if (result.status == "rejected") {
        alert(`${urls[num]}: ${result.reason}`);
      }
    });
  });
```







```javascript
// first result
  Promise.race([
  new Promise((resolve, reject) => setTimeout(() => resolve(1), 1000)),
  new Promise((resolve, reject) => setTimeout(() => reject(new Error("Ошибка!")), 2000)),
  new Promise((resolve, reject) => setTimeout(() => resolve(3), 3000))
]).then(alert); // 1
```








```javascript
let cache = new Map();

function loadCached(url) {
  if (cache.has(url)) {
    return Promise.resolve(cache.get(url)); // (*)
  }

  return fetch(url)
    .then(response => response.text())
    .then(text => {
      cache.set(url,text);
      return text;
    });
}
```


```javascript
let promise = Promise.resolve();

promise.then(() => alert("промис выполнен"));

alert("код выполнен"); // этот alert показывается первым
```





```javascript
let promise = Promise.reject(new Error("Ошибка в промисе!"));

setTimeout(() => promise.catch(err => alert('поймана')), 1000);

// Ошибка в промисе!
window.addEventListener('unhandledrejection', event => alert(event.reason));
```
Теперь, при запуске, мы сначала увидим «Ошибка в промисе!», а затем «поймана».

Если бы мы не знали про очередь микрозадач, то могли бы удивиться: «Почему сработал обработчик unhandledrejection? Мы же поймали ошибку!».