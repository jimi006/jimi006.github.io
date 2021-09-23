# Github Actions

---

## 目次

1. [Github Actions とは](Git/GithubActions?id=_1-github-actions-とは)
2. [Github Actions 用語集](Git/GithubActions?id=_2-github-actions-用語集)

   1. [Workflow](Git/GithubActions?id=_2-1-workflow)
   2. [Workflow file](Git/GithubActions?id=_2-2-Workflow-file)
   3. [Job](Git/GithubActions?id=_2-3-Job)
   4. [Step](Git/GithubActions?id=_2-4-Step)
   5. [Action](Git/GithubActions?id=_2-5-Action)
   6. [Command](Git/GithubActions?id=_2-6-Command)

3. [文法](Git/GithubActions?id=_3-文法)

   1. [Workflow の実行](Git/GithubActions?id=_3-1-workflow-の実行)

      1. [トリガー：cron](Git/GithubActions?id=_3-1-1-トリガー：cron)
      2. [トリガー：Push](Git/GithubActions?id=_3-1-2-トリガー：push)
      3. [トリガー：push とプルリクエスト](Git/GithubActions?id=_3-1-3-トリガー：push-とプルリクエスト)

   2. [Branch と Tag によるフィルター](Git/GithubActions?id=_3-2-branch-と-tag-によるフィルター)
   3. [実行環境を指定する](Git/GithubActions?id=_3-3-実行環境を指定する)

      1. [一つの実行環境](Git/GithubActions?id=_3-3-1-一つの実行環境)
      2. [複数の実行環境](Git/GithubActions?id=_3-3-2-複数の実行環境)

   4. [Action を使う](Git/GithubActions?id=_3-4-action-を使う)

---

<br><br>

### 1. Github Actions とは

GitHub Actions は、GitHub が提供する [`CI/CD`](Development/CI-CDツール.md) サービスである。 GitHub と高度に統合されており、GitHub に公開されたコードを自動でビルド・テスト・デプロイを行うのが主目的である。

<br><br>

### 2. Github Actions 用語集

Github Actions を使う上で知っておくべき用語をまとめる

<br><br>

#### 2-1. Workflow

Workflow は１つまたは複数の Job から構成される
Job の例としては、**build, test, package, release, deploy** がある
この Workflow は、cron でスケジュールされるか、事前に決めたイベントが生じたときに動き出す
事前に決められるイベントの例は、リポジトリへの Push、プルリクエスト、Webhook を使った外部のイベントがある

<br><br>

#### 2-2. Workflow file

Workflow file はリポジトリ内の.github/workflows ディレクトリに置かれた YAML ファイルのこと
これに Workflow の構成を書く

<br><br>

#### 2-3. Job

Job は Workflow file 内で Step の集まりとして定義される（Step は後で紹介する）
Job の識別子は前述したように、build, test, package, release, deploy などがある
複数の Job がある場合、それらを同時に実行したり、順番に実行したりできる
デフォルトではすべての Job が同時に実行される
Job を順番に実行していく場合は、一つ前の Job が成功した場合だけその後の Job が実行される

<br><br>

#### 2-4. Step

Job を構成する Action や Command の集まり(Action と Command はすぐ下で紹介する)
YAML ファイルの中では、Action は uses、Command は run というキーで指定される

<br><br>

#### 2-5. Action

Step を構成する最小単位で、自分で作ったもの使ったり、Github コミュニティで共有されているものを使ったり、パブリックなものを編集したりできる
Github Actions ではこの Action を使って Workflow を構成していくのが基本となるようだ

<br><br>

#### 2-6. Command

Command はこの Workflow が実行される環境の OS で使えるコマンドの事
Linux, macOS, Windows を選択でき、Workflow ではそれらのコマンドを使うことができる

<br><br>

---

### 3. 文法

ここからは、Workflow file に Workflow を書いていく
workflow file は.github/workflows に置く

<br><br>

#### 3-1. Workflow の実行

Workflow は事前に決めたイベントが起きた時に実行されると述べた
今回は cron と push と pull request イベントをトリガーにする方法を書く

<br><br>

##### 3-1-1. トリガー：cron

cron は "\* \* \* \* *"のように５つのセクションから成り
左から順に、分(0~59)、時(0~23)、日(1~31)、月(1~12)、曜日(0~6 0=日曜日)
*はすべての値を意味する
以下の例では、毎時 Workflow が実行される

```yaml
name: workflow-name
on:
  schedule:
    - cron: '0 * * * *'
```

<br><br>

##### 3-1-2. トリガー：Push

レポジトリへの Push をトリガーとする、おそらくもっとも一般的なトリガー
今後の説明ではこのトリガーを用いて説明していく

```yaml
name: workflow-name
on: push
```

<br><br>

##### 3-1-3. トリガー：push とプルリクエスト

プルリクも頻繁に行われると思うので追加する
以下のように[]にカンマ区切りで書けば複数指定できる

```yaml
name: workflow-name
on: [push, pull_request]
```

<br><br>

#### 3-2. Branch と Tag によるフィルター

単純に on: push と書くとすべてのブランチで push したときに Workflow が起動する
ブランチやタグごとに Workflow の構成を分けたい時にはフィルターを用いる

以下の書き方では、master ブランチへの push と、v1 というタグを push したときに Workflow が起動する

```yaml
name: workflow-name
on:
  push:
    branches:
      - master
    tags:
      - v1
```

<br><br>

#### 3-3. 実行環境を指定する

<br>

##### 3-3-1. 一つの実行環境

我々は Job の実行環境として Ubuntu, Linux, macOS などを選ぶことができる
また、build と test という二つのジョブを作ったときに、それぞれ別の実行環境で動作を確認できる

以下の例では、ubutu-18.04 の環境で Job を登録している
内容は、echo "Hello World"という Command がうまくいくかというもの

```yaml
name: workflow-name
on: push

jobs:
  build:
    name: Greeting
    runs-on: ubuntu-18.04
    steps:
      - name: Hello World
        run: echo "Hello World"
```

<br><br>

##### 3-3-2. 複数の実行環境

さっきは ubuntu-18.04 だけの確認だった
build matrix というものを使うと複数の実行環境での動作確認をできる
以下の例では ubuntu-16.04 と ubuntu-18.04 で実行させている

```yaml
name: workflow-name
on: push

jobs:
  build:
    name: Greeting
    runs-on: ${{matrix.os}}
    strategy:
      matrix:
        os: [ubuntu-18.04, ubuntu-16.04]
    steps:
      - name: Hello World
        run: echo "Hello World"
```

<br><br>

#### 3-4. Action を使う

Github Actions の肝である Action を使う
今回はすでに Github コミュニティで用意されているものを使ってみる
自分でも作ることができる

```yaml
name: Greet Everyone
on: push

jobs:
  build:
    name: Greeting
    runs-on: ubuntu-latest
    steps:
      - name:
          Hello World
          # GithubコミュニティのActionを使う
        uses:
          actions/hello-world-javascript-action@v1
          # withはactionへの入力パラメータのkey/valueのペア
        with:
          who-to-greet: 'hoge'
```

<br><br>
