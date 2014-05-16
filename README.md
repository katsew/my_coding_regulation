# コーディング規約

## HTML

・charsetはutf-8にすること
  ```
    <meta charset="UTF-8">
  ```
  
・doctypeはhtml5で宣言すること
  ```
    <!DOCTYPE html>
  ```
・img要素はalt属性を省略しない。省略可能な場合はalt=""とする  
  ```
    <img src="/img/example.png" alt="サンプル用の画像です">
  ```
  ```
    省略可能な場合
    <img src="/img/example.png" alt="">
  ```

・h1要素は1ページに複数配置しないこと  

・インデントはスペース2つですること  

・インラインにJavaScript, CSSを展開しないこと
  ```
    NG: <style></style>, <script></script>
  ```

・属性指定はダブルクオーテーション("")をつかうこと
  ```
    OK: <div class="pull-left"></div>
    NG: <div class='pull-left'></div>
  ```

・JavaScriptをonclickなどで呼び出さないこと
  ```
    NG: <a href="javascript:void(0);" onclick="func()"></a>
  ```

・a要素でhref属性を省略しないこと。JavaScriptで処理をする場合は "#" を入れておくこと
  ```
    OK: <a href="#">hoge</a>
    NG: <a href="">hoge</a>
  ```

・自作した.jsファイルはbody要素を閉じる直前に配置すること
  ```
    (...<script src="hoge.js"></script></body>)
  ```

## JavaScript

・処理がひとつの要素に対するものであればidを指定し、複数の要素に対して  
処理する場合はclassにjs-の接頭辞のついたクラスを指定し、明示的にすること。
  ```
    $('#modal').fadeIn(800);
    $('.js-colored-red').addClass('is-colored-red');
  ```
・ファイル名は単語の頭を大文字にすること
  ```
    ExampleHoge.js
  ```
・インデントはスペース2つにすること  
・文字列を表す場合はシングルクオーテーション('')を使うこと
  ```
    var text = 'Hello, world.';
  ```
・インラインで展開しないこと  
・変数宣言には必ずvar命令をつけること  
・文末は必ずセミコロン(;)をつけること
  ```
    var i = 0;
  ```
・定数はすべて大文字で記述し、単語間はアンダースコアでつなぐこと
  ```
    var CONST_HOGE = 'This is Sample.';
  ```
・関数はキャメルケースで記述すること  
  ```
    getMessageFromServer();
  ```
・値の比較は厳格に行うこと
  ```
    OK: if( '1' === 1) {} // This returns false
    NG: if( '1' == 1) {}  // This returns true
  ```
・eval()を使用しないこと  
・空の配列, オブジェクトの生成に（特別な理由がない限り）new演算子を使わないこと
  ```
    OK: 
      var obj = {};
      var arr = [];
    NG:
      var obj = new Object();
      var arr = new Array();
  ```
・意図がわかるようにコメントを記述すること


## CSS (SASS)

・ファイル名はハイフン(-)つなぎにすること
  ```
    example-sample.css (.sass)
  ```
・インデントはスペース2つにすること  
・スタイルの指定にid, js-のついたclassを使わないこと  
JavaScriptで処理する部分を明示的にするためにこれらを  
予約語としておき、通常のスタイル指定は使わないこと
  ```
  OK:
    .klass
      font-size: 14px;
  NG:
    #klass
      font-size: 14px;
    .js-klass
      float: left;
  ```

・単語の区切りは半角ハイフン(-)にすること
  ```
    .container-inner
    .container-inner-border
  ```

・IEの条件付きコメントを使用しないこと
  ```
    NG: [if IE], [if IE8]...
  ```

・モジュール設計をすること  
パーツ化することを目的にコーディングをすること  
HTML:
  ```
    <div class="module">
      <p class="module-text"></p>
      <p class="module-text is-hidden"></p>
      <p class="module-text-modifier"></p>
    </div>
  ```
CSS:
  ```
    .module
      width: 320px;
    .module-text
      font-size: 12px;
    .module-text.is-hidden
      display: none;
    .module-text-modifier
      font-size: 14px;
  ```

・モジュール外からCSSを適応しないこと  
モジュールのみにスタイルを適応すること  
ただし、JavaScriptの処理が入る場合この限りでない（*後述）。  
HTML:  
  ```
    <div class="container">
      <div class="module">
        <p class="module-text"></p>
        <a href="#" class="module-link"></a>
      </div>
    </div>
  ```
CSS:
  ```
    OK
      .module
        width: 320px;
      .module-text
        font-size: 14px;
    NG
      .container > .module
        float: left;
        width: 320px;
      .container .module .module-text
        font-size: 14px;
  ```

・JavaScriptによるレイアウト変更はレイアウト用のCSSを適応すること。  
JavaScriptによるレイアウト変更（カラム左右を変更など）には専用のCSSを作成し、適応すること。  
HTML:  
  ```
    <div class="container l-switched" id="switchPane">
      <div class="module-a">
        <p class="module-a-text"></p>
        <a href="#" class="module-a-link"></a>
      </div>
      <div class="module-b">
        <p class="module-b-text"></p>
        <a href="#" class="module-b-link"></a>
      </div>
    </div>
  ```
CSS:
  ```
    .module-a
      float: left;
    .module-b
      float: right;
      
    /* 左右カラム変更用 .l-switched */
    .l-switched
      .module-a
        float: right;
      .module-b
        float: left;
  ```

・コメントを記述すること  
JavaScriptや複雑なレイアウトの処理が入る場合や、緊急対応を実施した場合は、  
意図が分かりやすいようにコメントアウトで説明すること。  
  ```
    /* JavaScriptによるカラム制御
      .l-switchedが親要素に付いているときはカラムを左右入れ替える
    */
    .l-switched
      .module-a
        float: right;
      .module-b
        float: left;
  ```

・インデントを浅く保つこと  
インデントは2~3までとして、それ以上になる場合は、別モジュールにすることを考える。  
  ```
    OK:
    .module
      width: 320px;
      .module-item
        float: left;
   .link-module
      text-decoration: none;
      &:hover
        text-decoration: underline;
      &:active
        text-decoration: none;
      &:visited
        text-decoration: none;
        color: purple;
    NG:
    .module
      width: 320px;
      .module-item
        float: left;
        &> a
          text-decoration: none;
          &:hover
            text-decoration: underline;
          &:active
            text-decoration: none;
          &:visited
            text-decoration: none;
            color: purple;
  ```

・分けたモジュールが2ページ以上で使われる場合は、@importをつかって呼び出すこと
  ```
    /* common.css (sass) */
    .header
      width: 960px;
      height: 100px;
    
    /* example.css */
    @import "common"
    
    .example
      width: 320px;
      height: 100px;
  ```

・緊急対応用のCSSは別途分けておくこと  
緊急対応したCSS(sass)ファイルは別途分けておいて、StyleSheet読み込みの最後に読み込む。  
CSS:  
  ```
  /* fixme.css (sass) */
  .change-font-size
    font-size: 20px !important;
  ```
HTML:  
  ```
    <link rel="stylesheet" href="/css/common.css">
    <link rel="stylesheet" href="/css/page.css">
    <link rel="stylesheet" href="/css/fixme.css">
  ```
