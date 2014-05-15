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
・自作した.jsファイルは</body>の直前に配置すること
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
.container-inner
.container-inner-border

### モジュール設計をする
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

### インデントを浅く保つ
インデントは2~3までとして、それ以上になる場合は、別でモジュールにすることを考える。
ただし、hoverなどの擬似クラスはこの限りでない
.module
  .module-item
    &> a
      &:hover
      &:active
      &:visited


### hotfixなcssは別途分けておく
緊急対応したCSS(sass)ファイルは別途分けておいて、最後に読み込む
eg.

/* fixme.css (sass) */
.change-font-size
  font-size: 20px !important;

<link rel="stylesheet" href="/css/hoge.css">
<link rel="stylesheet" href="/css/fixme.css">