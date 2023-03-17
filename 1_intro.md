# 1. はじめに: Introduction

私がお話しするエンジニアや科学者は、MATLAB と Python を "MATLAB **<u>vs</u>** Python" と考えています。本書の目的は、そんな彼らに "MATLAB **<u>with</u>** Python"として考えることが可能であることを証明することです。

[TIOBE index](https://www.tiobe.com/tiobe-index/)によると Python は最近、最も使われているプログラミング言語となりました。Python は本来は汎用的な言語で、特にウェブ開発、人工知能（機械学習・ディープ ラーニング）に使われることが多い言語です。

MATLAB は、技術計算のためのプログラミング言語であり、エンジニアや科学者のための開発環境というイメージが強いと思います。しかし、MATLAB は Python を含む多くのプログラミングと柔軟に双方向で統合することが可能です。

MATLAB は一般的な Python のディストリビューションで動作します。本書では Python 3.10 （ダウンロードはこちら [Python.org](https://www.python.org/downloads/)) と MATLAB R2023a を使用しています。

## 1.1. 科学計算の歴史: A brief history of scientific computing

### 1.1.1. 数値解析の祖: The roots of numerical analysis

1970 年代、Cleve Moler は [EISPACK](https://en.wikipedia.org/wiki/EISPACK)（固有値計算向け）と [LINPACK](https://en.wikipedia.org/wiki/LINPACK) （線形代数向け）と呼ばれるFortran ライブラリの開発に積極的に参加していました。ニューメキシコ大学の数学教授であった彼は、Fortran のラッパーコードを書き、コンパイルし、デバッグし、またコンパイルし、実行し...という作業を省き、これらのライブラリを学生にも利用できるようにしたいと考えました。

そこで彼は、Fortran で行列計算のための対話型インタプリタを作り、MATLAB（MATrix LABoratory の略、30年後に公開された映画「マトリックス」とは関係ない）と名付けた。この最初のバージョンは、EISPACK と LINPACK のいくつかのルーチンをベースにしたもので、80 の関数しか含まれていませんでした。

この写真は当時の MATLAB のマニュアルで、黎明期のソフトのカバー領域をうかがい知ることができます。

<img src="./media/image2.png" />

当時、MATLAB はまだプログラミング言語ではありませんでした。ファイル拡張子（m-scripts）もなく、Toolbox もない。利用できるデータ型は行列だけだった。グラフィックス機能は、スクリーンに描かれたアスタリスク（Astérix The Gaulではありません）でした。

<img src="./media/image3.gif" />

関数を追加するためには、Fortran のソースコードを修正し、すべてを再コンパイルする必要がありました。だから、ソースコードは必要だからオープンにしていました。（オープンソースが登場したのは80 年代、Richard Stallman とフリーソフトウェア運動から）

Cleve Moler がカリフォルニアのスタンフォード大学で行った数値解析の講座の後、MIT で訓練を受けたエンジニアが彼のもとにやってきました。「私は Cleve に自己紹介した」。Jack Little は、二人の最初の出会いについて、このように語っています。Jack Little は MATLAB の PC での利用価値を考え、C に書き換えます。彼は Steve Jobs や Bill Gates のように、パーソナルコンピューティングが IBM のメインフレームサーバービジネスに勝利することを確信していました。さらにはソフトウェアの機能を拡張するためのプログラムファイルを書く機能やツールボックスも追加し、堅牢なアーキテクチャで、モジュール化された拡張性の高いビジネスモデルを目指しました。そして 1984年、MATLAB を商業化するために（The）MathWorks 社を設立します。

**MATLAB の起源についてもっと知りたい場合は: Read more about the origins of MATLAB:**

-   A history of MATLAB – published in June 20202 -
    <https://dl.acm.org/doi/10.1145/3386331>

-   The Origins of MATLAB
    <https://www.mathworks.com/company/newsletters/articles/the-origins-of-matlab.html>

-   Cleve’s Corner – History of MATLAB Published by the ACM
    <https://blogs.mathworks.com/cleve/2020/06/13/history-of-matlab-published-by-the-acm/?doing_wp_cron=1642533843.1107759475708007812500>

### 1.1.2. 世界の別の場所では: In a parallel universe

1980年代、Guido van Rossum は [Centrum Wiskunde & Informatica](https://en.wikipedia.org/wiki/Centrum_Wiskunde_%26_Informatica)（CWI, 英語名: "National Research Institute for Mathematics and Computer Science"）で "ABC"と呼ばれる言語の開発に取り組んでいました。

"ABC は、プログラマーやソフトウェア開発者ではない（が、知的な）コンピュータユーザーに教えることができるプログラミング言語を目指していました。1970 年代後半、ABC の主な開発者は、そのような層に従来のプログラミング言語を教えていました。彼らの生徒には、物理学者から社会科学者、言語学者まで、超大型コンピュータの使い方を必要としているさまざまな科学者がいました。しかし、そのような生徒たちは、プログラミング言語が持つ制限や制約、恣意的なルールに戸惑っていました。このようなユーザーの声をもとに、ABC の設計者は別の言語を開発しようとしたのです。"

1986 年、Guido van Rossum は CWI の別のプロジェクト、Amoeba Poject に写ります。Amoeba は分散型オペレーティングシステムであった。1980 年代後半には、スクリプト言語が必要であることに気が付きます。このプロジェクト内で与えられた自由な発想で、Guido van Rossum は自分の「ミニプロジェクト」を立ち上げます。

1989 年 12 月、Guido van Rossum は、休暇中の「クリスマス前後の 1 週間を退屈しないように『趣味の』プログラミングプロジェクト」を探していたところ、「最近考えていた新しいスクリプト言語：Unix/C ハッカーにアピールできる ABC の子孫」のインタプリタを書くことにします。Python という名前を選んだのは、「ちょっと不遜な気分で（Monty Python's Flying Circus の大ファンで）」だとしています。(“Programming Python” Guido van Rossum, 1996)](https://www.python.org/doc/essays/foreword/)』序文より(minoue_xx 訳)

そして Guido van Rossum はシンプルな仮想マシン、シンプルなパーサー、シンプルなランタイム、基本的な構文を作成し、文のグループ分けにインデントを使用します。そして、辞書、リスト、文字列、数値という少数のデータ型を開発。Python の誕生です。

Python の成功に最も貢献したのは、Python を簡単に拡張できるようにしたことだと Guido は考えています。

**Main milestones of the Python language:**
- 1991: Python 0.9.0 published to alt.sources by Guido Van Rossum
- 1994: Python 1.0. include functional programming (lambda’s map, filter, reduce)
- 2000: Python 2.0 introduces list comprehension and garbage collection
- 2008: Python 3 fixes fundamental design flaws and is not backward compatible
- 2022: Python 2 is end of life, last version 2.7.18 released


**Read more about Python:**

-   The Making of Python - A Conversation with Guido van Rossum, Part I
    <https://www.artima.com/articles/the-making-of-python>

-   Microsoft Q&A with Guido van Rossum, Inventor of Python  
    <https://www.youtube.com/watch?v=aYbNh3NS7jA>

-   The Story of Python, by Its Creator, Guido van Rossum  
    <https://www.youtube.com/watch?v=J0Aq44Pze-w>

-   Python history timeline infographics  
    <https://python.land/python-tutorial/python-history>

## 1.2. 著者について: About the author

私の名前は Yann Debray で、MathWorks で MATLAB プロダクトマネージャとして働いています。私が MATLAB を売り込もうとしているという点で、偏った意見を持っていると思われるかもしれません。それは間違いではありません。ただ私の動機をよりよく理解してもらうために、私の経歴をもう少し深く紹介します。

私は 2020 年 6 月（COVID-19パンデミックの真っ最中）に MathWorks に入社しました。それ以前は、[Scilab](https://scilab.org/) というプロジェクトに 6 年間携わりました。Scilab は、MATLAB に代わるオープンソースのソフトウェアです。この経験が、オープンソースや科学技術計算に対する私の意欲につながったっています。

数値計算との出会いは、2013 年 12 月、[Claude Gomez](https://www.d-booker.fr/content/81-interview-with-claude-gomez-ceo-of-scilab-enterprises-about-scilab-and-its-development) に初めて会ったときでした。彼は当時、Scilab Enterprises の CEO で、Scilab を研究プロジェクトから企業にした人物でした。ビジネスモデルは、Linux を中心としたサービスを販売する Red Hat からヒントを得ています。

私は科学技術計算においてオープンソースを持続可能なモデルにすることの難しさをよく理解しています。それが、オープンソースとプロプライエタリなソフトウェアの間の力の均衡を信じる理由です。すべてのソフトウェアがフリーになれるわけではありません。シミュレーションのような分野では、何十年もの投資を必要とする専門知識が必要ですが、知的財産権によって駆動されるエンジニアリングソフトウェアの市場は今後何年にもわたって続くでしょう。

## 1.3. Open-source vs Commercial

*商品化するのか、それともオープンソースにするのか*は、この本を執筆する際に考えたことの 1 つでした。私は、本を書くとはどういうことなのか、理想的な考えを持っていました。名声や華やかさ。しかし、現実的には、かなりニッチな内容なので、たくさん売れるわけがないとわかっています。私の予想では、MATLAB の 500 万人のユーザーのうち、30 ％程度の Python にも興味がある人がターゲットになると思います。オープンソースに対する理想論だけでなく、このプロジェクトをやり遂げるには具体的なモチベーションが必要だと感じていました。だから、この本のハードコピーを販売するというのが最初のアイデアでした。しかし、私の親愛なる同僚で親友の Mike Croucher は、彼が言うところの「枯れ木」になってはいけないと忠告してくれました。MATLAB の新バージョンが出るたびに（年に2回）、印刷されたコンテンツはすぐに陳腐化してしまうという事実を示唆しています。最終的に、コンテンツをオープンソース化することは、有料版の本をリリースすることと矛盾しないと判断しました。実際、技術書を買うときはオープンソースライセンスを適用している人に決めることが多いです。

## 1.4. 本書が向いている方: Who is this book for?

もしあなたが以下のシナリオに心当たりがあるなら、本書はあなたのためのものです。

Python について耳にすることが多くなってきた MATLAB を使用しているエンジニアや研究者の方。特にデータサイエンスや人工知能に関連するテーマではその傾向が顕著です。オンラインでコードを検索していると、Python で書かれた面白いスクリプトやパッケージに出くわすことがあります。また、Python を使っている同僚と協調する方法を探しているかもしれません。

<img src="./media/image5.png" />

あなたはデータサイエンティスト（もしくは、なりたいと思っている）で、科学的/工学的データ（無線、オーディオ、ビデオ、レーダー/ライダー、自律走行など）に取り組んでいます。データ処理に関連する日常業務の一部には Python を使用するでしょうが、AI ワークフローのエンジニアリング部分には MATLAB を検討しているかもしれません。（特に組み込みシステムに実装される場合）。この部分はエンジニアの同僚が担当するとすると、彼らが共有するモデルやスクリプトを実行できるようにしたいかもしれません。

<img src="./media/image6.png" />