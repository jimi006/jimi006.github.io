# PHP実践

フォームで送信したデータを受け取る
フォームで送信した値を受け取るには「$_POST」を使用します。
「$_POST」は連想配列になっています。[ ]の中に、< input >と< textarea >のname属性に指定した値を入れることで、それぞれの送信した値を受け取ることが出来ます。

```PHP
 index.php

  <form action="sent.php" method="post">

  // テキストボックス
    <input type="text" name="name">

  // テキストエリア
    <textarea name="body"></textarea>

 // 送信ボタン
    <input type="submit" value="送信">
  </form>
```

テキストボックス<br>
<input type="text" name="name">

 テキストエリア<br>
<textarea name="body"></textarea>

 送信ボタン
<input type="submit" value="送信">

```PHP
sent.php

// nameを受け取りecho
  <?php echo $_POST['name'] ?>

// bodyを受け取りecho
  <?php echo $_POST['body'] ?>
```
---

セレクトボックスで選んだ選択肢の値を渡す
セレクトボックスの値の渡し方を見てみましょう。
< select >タグには「$_POST」で値を受け取るためのname属性を指定します。
< option >タグのvalue属性が送信される値です。

```PHP
 index.php

  <form action="sent.php" method="post">

    <select name="fluit">

      <option value="apple">りんご</option>
      <option value="banana">ばなな</option>
      <option value="orange">みかん</option>

    </select>
  </form>
```
セレクトボックス
<select name="fluit">
  <option value="apple">りんご</option>
  <option value="banana">ばなな</option>
  <option value="orange">みかん</option>
</select>

```PHP
sent.php

// <select>タグのnameを受け取りecho
  <?php echo $_POST['fruit'] ?>
```
---

繰り返し処理と変数展開を用いて多数の< option >タグを作る


```php
例1）for文を用いて6歳から100歳までをoptionで選べるようにする

  <select name="age">
    <option value="未選択">選択してください</option>
    <?php 
      for($i = 6; $i <= 100; $i++){
        echo "<option value='{$i}'>{$i}</option>";
      }
    ?>
  </select>
```

```PHP
例2）foreach文を用いて配列をoptionで選べるようにする

  <?php 
    $types = array('PHP', 'Java', 'Python', 'JavaScript',  'HTML');
  ?>
  
  <select name="category">
    <option value="未選択">選択してください</option>
    <?php
      foreach($types as $type){
        echo "<option value='{$type}'>{$type}</option>";
      }
     ?>
  </select>
```