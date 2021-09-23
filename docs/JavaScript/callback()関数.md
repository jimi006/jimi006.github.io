# callback()関数について

## 目次

1. [コールバック関数とは](<JavaScript/callback()関数?id=_1-コールバック関数とは>)
2. [基本的な記述法](<JavaScript/callback()関数?id=_2-基本的な記述法>)
3. [様々な記述例](<JavaScript/callback()関数?id=_3-様々な記述例>)
4. [まとめ](<JavaScript/callback()関数?id=_4-まとめ>)

<br>

---

<a id="anchor1"></a>

### 1. コールバック関数とは

「コールバック」は、簡単に言うと関数の引数に別の関数を指定する処理を指します。
これにより、A という関数が実行されたあとに引数で指定していた B という関数を実行するということが可能になります。
よく使われるケースとして、例えばサーバーからデータを取得したあとに次の処理を実行したいという場合です。
<br>

---

<a id="anchor2"></a>

### 2. 基本的な記述法

```javascript
function 関数( 別の関数（callback） ) {

  //何らかの処理を記述する
```

<br>

---

<a id="anchor3"></a>

### 3. 様々な記述例

3-1. 無名関数を使った記述法

```javascript
function callback1(n1) {
  console.log('callback1が' + n1 + 'を受け取ったよ');
}
```

3-2. Ajax を利用した記述法

```javascript
function getJsonData(callback) {
  const xhr = new XMLHttpRequest();

  xhr.open('GET', '/sample.json');
  xhr.send();

  xhr.onreadystatechange = function() {
    if (xhr.readyState === 4 && xhr.status === 200) {
      callback(JSON.parse(xhr.responseText));
    }
  };
}
```

<!-- 3-3. -->

<br>

<!-- ---

<a id="anchor4"></a>

### 4. まとめ

<br>

--- -->
