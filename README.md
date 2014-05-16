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


## CSS (SASS)

### スタイルの指定にidを使わないこと
eg.
  ```
  OK:
    .klass
      font-size: 14px;
  NG:
    #klass
      font-size: 14px;
  ```

### 単語の区切りは半角ハイフン(-)にすること
eg.
  ```
    .container-inner
      width: 960px;
    .container-inner-border
      border: 1px solid #ccc;
  ```

### モジュール設計をすること
パーツ化することを目的にコーディングをすること  
eg.  
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

### モジュール外からCSSを適応しないこと
モジュールのみにスタイルを適応すること  
ただし、JavaScriptの処理が入る場合この限りでない（*後述）。  
eg.  
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

### JavaScriptによるレイアウト変更はレイアウト用のCSSを適応すること
JavaScriptによるレイアウト変更（カラム左右を変更など）には専用のCSSを作成し、適応すること。  
eg.  
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

### コメントを記述すること
JavaScriptや複雑なレイアウトの処理が入る場合や、緊急対応を実施した場合は、  
意図が分かりやすいようにコメントアウトで説明すること。  
eg.
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

### インデントを浅く保つこと
インデントは2~3までとして、それ以上になる場合は、別モジュールにすることを考える。  
eg.  
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

### 分けたモジュールが2ページ以上で使われる場合は、@importをつかって呼び出すこと
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

### 緊急対応用のCSSは別途分けておく
緊急対応したCSS(sass)ファイルは別途分けておいて、StyleSheet読み込みの最後に読み込む。  
eg.  
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
