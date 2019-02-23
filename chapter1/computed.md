# computed

前回のHello, World!の表示ではVueインスタンスのコンストラクタにJSON形式で値を渡していました。

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

今回は追加でcomputedタグを解説します。computedはVueインスタンスに含まれ、算出プロパティ\(computed property\)と呼ばれます。computedタグはいわゆる関数に似た働きをします。実際にcomputedタグを`helloworld.html`に書くと以下の通りになります。

```javascript
var vm = new Vue({
    el: '#app',
    data: {
        message: 'Hello, World!'
    },
    computed: {
        warota : function(){
            return this.message + 'www';
        }
    }
})
```

そしてdivタグの中身を以下のように変更します。

```markup
<body>
    <div id="app">
        {{ warota }}
    </div>
</body>
```

描き終えたらブラウザで確認してみましょう。`Hello World!www`と表示されていれば成功です。

### 解説

まず今回作成した`warota`に関しての説明を行います。`warota`は`data`のmessageに対して**www**を文字列に追加するメソッドです。これによって草を生やすことができます。ここで、`this.message`のthisはvmを指しています。前回`vm.message`でmessageを参照できたように、`this.message`でmessageの内容を取得・変更できます\(_**this.data.messageとはできないので注意\)**。_

次に`{{ warota }}`部分についてです。warotaは関数なのにwarota\(\)のようにしないのはおかしいんじゃないか。と感じた人もいるかもしれません。その人は少し勘が鋭いです。冒頭で

> computedタグはいわゆる関数に似た働きをします

と書きました。そうです。computedは厳密には関数ではないです。computedを使用するときの注意としては

1. dataを変形するようなときにしか使用しない
2. 乱数を使用するメソッドには使わない

があります。1はなぜかと言うと、computedは**変数**のような役割を果たすからです。要するに`warota`は変数messageにwwwを語尾に必ず追加する変数としてみることができます。なので関数呼び出しをする必要がありません。  
次に2は、computedの特性に関しています。少し難しいですが、computedは最初に呼び出された時の値をキャッシュしています。これは再度計算を行わなくていいようにコンピュータがメモを取ると言うことです。computedは頭がいいので例えば乱数を返すような処理で

```javascript
computed: {
    randomNumber: function(){
        return Math.random();
    }
}
```

とした場合、`randomNumber`を何度呼び出しても同じ値しか返しません。これはコンピュータが`Math.random()`の値をキャッシュしてしまい、２度目に`Math.random()`を呼び出さずに前の値を使いまわしてしまうためです。computedはリアクティブな要素\(data内の変数等\)が変更された場合にだけキャッシュがクリアされます。これによってユーザは必要以上に同じ処理をしなくてもよくなるため高速化に繋がります。なのでcomputedを使う場合にはある程度注意をする必要があります。

じゃあ乱数を返す関数はVue.jsでは作れないの？となるかもしれません。安心してください。次章で関数を定義するmethodsについて解説を行います。

