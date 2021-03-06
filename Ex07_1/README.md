# Ex07_1

### 概要
掲示板のWebアプリケーションです。

### 技術要素

 * Servlet
 * JSP

### リファレンス

 * 『スッキリわかるサーブレット&JSP入門』
   * P.21～P.186

### Git
* 作業ブランチ
  * develop/Ex07_1
* MergeRequestタイトル
  * Ex07_1

---

## 課題

Servletおよび、JSPを使用して掲示板のWebアプリケーションを作成しなさい。

### 画面概要および画面遷移図

![画面](img/ex07_1.png)

### サーブレットクラス(4つ)

サーブレットクラスはすべて「`jp.co.axrossroad.sup.ex0701.servlet`」パッケージ内に作成すること。

 * 記事一覧画面表示サーブレット
   * `ArticleListServlet`
 * 記事詳細画面表示サーブレット
   * `ArticleDetailServlet`
 * 記事作成画面表示サーブレット
   * `ArticleNewServlet`
 * 新規記事登録サーブレット
   * `ArticleRegisterServlet`

### JSPファイル(3つ)

JSPはすべて「src/main/webapp/WEB-INF/views」ディレクトリ内に作成すること。

 * 記事一覧画面JSP
   * `article_list.jsp`
 * 記事詳細画面JSP
   * `article_detail.jsp`
 * 記事作成画面JSP
   * `article_new.jsp`

### JSPへの直接アクセス禁止

WEB-INFディレクトリ以下にJSPを配置しているので、ブラウザ上でJSPファイルまでのURLを指定して直接JSPを呼び出すことはできない。JSPはサーブレットからのフォワードでのみ呼び出すことが可能である。

### 入力チェック処理

必要な入力制限、入力チェックは各自実装すること。

### エラー発生時

エラー発生(例外発生)時も考慮し、可能な限りユーザにエラーメッセージを表示すること(Javaのスタックトレースがブラウザに表示されないように)。

### データベース

あらかじめ[20.データベース環境の準備と使用](https://sites.google.com/a/axrossroad.co.jp/startupproject/home/new_progtraining/db_prepare)
でDBの準備をしておくこと。Ex06_1で準備が終わっていれば、やらなくていいです。

「T\_ARTICLE」表を使用します。CREATE TABLE文はsqlディレクトリの中の「ex07\_1.ddl」にあります。

Gradleビルドスクリプトの設定により、JDBCドライバファイルは自動的に設定されます。

### getRequestDispatcherのパス文字列

* フォワード先がJSPの場合のパス文字列は「/WEB-INF/views/_フォワード先JSPファイル名_」
* フォワード先がサーブレットの場合のパス文字列は「_フォワード先サーブレットクラスの@WebServletに設定したURL_」

### formタグのaction属性

JSP(HTML)のformタグに設定するaction属性の値は「_送信先サーブレットクラスの@WebServletに設定したURLから先頭の"/"を削除したURL_」

### CSSファイルの格納先

CSSファイルは「src/main/webapp/css」ディレクトリ内に配置すること。JSP(HTML)からの読み込む際は「`<link rel="stylesheet" type="text/css" href="css/CSSファイル名">`」

---

## 進め方

__※レビュー依頼は「レベル〇まで作成しました」文言を入れること__

レベル1⇒4の順で機能を作成すること。

各レベルが完成(動作OKでレビュー前)した段階で、リーダーに報告する(この時点でコミット・プッシュしておくこと)。

リーダー確認の後、次のレベルへ進むこと。

⇒ __予定の締め切りまでに最低でもレベル1は終わらせること。__

 * レベル1
   * 記事一覧画面の表示 article_list.jsp、ArticleListServlet
 * レベル2
   * 記事詳細画面の表示  article_detail.jsp、ArticleDetailServlet
 * レベル3
   * 記事作成画面の表示 article_new.jsp、ArticleNewServlet
 * レベル4
   * 記事登録機能 ArticleRegisterServlet
 * レベル99
   * Bootstrapを使ったイケてるWebフロントへの改造

---

## 戦略

これまでの課題よりボリュームも大きく、扱う技術要素も複数あるため戦略を立てて取り組む。「レベル1」攻略の戦略の一例を以下に示す。


1. 記事一覧画面JSPを用意する。ただし、スクリプトレット(JSPに組み込むJavaコード)は書かず、単純なHTMLだけの状態としておく。
1. 記事一覧画面表示サーブレットを用意する。サーブレットの`doGet`メソッドに、記事一覧画面JSPへのフォワード処理のみを書いておく。
1. 記事一覧画面表示サーブレットを実行して、記事一覧画面JSPへフォワードされ、画面が表示できることを確認する。⇒ __フォワードの動きの確認OK__
1. 記事一覧画面表示サーブレットで、フォワード処理の前に画面に受け渡すデータを用意する処理を作成する。このときのデータは、ダミーの単純な文字列データ1つを用意して、`setAttribute`でJSP側に受け渡す。
1. 記事一覧画面JSPの中で、記事一覧画面表示サーブレットで`setAttribute`した文字列を`getAttribute`で取り出し、画面に出力するスクリプトレットを作成する。
1. 記事一覧画面表示サーブレットを実行して、サーブレット→JSPに値が受け渡され、画面に表示されることを確認する。⇒ __単純な値受け渡しの確認OK__
1. 記事一覧画面表示サーブレット、 記事一覧画面JSPのダミー文字列受け渡し処理を削除し、今度は本来受け渡すべき値の処理を作成する。このとき、データはDBから取得するのではなく、プログラムの中でダミーのデータを用意する。一覧データであれば、1行分のデータを格納するBeanクラスを用意し、そこにデータを詰めリストに追加して、`setAttribute`でJSP側に受け渡す。
1. 記事一覧画面JSPの中で、記事一覧画面表示サーブレットで`setAttribute`したオブジェクトを`getAttribute`で取り出し、画面に出力するスクリプトレットを作成する。
1. 記事一覧画面表示サーブレットを実行して、サーブレット→JSPに値が受け渡され、画面に表示されることを確認する。⇒ __仕様通りの値受け渡しの確認OK__
1. 記事一覧画面表示サーブレットのデータ設定前の箇所に、DBからデータを取得する処理を作成する。取得がうまく行っているか、標準出力(`System.out.println`)へ取得データを出力して確認する。DBのテーブルには、手動であらかじめデータを数レコード登録しておく。⇒ __DBからデータ取得の確認OK__
1. 記事一覧画面表示サーブレットのダミーデータを設定していた部分を、DBから取得したデータを使う処理に置き換える。⇒ __DB→サーブレット→JSPの一連の流れの確認OK__