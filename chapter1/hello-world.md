# Hello, World!

さあ、とりあえずHello World!が表示できないことには話になりません。Vue.jsでHello World!を表示するのはとても簡単です。デスクトップに`src`を作成し、`src/helloworld.html`を作成してみましょう。内容は以下の通りにします。

{% code-tabs %}
{% code-tabs-item title="helloworld.html" %}
```markup
<!DOCTYPE html>
<html>
    <head>
    <meta charset="UTF-8" />
    <script src="https://cdn.jsdelivr.net/npm/vue"></script>
    </head>
    
    <body>
        <div id="app">
            {{ message }}
        </div>
    </body>
    
    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                message: 'Hello, World!'
            }
        })
    </script>
</html>
```
{% endcode-tabs-item %}
{% endcode-tabs %}

これが描き終わったら`src/helloworld.html`をGoogleChrome等で開いてみましょう。Hello World!と表示されていれば成功です。

### 解説

細かな部分の説明をしていきます。まず`body`タグに注目しましょう。

```markup
<body>
    <div id="app">
        {{ message }}
    </div>
</body>
```

`div`タグが作成され、`id`が`app`となっています。また、`div`タグ中には{{}}で囲まれてmessageが入っていることがわかります。このmessageはVue.jsの変数で、文字列**`Hello World!`**が入っています。さて、ではこの**message**はどこで定義されているのでしょうか。答えは`script`タグの以下の部分にあります。

```markup
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            message: 'Hello, World!'
        }
    })
</script>
```

変数`vm`にVueクラスがnewされています。これはVueのインスタンスを生成しています。ここで、変数`vm`はついては今の議論にはあまり意味をなさない変数なので無視します。

Vueのインスタンスを生成するために、コンストラクタに**JSON形式**で値が渡されています。

{% code-tabs %}
{% code-tabs-item title="json形式の引数" %}
```javascript
{
    el: '#app',
    data: {
        message: 'Hello, World!'
    }
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}

これらはC++で言うところの連想配列であり、タグに対して値が参照できる配列のことです。タグの役割を説明すると以下の通りです。

* **elタグ** 　Vue.jsをHTMLのタグにマウントする要素を決める
* **dataタグ** 　Vue.jsで用いる変数を定義する

`el`タグによってVue.jsを適用する箇所を決め\(今回の場合`<div id="app">`\)、その適用された箇所で使用できる変数を`data`で記述しています。これによって`data`タグ中のmessage変数に`Hello World!`を入れて表示することが可能になります。

さて、これでHello World!の表示される仕組みを一通りみてきました。ですがこれではVue.jsの一部も理解できていません。では、作成した`helloworld.html`をGoogleChromeで開きます。その後、右クリック -&gt; 検証を押して開発者ツールを開きます。開けたらConsoleタブをクリックします。するとコンソールが出てくるのでそこにvm.messageと入力してEnterしましょう。すると"Hello, World!"と表示されることがわかると思います。  
そうです。これは先ほど作成した`vm`インスタンスのmessage変数を参照しています。では、このmessage変数の内容を変更したらどうなるでしょうか？

```javascript
vm.message = 'Goodnight, World.'
```

画面に表示されていた文字列が入力した内容に変更されたことがわかると思います。しかもこれはページの更新、読み込みをしていません。これがVue.jsのリアクティブなシステムの一端です。このようにページ遷移をすることなく値の変更を検知してViewを変更することが可能です。

