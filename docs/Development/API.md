# API

---

### API とは

> API（Application Programming Interface）は、アプリケーション同士を協調して動作させるための仕様です。<br>
> アプリケーションは API を提供することで、他のアプリケーションとやりとりすることができます。

例えば、Twitter、Facebook など様々な事業者は自社のサービスを外部のアプリケーション向けに提供するために、Web API を公開しています。外部のアプリケーションはそれらの Web API を通してサービスを利用できます。

私たちが Web ページを参照する場合、Web ブラウザが HTTP の URL にリクエストし、レスポンスとして返される HTML を描画します。

それと同様に Web API の場合も、アプリケーションが HTTP の URL にリクエストし、レスポンスとして返されるデータを処理します。

Web API では、HTTP 経由で JSON 形式のメッセージを送受信するのが一般的です。[`JSON（JavaScript Object Notation）`](Development/JSON.md)は、JavaScript の記法をベースにオブジェクトを表記する方法で、様々なプログラミング言語で使われる汎用的な形式です。
