![](<./img/Pasted image 20230111094342.png>)

---
~~首页长这样~~

![](<./img/Pasted image 20230111135924.png>)

害没加载完就die了

![](<./img/Pasted image 20230111140035.png>)

```js
document.onkeydown = function(event) {
  event = event || window.event;
  if (event.keyCode == 27) {
    event.preventDefault();
    window.location = "/chase/";
  } else die();
};
function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}
async function dietimer() {
  await sleep(10000);
  die();
}
function die() {
  window.location = "/die/";
}
dietimer();
```

`keyCode = 27`是`esc`，按下即可"escape"✨

---

```
/chase/
```

![](<./img/Pasted image 20230111140643.png>)

```html
<button onclick="left()">Left</button>
<button onclick="right()">Right</button>
```

```js
function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

async function dietimer() {
  await sleep(1000);
  die();
}

function die() {
  window.location = "/die/";
}

function left() {
  window.location = "/die/";
}

function leftt() {
  window.location = "/leftt/";
}

function right() {
  window.location = "/die/";
}

dietimer();
```

横竖都是死，这波直接不装了呗（

---

```
/leftt/
```

![](<./img/Pasted image 20230111141253.png>)

```html
<button onClick="window.location='/die/'">Take the shot</button>
<!-- <button onClick="window.location='/shoot/'">Take the shot</button> -->
```

---

```
/shoot/
```

![](<./img/Pasted image 20230111141552.png>)

震惊，居然没出事

---

```
/door/
```

![](<./img/Pasted image 20230111141701.png>)

360度开放大门的敌人是吧

```html
<script src="/static/js/door.js"></script>
<button onClick="check_door()">Check</button>
```

```
/static/js/door.js
```

```js
function check_door() {
  var all_radio = document.getElementById("door_form").elements;
  var guess = null;

  for (var i = 0; i < all_radio.length; i++)
    if (all_radio[i].checked) guess = all_radio[i].value;

  rand = Math.floor(Math.random() * 360);
  if (rand == guess) window.location = "/open/";
  else window.location = "/die/";
}
```

九死一生.jpg

---

```
/open/
```

![](<./img/Pasted image 20230111142521.png>)

```html
<script src="/static/js/open_sesame.js"></script>
<script>
  open(0);
</script>
```

```
/static/js/open_sesame.js
```

```js
function sleep(ms) {
  return new Promise(resolve => setTimeout(resolve, ms));
}

function open(i) {
  sleep(1).then(() => {
    open(i + 1);
  });
  if (i == 4000000000) window.location = "/fight/";
}
```

---

```
/fight/
```

![](<./img/Pasted image 20230111142909.png>)

```html
<script src="/static/js/fight.js"></script>
<input type="text" id="action">
<button onClick="check_action()">Fight!</button>
```

```
/static/js/fight.js
```

```js
// Run to scramble original flag
//console.log(scramble(flag, action));
function scramble(flag, key) {
  for (var i = 0; i < key.length; i++) {
    let n = key.charCodeAt(i) % flag.length;
    let temp = flag[i];
    flag[i] = flag[n];
    flag[n] = temp;
  }
  return flag;
}

function check_action() {
  var action = document.getElementById("action").value;
  var flag = ["{hey", "_boy", "aaaa", "s_im", "ck!}", "_baa", "aaaa", "pctf"];

  // TODO: unscramble function
}
```

`flag`复原之后是

```
pctf{hey_boys_im_baaaaaaaaaack!}
```

#Web #js #代码审计 