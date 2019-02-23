# v-bind

## スタイルバインディング

`v-bind`は属性やプロパティと式を束縛\(bind\)することができます。実際に例を見ていきます。dataに以下のオブジェクトを追加します。

```javascript
data: {
    message: 'Hello, World!',
+   redObject: {
+       color: 'red'
+   }
},
```

そして次にHTMLを以下のように変更します。

```markup
<div id="app">
    <span v-bind:style="colorObject">Red World?</span>
</div>
```

ブラウザで確認して文字列が赤く表示されていればOKです。

### 解説

v-bindをすることによって、タグに対して属性名を指定することができます。先ほどの

```markup
<span v-bind:style="colorObject">Red World?</span>
```

これはstyle属性に対して`redObject`をbindしています。これは

```markup
<span style="color : red">Red World?</span>
```

としていることと同義です。v-bindの利点は、通常のHTMLではリアクティブに変更するのが難しかった部分をv-bindによって束縛することで動的に変更することが可能になるところです。では試しにcolorObjectのredをblueに変更してみましょう。

```javascript
vm.colorObject.color = 'blue'
```

変更されましたか？おそらく**何も変更が無い**と思います。これはVue.jsの仕様で、ObjectやArrayの変更をリアクティブに検知させるには**`Vue.set`**を用います。

```javascript
Vue.set(vm.colorObject, 'color', 'blue');
```

構文は`Vue.set(object, key, value)`です。これによってコンソール上から表示色を動的に変更することができました。

## クラスバインディング

スタイル同様にクラスもbindすることができます。ここで言うクラスとはCSSのことを指します。試しにCSSを作成して読み込んでみましょう。

```markup
<head>
    <meta charset="UTF-8" />
    <script src="https://cdn.jsdelivr.net/npm/vue"></script>
+   <style type="text/css">
+       .red-back {
+           background-color: red;
+       }
+   </style>
</head>
```

`red-back`という背景が赤色になる簡単なCSSです。このスタイルをdiv要素に適用したい場合、以下のように書くことができます。

```javascript
<div id="app">
    <span v-bind:class="{'red-back': isRedBack}">Red Back?</span>
</div>
```

ここでisRedBackはdataに定義されたBoolean型のプロパティです。

```javascript
data: {
    message: 'Hello, World!',
    redObject: {
        color: 'red'
    },
+   isRedBack: true
},
```

ブラウザで確認して文字の背景色が赤色になっていればOKです。

### 解説

スタイルバインディングと同様の動きです。上では説明しませんでしたが、スタイルバインディング同様にdataに定義を書いておき、それを読み込ませることも可能です。

```javascript
data: {
    redObject: {
        'red-back': true
    }
}
```

```markup
<span v-bind:class="redObject">Red back?</span>
```

このように、クラスバインディングではCSSのスタイルと同じ名称のタグと、その条件がtrueである時にクラスを適用する仕組みになっている。この辺は忘れやすい・理解しにくい部分であるため、自分でいくらかサンプルとは異なるものを自分で考え実装することで整理することが望ましいでしょう。



