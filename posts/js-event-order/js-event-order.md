## 事件的冒泡与捕获

### 事件绑定

给元素绑定事件可以通过属性 `onclick`，也可以使用 `addEventListner`。`addEventListner` 被大多数主流浏览器及 IE9 之后的 IE 所支持，IE8及以下使用 `attachEvent`。

```js
element.onclick = function handler(e){...}
element.addEventListner('click', function handler(e){...})
element.attachEvent('click', function handler(e){...}) /*for IE<9*/
```

其中 `addEventListner` 第三个参数 `useCapture` 可以指定事件处理器在何时被执行，事件的捕获阶段（capture）还是冒泡阶段（bubble）。


### 事件发生的时机问题

先看个示例。

```js
window.addEventListener("click", function(){alert(1)}, false);
window.addEventListener("click", function(){alert(2)}, true);
window.addEventListener("click", function(){alert(3)}, false);
window.addEventListener("click", function(){alert(4)}, true);
```

执行结果是:

* `2`  事件捕获，先绑定
* `4`  事件捕获，后绑定
* `1`  事件冒泡，先绑定
* `3`  事件冒泡，后绑定

从上面可以看出，事件的触发顺序是先捕获，再冒泡，先绑定的先执行，即事件的整个传递流程为：

* 外层容器的捕获
* 内层元素的捕获
* 内层元素的冒泡
* 外层容器的冒泡

```
                  |  A
 -----------------|--|-----------------
 | Parent         |  |                |
 |   -------------|--|-----------     |
 |   |Children    V  |          |     |
 |   ----------------------------     |
 |                                    |
 --------------------------------------
```

任一过程中的事件处理器调用 `stopPropagation` 都会中断上面这一流程。

那么问题来了，当 onclick 闯入的时候，他与捕获冒泡的顺序是怎样的呢？看下面的测试：

```js
window.onclick = function () {console.log('onclick')}
window.addEventListener("click", function () { console.log('capture') }, true);
window.addEventListener("click", function () { console.log('bubble') }, false);
```

结果：

```
capture
onclick
bubble
```

```js
window.addEventListener("click", function () { console.log('capture') }, true);
window.onclick = function () {console.log('onclick')}
window.addEventListener("click", function () { console.log('bubble') }, false);
```

结果：

```
capture
onclick
bubble
```

```js
window.addEventListener("click", function () { console.log('capture') }, true);
window.addEventListener("click", function () { console.log('bubble') }, false);
window.onclick = function () {console.log('onclick')}
```

结果：

```
capture
bubble
onclick
```

从上面的示例可以看出，捕获永远是最先执行的，`onclick` 与冒泡的顺序与绑定事件的顺序有关，谁先绑定谁先执行。由此推断，`onclick` 其实发生在冒泡阶段。


### 事件代理 

正是利用上面的事件冒泡，衍生出了「事件代理」。即不将事件处理器直接绑定在目标身上，而是绑定在容器上，这样就不用为每个元素绑定事件处理器，节省了资源，也解决了动态生成的元素的事件绑定问题及元素删除后事件的清理问题。


### 相关资源

* [What is event bubbling and capturing?](https://stackoverflow.com/questions/4616694/what-is-event-bubbling-and-capturing)
* [addEventListener vs onclick](https://stackoverflow.com/questions/6348494/addeventlistener-vs-onclick)

