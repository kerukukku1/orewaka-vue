# v-model

v-modelはこれぞVue.js!といったディレクティブです。まずは体験してもらった方が早いです。以下のようなコードを実装してみてください。

```markup
<div id="app">
    <input v-model="text"></input> | input : 
    {{ text }}
</div>
```

```javascript
data: {
    text: ''
}
```

たったこれだけです。ブラウザで確認するとinputエリアが表示されています。inputエリアに適当な文字を入力してみてください。リアルタイムにそれが右側に反映されていけばOKです。

### 解説

#### 双方向バインディング

双方向バインディングとは、その名の通り双方向にバインディングをすることです。今までは`v-bind:対象`で対象に対する一方向バインディングを行なっていました。今回の場合、textに対するバインディングとinputの中の文字列`$event.target.value`に対してバインディングが行われています。何を言っているのかわからない？では、コンソールでvm.textの値を`'aaa'`としてみましょう。

```javascript
vm.text = 'aaa';
```

さて、どうなったでしょうか。input内の入力値が**'`aaa'`**となったと思います。これはinputの要素**value**と**text**がバインディングされているからです。このように双方にバインディングし影響を及ぼしあうのを双方向バインディングと言います。では以下のコードをみてください。

```markup
<input
    v-bind:value="text"
    v-on:input="text = $event.target.value"
></input> 
| input : {{ text }}
```

これはv-modelと全く同じ動作をします。v-on:inputで入力イベントを検知し、**入力値**`$event.target.value`を**textプロパティに常に代入**します。また、**inputエリアの表示の値**`value`は常に変数textと同値になるようにバインディングされます。これによって**textの値とinputエリアの表示の値**が双方向バインディングになりました。お互いの値の同期が取れていると考えてもらえれば簡単かもしれません。

