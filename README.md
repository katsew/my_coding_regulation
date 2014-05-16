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
  ```

・自作した.jsファイルはbody要素を閉じる直前に配置すること
  ```
    (...<script src="hoge.js"></script></body>)
  ```


## CSS (SASS)

### スタイルの指定にidを使わないこと
eg.
  ```
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
パーツ化することを意識したコーディングをする  
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
緊急の場合を除き、モジュールのみでレイアウトを完了すること
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
    NG
      .container .module
        width: 320px;
      .container .module .module-text
        font-size: 14px;
    OK
      .module
        width: 320px;
      .module-text
        font-size: 14px;
  ```

### インデントを浅く保つこと
インデントは2~3までとして、それ以上になる場合は、別モジュールにすることを考える。  
eg.  
CSS
  ```
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
    OK:
    .module
      width: 320px;
      .module-item
        float: left;
        
     .module-link
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
  

### hotfixなcssは別途分けておく
緊急対応したCSS(sass)ファイルは別途分けておいて、最後に読み込む。  
eg.  
CSS:  
  ```
  /* fixme.css (sass) */
  .change-font-size
    font-size: 20px !important;
  ```
HTML:  
  ```
    <link rel="stylesheet" href="/css/hoge.css">
    <link rel="stylesheet" href="/css/fixme.css">
  ```
