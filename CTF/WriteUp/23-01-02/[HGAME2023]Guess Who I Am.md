> 刚加入Vidar的兔兔还认不清协会成员诶，学长要求的答对100次问题可太难了，你能帮兔兔写个脚本答题吗？

![[Pasted image 20230106141654.png|700]]

单击`Vidar-Team`跳转至以下地址

```
https://vidar.club/member
```

![[Pasted image 20230106141928.png|700]]

大概是要求寻找符合`Intro`的成员的名字填入输入框

![[Pasted image 20230106142055.png|700]]

在`index.xxx.chunk.js`里找到原始数据

```js
a = function(arg){return 0;}; // 有的avatar通过a(<number>)获取，这里随便定义下
data = [...]; // 复制粘贴
```

然后来一波自动化

```js
// 获取询问中的intro
s = document.getElementsByClassName("question")[0].attributes["value"].value;
// 屏蔽alert弹窗
alert = function(){return 1;};

for (let index in data) {
	if (data[index].intro == s) {
		// 直接赋值value不管用，还需要显式触发input事件来让event handler更新一些值
	    let inputFrame = document.getElementsByClassName("n-input__input-el")[0];
	    let event = document.createEvent("HTMLEvents");
	    event.initEvent("input", true, true);
		inputFrame.value = data[index].id;
		inputFrame.dispatchEvent(event);

	    document.getElementsByClassName("n-button")[0].click()
	    break;
	}
}
```

反复执行，或者直接加个(伪)递归（`js`没有`sleep`是没想到的）

```js
var times = 100;

function action() {
  // 获取询问中的intro
  s = document.getElementsByClassName("question")[0].attributes["value"].value;
  // 屏蔽alert弹窗
  alert = function() {
    return 1;
  };

  for (let index in data) {
    if (data[index].intro == s) {
      // 直接赋值value不管用，还需要显式触发input事件来让event handler更新一些值
      let inputFrame = document.getElementsByClassName("n-input__input-el")[0];
      let event = document.createEvent("HTMLEvents");
      event.initEvent("input", true, true);
      inputFrame.value = data[index].id;
      inputFrame.dispatchEvent(event);

      document.getElementsByClassName("n-button")[0].click()
      break;
    }
  }

  times = times - 1;
  if (times != 0) {
    setTimeout(action, 100);
  }
}
setTimeout(action, 100);
```

![[Pasted image 20230106145712.png|700]]

感觉不如手动快QAQ

#Web #js #自动化