"Test Driven Development with Python" 学習用ノート
#####################################################

個人的な復習用にノートを作成しています。日本語を母語とする読者が本書購入検討のをする際に助けになれば幸いです。

* まえがき

  * Python 3を利用する
  * HTMLとJavaScriptに関して少し理解があることを求める
  * 必要なソフトウェアのインストール

    * Firefox

      Seleniumが標準で操作できる

    * Git

      Windowsの場合Git for Windowsに付属するGit Bashがシェルとして有用

    * Python 3

      WindowsやMacは公式のインストーラを使用する

    * Gitのデフォルトエディタと基本的な設定(git config --global user.name と git config --global user.email)
    * pip(pip3)でパッケージ導入

      * django
      * selenium

  * IDEは使わないことを推奨

    この本が終わったら使いましょう。

* Part I. The Basics of TDD and Django

  コピペは避けよう。写経して"your muscle memory"に刻み込んだほうがいい。どうせPDFからコピペしてもろくなことにならないしね。

#. ファンクショナル・テストを使ってDjangoをセットアップしよう

  * Testing Goatに従え
  * functional_test.pyというファンクショナル・テスト用のスクリプトを作成する

    * seleniumでFireFoxの起動とページの表示を行う
    * assertでページタイトルをテストする。"Django"の文字列が含まれるか
    * 最初に書いたテストは予想通り失敗する。これでアプリを作り始めてよい

  * $ django-admin.py startproject [projectname]でプロジェクト作成。プロジェクト名でネストされたディレクトリができるが混乱しないように。上位のプロジェクト名ディレクトリが作業ディレクトリになる
  * manage.py はDjangoにおける便利道具
  * $ python3 manage.py runserverでローカルでDjangoが走る
  * 今度のfunctional_test.pyはFirefoxを起動する。Djangoが動いている。何も出力しない
  * 成果をgitにコミットする

    * gitリポジトリを初期化する
    * "db.sqlite3","___pycache__","\*.pyc"を.gitignoreに加える。すでに"git add"したものは"git rm --cached"する
    * git statusしてからgit addおよびgit commit

#. unittestモジュールを使ってファンクショナル・テストを拡張する

  * To-Doリストアプリをつくることにする
  * ファンクショナル・テスト(呼び方は色々ある)は人間が読めるストーリーを持っている必要がある
  * コメントの形でfunctional_test.pyに最小限の利用ストーリーを書く。テストは今まで書いたものをストーリーに当てはめ、少し変更した部分以外は足していない
  * しかし変更部分があるのでテストは失敗する
  * assertのみでは表示がわかりにくく、またテストの前処理や後処理に不便なのでunittestモジュールを使う
  * functional_test.pyにunittestモジュールを導入する

    * unittest.TestCaseを継承したクラス
    * setUp(self)とtearDown(self)
    * test_で始まる名前を持ったメソッド
    * unittestのassertInやfail
    * "if __name__ == '__main__':"
    * unittest.main(warnings='ignore')の引数は省略可能。試す

  * Seleniumのimplicitly_wait()
  * gitにcommmitする

    * git status
    * git diff
    * git commit -a

#. ユニットテストによるシンプルなホームページのテスト

  * $ python3 manage.py startapp [appname] で新しいアプリをセットアップする
  * Djangoのプロジェクトは複数のアプリを含むことができる
  * サードパーティーのアプリを使ったり、自分のアプリを別のプロジェクトに再利用できる
  * ファンクションテストはアプリケーションを外側からユーザーの視点でテストする
  * ユニットテストは内側からプログラマーの視点でテストする

    #. ユーザーの視点でファンクショナルテストを書く
    #. ファンクショナルテストが失敗したら、どのようにそれをパスするコードを書くか考え、一つ以上のユニットテストを書く
    #. ユニットテストに失敗したら、それに通る最小限のアプリケーションコードを書く。2と3を繰り返す
    #. ファンクショナルテストを通るまで繰り返す

  * ユニットテストは先ほど作成したアプリのディレクトリに生成されたtests.pyに書く
  * Djangoによってunittestを拡張したdjango.testのTestCaseを利用する
  * ファンクショナルテストは直接functional_test.pyを実行していたが、tests.pyは
    $ python3 manage.py testでを実行するとDjangoのtest runnerが実行する
  * gitにcommitする

    * git status
    * git add [appname]
    * git diff --staged
    * git commit -m "comment"

  * DjangoのURLとViewをテストする
  * django.core.urlresolversからresolveを
  * django.textからTestCaseを
  * [appname].viewsから[home_page(関数のスタブ)]をそれぞれimportする
  * resolve('/')でサイトのrootのviewをマップする
  * [appname].viewに実装する関数とresolve('/')の関数をassertEqualする
  * $ python3 manage.py test 関数のimportに失敗する
  * ImportErrorに従って[appname].views.pyにhome_page = Noneを追記する。冗談のように見えるがTDDのデモとして行う
  * $ python3 manage.py test 今度のエラーはresolve('/')によるものである
  * [projectname]/urls.py のulrpatternsに正規表現とviewになる関数のマッピングを記入する
  * 今回は[projectname]/ulrs.pyにfrom [appname] import viewsでimportし、
    r'^$'(空の文字列)とviews.home_pageをマッピングする
  * 正規表現は勉強しよう
  * $python3 manage.py test AttributeErrorが出る。home_pageがNoneTypeなため
  * [appname]/views.pyのhome_pageを中身がpassのみの空の関数 home_page()に変更する
  * $python3 manage.py test パスする。
  * gitにcommitする

    * git diff
    * git commit -am "comment" これは最後のcommitに関するバリエーション。
      tracked files全てをcommitメッセージとともにcommitする。
      新しいファイルがあればgit addする必要がある。

  * Viewのユニットテストを行う
  * [appname]/test.pyのユニットテストにHTMLをテストするコードを追記する
  * test_で始まる新たな関数でHTMLを検証する
  * そのためにfrom django.http import HttpRequestをインポートする
  * HttpRequest()で生成したrequestオブジェクトをhome_page(request)
    してresponseを得る。response.contentにHTMLがあるのでこれをテストする。
  * テスト。TypeError。home_page()は引数がない。[appname]/view.pyを書き換えて
    home_page(request)とする
  * テスト。AttributeError。home_page(request)がそのままreturn HttpResponse()する
    ように書き換える
  * テスト。AssertionErrorとなる。home_page(request)の返り値であるHttpRequest()に
    return HttpRequest('<html>')の形でアサーションの対象になる文字列を足していく。
  * $ python3 manage.py test にてエラーなしとなる
  * $ python3 functional_test.py も意図的にfailするようにした箇所までパスする。
  * gitにcommitする

    * git diff
    * git commit -am "comment"
    * git log --oneline

  * まとめ

    * Django appの開始
    * ファンクショナルテストとユニットテストの違い
    * DjangoのURL解決とurls.py
    * Djangoのview機能（関数？)、requestオブジェクトとresponseオブジェクト
    * 基本的なHTMLの出力

  * 便利なコマンドとコンセプト

    * $ python3 manage.py runserver Djangoの開発サーバー起動
    * $ python3 functional_test.py ファンクショナルテストの実行
    * $ python3 manage.py test ユニットテストの実行
    * ユニットテストとコーディングのサイクル

      #. ターミナルでユニットテストを実行する
      #. エディタで最小限のコード変更を行う
      #. 繰り返す!

#. 私たちはこれらのテストで何をしているのか?
    * テスト多い問題

        * テストが詳細、瑣末なものに感じられるかもしれない。home_page = Noneって本気か？
        * 実際の開発でもこのようなコードを書くのか？
        * プログラムは困難である。私たちはほとんどのときに賢くうまくいくが、TDDは
          私たちがさえない時に助けてくれる
        * Kent Beck曰く「プログラミングは井戸からロープで水バケツをくみ上げるようなものだ」
        * TDDは作業の進捗のラチェット（逆転防止歯車）となってあなたを助けてくれる
        * これであなたはいつでも賢くなくてもよくなる
        * それでもやりすぎじゃない？
        * 訓練のためがんばりましょう
        * 「些細な関数をテストするメリット」単純な関数や定数にテストを書いてますが
        * この本では格闘技の型のように網羅されたTDDを提示している
        * 実践では問題はもっと複雑だから、簡単な問題を放置しているうちに
          問題は難しくなり茹でガエルになってしまう
        * 簡単な関数にテストを書くことはいいプレースホルダーになる。精神的な
          障害も少なくなる。単純な関数が複雑になるのにつれて、自然にテストも
          成長させる。

    * ユーザー操作のテストにSeleniumを使う

        * $ python3 functional_tests.py を実行して意図的なFailを確認する
        * うまくいかない場合は manage.py runserverをしているか確認する
        * functional_test.pyにinputタグからの投稿やTo-Doリストへの反映を
          テストするテストを追記する
        * seleniumのfind_element_by...やsend_keys、Keysを使いました。
        * seleniumの find_element_by_tag_nameやfind_element_by_idと
          複数形のsがついたfind_elements_by_tag_nameの違いに注目。
          前者は一つも要素が見つからないと例外となる、後者は空のリストを返す。
          (これ、実践Selenium WebDriverでやったところだ！)
        * generator expression(ジェネレータ式)が出てきています。Pythonに
          慣れていない人はググるかGuidoの親切なガイドを見てね
        * http://python-history.blogspot.co.uk/2010/06/from-list-comprehensions-to-generator.html
        * テストは失敗する。functional_test.pyにまだページにない要素を足してある
        * たくさんFTに追記したのでgitにcommitする

            * git diff
            * git commit -am "comment"

    * 「定数をテストするな」ルール、そして救済のためのテンプレート
        * ユニットテストはロジックや制御のフロー、設定をテストする
          HTML文字列をテストしている今の私たちのそれはそうなっていない
        * 機能を変更することなくコードを改善する、それがリファクタ
        * ユニットテストを通ることを確認する
        * テンプレートを使ってのリファクタリング

            * [appname]/templates ディレクトリを作成する
            * [appname]/templates/home.html としてHTMLを書く。
              ユニットテストに書いていたのと同じHTML。
            * [appname]/views.py に django.shortcutsからrenderをインポートし
              return render(request, 'home.html')でテンプレートを使う。
             * HttpResponseからDjangoのrender機能に変更した
               これは第一引数がrequest（理由は後で）で次がrenderに渡すテンプレートの
               名前になる。Djangoは自動的に全てのアプリのtemplatesと呼ばれるフォルダ内を
               検索する。そしてテンプレートに基づいたHttpResponseがビルドされる。
            * テンプレートはDjangoの強力な機能で、Pythonの変数をHTMLに埋め込める。
              あとの章でもrenderやrender_to_string(ディスクからファイルを読み込む)
              を使う理由である。
            * ところでこれはユニットテストに通らない
            * トレースバックを読む練習
            * Djangoはテンプレートを見つけられない。templatesフォルダに配置しているのに
            * 実は作成したアプリを公式には、まだDjangoに登録していなかった。
            * つまり、startapp コマンドだけでは不十分ということ。
            * settings.pyのINSTALLED_APPSにアプリを追記する。末尾のコンマを忘れないこと
            * まだテストに通らない。これはユニットテストではHTMLの末尾に改行などがないからである。
            * self.assertTrue(response.content.strip().endwith(b'</html>'))のようにする。
            * ユニットテスト側を変更しているので小さなずる(cheat)だがHTML末尾の空白なんかどうでもいいこと。
            * もう一押し。定数としてのHTMLではなく正しいテンプレートがレンダリングされることを
              確認するテストに書き換える
            * render_to_stringを使用する(この文脈はP.56で助けになる。初回は見落とした)
            * response.contentのバイト列をPythonのユニコード文字列にするため.decode()も使う

        * リファクタリングにおいて

            * Kent Beck曰く「この通りにするよう勧めているのか？違う、このようにできることを勧めている」
            * リファクタリングするときは、コードかテストについて作業すべきで、同時にその両方をしてはいけない
            * この作業をとばしたくなるかもしれないが、多くのファイルを編集して何をやるべきか
              見失いがちになる。リファクタリング・キャットみたいなことになるまえに、ちょっとづつ
              作業しよう。
            * リファクタリング・キャットはまた本書で現れる。
              多すぎる変更を一度にしようとするときまた現れる。
            * リファクタリング・キャットはテスティング・ゴートの反対側の肩に乗っていて
              漫画の悪魔のように、あなたに悪いアドバイスを与えるのだ...

        * リファクタリングが終わったらいい頃合い
        * gitにcommitする

            * git status
            * git add . #templatesフォルダが追跡されていないので
            * git diff --staged
            * git commit -m "comment"

        * テンプレートに幾つかの要素を追加し、追加のたびにFTを回す。詳細は省略
        * まとめ：TDDプロセス

            * ファンクショナルテスト
            * ユニットテスト
            * ユニットテスト/コーディングのサイクル
            * リファクタリング
            * テストを書く、コードを書く、テストが通れがリファクタリングする
            * 二重ループのように、ファンクショナル・テストを書いた後、
              コードを書く、のプロセスの中にユニットテストとコード、リファクタリングが入っている。

#. ユーザー入力の保存
