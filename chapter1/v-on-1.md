# v-on

v-onはディレクティブの中でも特に重要です。v-onディレクティブは要素にイベントリスナを付与します。イベントリスナはデフォルトの要素に対して付いていてクリック・入力・マウスが上に乗ったetc...などがあります。イベント\(動作\)を監視するものだと思っていればOKです。

以下にいくつかのv-onを用いたサンプルを紹介します。

```javascript
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8" />
        <script src="https://cdn.jsdelivr.net/npm/vue"></script>
    </head>
    
    <body>
        <div id="app">
            数値を入力してください。
            <input v-on:input="a = $event.target.value"></input> +
            <input v-on:input="b = $event.target.value"></input> =
            <button v-on:click="sum()">実行!</button>
        </div>
    </body>

    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                a: 0,
                b: 0
            },
            methods: {
                sum : function() {
                    alert(Number(this.a) + Number(this.b));
                }
            },
        })
    </script>
</html>
```

inputエリアに適当な数字をそれぞれいれて実行ボタンを押してみましょう。二つの数値の和が出てくればOKです。

### 解説

上記コードでは以下のv-onディレクティブを用いました。

* v-on:input
* v-on:click

**v-on:input**はinputエリア等、キーボードからの入力で発火するイベントリスナです\(イベントが呼ばれることを発火と呼びます\)。また、入力は**$event**変数に自動的に定義され、**$event.target.value**とすることで入力された値を取得することができます。これによって、入力された値をそれぞれプロパティ**a,b**に代入しています。

**v-on:click**は要素がクリックされたときに発火するイベントです。実行ボタンがクリックされたとき、要素が発火し、**sum\(\)**メソッドが呼び出されます。これによって、alertで数値の合計値が表示されるようになっています。

