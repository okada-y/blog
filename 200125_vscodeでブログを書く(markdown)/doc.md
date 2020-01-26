<!--
{"id":"26006613502442871","title":"VScodeでブログを書く、投稿する","categories":["vscode","ブログ"],"draft":"no"}
-->













こんにちは、おかやんです。  

先日、マイクロマウスの開発環境をvscodeに移行させ（またブログに書きます）  
なんでもかんでもvscodeで書きたがるお年頃になってしまいました。  

そこで手始めに、markdawnで記述しているこのブログをvscodeで書いてみよう、 
投稿してみようと。  
今回はその準備と簡単な紹介です。

[:contents]

### 環境構築

今回はVscodeの導入が終わっている前提で話を進めます。

<a id="markdown-拡張機能の導入" name="拡張機能の導入"></a>
#### 拡張機能の導入

今回は以下の拡張機能を導入します。   

- `hatebablogger`：はてなブログへの投稿 

導入の方法は、
`Ctrl+Shift+x`でExtensonsを開き、拡張機能の名前を入力。  
以下の画面から緑色のあるinstallボタンをクリックすると導入できます。  
![](https://cdn-ak.f.st-hatena.com/images/fotolife/o/okayannnn/20200125/20200125192942.png)

環境構築は以上です。簡単！

<a id="markdown-ブログを書く" name="ブログを書く"></a>
### ブログを書く

Markdawnの書き方や、便利ショートカットなどの紹介はほかのブログに任せるとして、  
今回はvscodeでブログをかくにあたり、便利と感じた機能の紹介をしていきます。


<a id="markdown-プレビュー機能を使う" name="プレビュー機能を使う"></a>
#### プレビュー機能を使う。
vscodeには標準でmarkdownの出力をプレビューできる機能が実装されています。   
 非常に便利そうなのでつかってみます。

まず初めに、markdownの標準拡張子となる'mdファイル'を作成、開きます。  
次に、vscodeの右上にある以下のマークを押して編集画面をに分割します。  
![](https://cdn-ak.f.st-hatena.com/images/fotolife/o/okayannnn/20200125/20200125193155.png)

そして、`Ctrl+k,v`をおすとプレビュー画面が表示されます。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/o/okayannnn/20200125/20200125193459.png)  

この状態で画面左側のエディタを編集していくと、プレビューが都度対応します。

<a id="markdown-intellisenseを使う" name="intellisenseを使う"></a>
#### intellisenseを使う。

vscodeではintellisenseという自動補完支援システムが搭載されています。  
markdawn編集の際も便利なのでしっかり使っていきたいところです。

編集画面にて`Ctrl+space`をおすと、  
![](https://cdn-ak.f.st-hatena.com/images/fotolife/o/okayannnn/20200125/20200125195106.png)  
こんな感じで編集時につかうであろうコマンドの候補が出てきます。

例えば`unorderd list`を選択すると、以下のようなリストが出力されます。便利！

- first
- second
- third

#### スニペットを使う。
スニペットとはwindowsでいう辞書機能のようなものです。  
とても便利なのでこれも使っていきましょう。 

スニペットに単語を登録していきます。  
まずは`File -> preference -> user snippet`を選択します。  
すると適応させる言語のリストが表示されます。  
今回は`markdown`を選択します。

すると、`markdown.json`の編集画面に移ります。  
ここに辞書登録したい文字を書いていきます。
以下はその例です。

	"content": {    //適当なラベル        
        "prefix": "content", //入力するワード
        "body": [
            "[:contents]"//出力するワード
        ],
        "description": "For HatenaBlog" //説明
    },

これにより、conte・・・あたりまで入力して、`Ctrl+space`を入力し、
contentを選択すると、登録した文字が出力されます。

![](https://cdn-ak.f.st-hatena.com/images/fotolife/o/okayannnn/20200125/20200125210917.png)

よく入力する文字は登録しておくと便利ですね。

<a id="markdown-ブログを投稿する" name="ブログを投稿する"></a>
### ブログを投稿する

Vscodeでブログを編集したら、次はVscodeで投稿しましょう。  
ここで事前に導入した`hatebablogger`が活躍します。  

`hatebablogger` の使い方は作者のページ
[作者のページ](http://uraway.hatenablog.com/entry/2018/12/12/001545)
が大変わかりやすいので、そちらを参照してください。

> 注意：VScodeの日本語化をしていると上手く機能しませんでしたので、使用の際は日本語化をやめるとよいです。

`hatebablogger`では以下のことができます。  
 
- 下書きの投稿、更新
- 記事の投稿、更新
- 画像の投稿

私としては特に3つ目の画像の投稿ができるのが素晴らしいと感じています。  
これで私も快適はてなブロガーです。

<a id="markdown-まとめ" name="まとめ"></a>
### まとめ

簡単ですが、以上です。  
vscodeを導入してからまだ1週間ほどですが、   
vscodeの可能性にわくわくがとまらないこの頃です。  
やはり使用人口が多いのは正義。

ブログの内容に誤り等ございましたら、ご指摘ください。
それでは、また。