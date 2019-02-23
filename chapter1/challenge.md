# CHAPTER1 - challenge

さて、ここまでVue.jsの機能を色々と紹介してきました。まだVue.jsの全容は見えてきてはいませんが、一度中間ポイントとして課題を与えます。この課題を解けるようになっていればCHAPTER1は理解できていると思います。

{% hint style="success" %}
## 問題

名前を入力して追加ボタンを押すたびにリストに名前を追加していく名簿プログラムを作成せよ。またリストの名前の後には「様」がつくようにせよ\(**変数に様付けして代入してはいけない**\)。

#### EXTRA

入力された名前が1文字以下9文字以上の場合にエラーメッセージを出力するようにせよ。
{% endhint %}

{% hint style="info" %}
### Hint

使うのはcomputed, v-if, v-for, v-on:click, v-on:input

配列に要素を追加するのは`array.push(content)`の形
{% endhint %}

## 解答例

```markup
<!DOCTYPE html>
<html>
    <head>
        <meta charset='UTF-8' />
        <script src='https://cdn.jsdelivr.net/npm/vue'></script>
        <style type='text/css'>
            .error {
                color: red;
                border-bottom: 1px solid black;
                padding: 10px;
            }
        </style>
    </head>
    
    <body>
        <div id='app'>
            <br>
            お名前 : 
            <input 
                v-on:input='name = $event.target.value'
                v-bind:value='name'
            ></input>
            <button v-on:click='addMember()'>追加</button>
            <span v-if='!canAdd' :class='{error: !canAdd}'> 名前は2文字以上8文字以下にしてください。</span>
            <h1>名簿リスト</h1>
            <ul>
                <li v-for='member in names'> {{ displayName(member) }}</li>
            </ul>
        </div>
    </body>

    <script>
        var vm = new Vue({
            el: '#app',
            data: {
                name,
                names: []
            },
            methods: {
                addMember : function() {
                    if(!this.canAdd)return;
                    this.names.push(this.name);
                    this.name = '';
                },
                displayName : function(name) {
                    return name + '様';
                }
            },
            computed: {
                canAdd: function () {
                    const len = this.name.length;
                    return (len >= 2 && len <= 8);
                }
            }
        })
    </script>
</html>
```

