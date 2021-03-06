# 繰り返し・条件分岐 \(v-for, v-if\)

## 

## v-for

ディレクティブとはDOM要素に対して何らかの処理を実行することをライブラリに伝達するものです。Vue.jsでは接頭語にv-がつくものがそれにあたります\(v-bind等\)。`v-for`は繰り返し処理を行うディレクティブであり、様々な場面で活躍します。例えば以下のようなリストを表示するプログラムを作るとします。

* 和田
* 高松
* 若山

これを`v-for`を用いて表示すると以下のようになります。

```markup
<ul>
    <li v-for="name in names"> {{ name }} </li>
</ul>
```

```text
data: {
    names : [
        '和田', '高松', '若山'
    ]
}
```

名前のリストが表示されれば成功です。

### 解説

v-forの構文は`変数in配列`といった形になります。配列の内容を一つ一つ変数に代入していきます。これは一般的なfor文と同様の動きです。namesの要素が代入されたnameは変数なので{{}}で囲って表示させることができます。

次に、上記のリスト表示のコードをHTMLのみで表現しようとすると以下のようになります。

```markup
<ul>
    <li> 和田 </li>
    <li> 高松 </li>
    <li> 若山 </li>
</ul>
```

HTMLだけのほうが簡潔で、`v-for`使うコードの方が冗長になってない？と感じる人が多いと思います。確かにこの程度なら生HTMLだけで書ききれますね。しかしこれが100人単位でリストを表示したいといった場合には途方もない量のコードをコピーペーストする必要があります。`v-for`を用いることでこれを簡単にすることができます。

いやいや結局**`names`**に名前入れてるから名前を入れる手間は同じなんじゃ無いの？と思う人もいるかもしれません。この場合はnamesをデータベースから参照するようにすればいちいち書く必要もありません。後半の章ではデータベースと連携した例も紹介する予定です。

## v-if

v-ifは値の真偽値によって要素の描画を行うかを決めるディレクティブです。先ほど作ったリストにv-ifを追加すると以下のようなコードとなります。

```markup
<ul v-if="isShow">
    <li v-for="name in names"> {{ name }} </li>
</ul>
```

```javascript
data: {
    isShow: false
}
```

リストの表示が消えていれば成功です。また、コンソールで`vm.isShow`の値をtrueに変化させてみて、リストが再度表示されればOKです。

### 解説

v-ifはバインドされた値の真偽値によって表示・非表示を決定します。これは例えばエラーメッセージの表示に用いることができます。テキストボックスに入力された値が不正であった場合等にv-ifの表示をONにすればエラーメッセージを表示させることができます。

それでは軽く問題を解いてみましょう。

{% hint style="success" %}
## 問題

59点以下であれば赤点と表示するプログラムを作成せよ。また赤点と表示する文字はクラスバインディングによって文字色を赤くせよ。
{% endhint %}

## 

```markup
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8" />
        <script src="https://cdn.jsdelivr.net/npm/vue"></script>
        <style type="text/css">
            .error {
                color: red;
            }
        </style>
    </head>
    
    <body>
        <div id="app">
            <p v-if="!canPass" v-bind:class="{error: !canPass}">赤点です。</p>
            <span>点数 : {{ score }}</span>
        </div>
    </body>

    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                score: 59
            },
            computed: {
                canPass : function() {
                    return this.score >= 60
                }
            },
        })
    </script>
</html>
```

