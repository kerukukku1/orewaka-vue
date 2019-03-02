# グローバルコンポーネント

それではVue.jsでのコンポーネントについて説明していきます。前回のおさらいですが、コンポーネントとはWeb開発に置ける部品を指します\(ボタンや画像等\)。Vue.jsにおけるコンポーネントとは、自己完結し、小さく、再利用性があることを思想においています。Vue.jsのコンポーネントは`Vue.component(name, option)`で書かれます。nameはタグ名、optionはVueインスタンスと同じような設定ができます。以下がVueコンポーネントで設定できるoptionの一覧です。

| オプション名 | 用途 |
| :--- | :--- |
| data | UIの状態やデータを定義する |
| methods | イベントが発生したときなどの動きをメソッドで置く |
| computed | データの情報を変化させて返す |
| **template** | コンポーネントのテンプレートのHTMLを書く |
| **props** | 親ノードから子ノードへデータを受け渡す |
| **created** | ライフサイクルフック |
| **filters** | データを文字列と整形して処理を挟む |

ざっとこれだけのオプションがあります\(いくつかはまだ端折っています\)。前半の3つのオプションはVueインスタンスを作成するときにいつもお世話になっていました。後半の太字4つは新出のオプションです。以下で例を交えて説明します\(ちなみに表のオプションはVueインスタンスにも使えます\)。

### templateオプション

```markup
<body>
  <div id='app'>
    <my-name />
  </div>
</body>

<script>

  Vue.component('my-name', {
    template: '<div> Mahito Fujino </div>'
  })

  var vm = new Vue({
        el: '#app',
  });
</script>
```

templateオプションはその名の通りコンポーネントのテンプレートを定義します。上の場合、divタグで囲まれた自分の名前を表示するプログラムになっています。

### propsオプション

```markup
<body>
  <div id='app'>
    <my-name name='Mahito Fujino'/>
  </div>
</body>

<script>
  Vue.component('my-name', {
    props: {
      name: String
    },
    template: `<div> {{ name }} </div>`
  })
  
  var vm = new Vue({
    el: '#app',
  });
</script>
```

propsオプションはコンポーネントに対して親が子にデータを渡せる構造です。propsには`属性名:型`を定義します。nameの場合はStringが型なので上のような形になります。bodyの中を見てみると、my-nameタグでname属性に名前を入力していることがわかります。これで親コンポーネントから子コンポーネントのmy-nameに対してnameという名前のデータを渡すことができます。ちなみにこのnameは`this.name`の形でコンポーネント内から参照することができます。

### createdオプション

```markup
<body>
  <div id='app'>
    <my-name />
  </div>
</body>

<script>
  Vue.component('my-name', {
    props: {
      name: String
    },
    template: `<div> {{ name }} </div>`,
    created() {
      alert('my-name component is created!');
    }
  })
  
  var vm = new Vue({
    el: '#app',
  });
</script>
```

createdオプションはライフサイクルにフックされます。今は何を言っているのか理解するのは難しいと思うのでライフサイクルについては後述しますが、createdはmy-nameのコンポーネントが生成された瞬間に呼ばれるメソッドだと考えてくれていればOKです。

### filtersオプション

```markup
<body>
  <div id='app'>
    <input v-model="name"></input>
    <my-name :name="name"/>
  </div>
</body>

<script>
  Vue.filter('headNameUpper', function(name){
    if(!name)return;
    var ret = name.toString();
    return ret.charAt(0).toUpperCase() + ret.slice(1);
  });
  Vue.component('my-name', {
    props: {
      name: String
    },
    template: `<span> Your name : {{ name | headNameUpper }} </span>`,
    created() {
      // alert('my-name component is created!');
    }
  });

  var vm = new Vue({
    el: '#app',
    data : {
      name : ''
    },
  });
</script>
```

filtersオプションは文字に対して処理を噛ませることができるオプションです。上の例では、入力された文字に対して頭文字を大文字に自動で変換して表示する機能を実装しています。フィルタの定義はcomponentと似ていて、`Vue.filter(name, function)`の形で書くことができます。フィルタの適用を適用する場合、{{ value \| filter }}といった形をとります。

filterは名前と、そのフィルタの振る舞いをfunctionで定義します。定義するfunctionにはフィルタから値が第一引数として渡されるのでその値をメソッド内で変形させるのが一般的な動きです。

## 問題点

さて、今回の`Vue.component`の書き方は問題が発生する場合があります。それはこれがグローバルコンポーネントだからです。グローバルコンポーネントは定義されるとどのVueインスタンスでも使用が可能になります。どのインスタンスからも呼ばれてしまう時点でVueの小さく作るといった思想に反してきますし、基本的にグローバルなコンポーネントは用いることは少ないです。では、どうやってコンポーネントを定義すればいいのか？それは**ローカルコンポーネント**で定義することです。次はローカルコンポーネントについてお話しします。

