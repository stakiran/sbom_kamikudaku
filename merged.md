# Chapter-0 はじめに

# 本書について
本書は SBOM を噛み砕いて解説したものです。

無料のオンラインドキュメントであり、筆者が自身の備忘録のために整理したものを公開しています。コンセプトは「あまり手間をかけずにざっくりと理解する」です。噛み砕くと題したとおり、わかりやすい解説を心がけました。その代わり、ざっくりの域を出ることができません。厳密性や専門家向けの高度な詳細は[参考文献](ref)に譲ります。

本書は三つの部から構成されています。SBOM を知らない方向けの基本事項から、知っている方向けの実践的な内容まで幅広く扱っています。

想定読者は以下のとおりです。

- プログラマー含むソフトウェア開発者（以降 **エンジニア** と書きます）の方
    - 第一部で概要をつかむ
    - 第二部で扱い方をつかむ
- 非エンジニアの方
    - 第一部で概要や前提知識をつかむ
- SBOM に関するビジネスを検討している方や読み物を探している方
    - 第三部でヒントやインスピレーションを漁る
- SBOM に関するビジネスや啓蒙の推進者を探している方
    - 本書を読んで筆者の実力を図る

# 注意事項
- 本書は筆者個人の見解であり、所属組織とは一切関係がありません。
- 本書の内容に基づく運用結果について、筆者は一切の責任を負いかねますので、ご了承ください。
- 本書に記載されている手法名、会社名および製品名は、各社の商標または登録商標です。なお、本書中では ™、®、© マーク等は表示していません。

# 筆者について
吉良野すた(sta, stakiran)と申します。

- ホームページ
    - [吉良野すたホームページ](https://stakiran.github.io/stakiran/)
- ブログ
    - [stakiran研究所](https://scrapbox.io/sta/)

# お問い合わせ
以下から可能です。

- 筆者ホームページのお問い合わせ先
- GitHub Issues
    - [stakiran/sbom_kamikudaku: 書籍「SBOMを噛み砕く」用](https://github.com/stakiran/sbom_kamikudaku/tree/master)

# 更新履歴
- 2024/05/31 v1.1
    - 試験的に GPTs を追加（付録1）
- 2024/05/29 v1.0
    - 初版

# Chapter-1 📘第1部「SBOM をざっくり理解する」

SBOM を知らない方向けに、概要や状況をざっくり解説します。また、はじめに前提知識が無いと思われる非エンジニアの方向けの解説も用意しています。

# Chapter-2 前提知識

SBOM を知るために必要な前提知識。

# ソフトウェアについて
アプリ（アプリケーション）、Webサービス、ツール、コマンドなどはソフトウェアとして動作しています。

このソフトウェアはプログラムを書く（ **プログラミング** ）ことでつくります。自然言語で小説を、楽譜で音楽をつくりますが、プログラミング言語でソフトウェアをつくるのです。

だからといって一からプログラミングするというと、そうじゃありません。 **電子的な世界であり複製（コピペ）がしやすいので、「誰かがつくった便利プログラム（便利パーツ）」を拝借すれば楽をできます**。パーツを組み合わせて新たなソフトウェアをつくるのです。そういう意味では、プログラミングはパズルゲームみたいなものです。

さて、現代のソフトウェアは、便利パーツを何十何百何千と使うのが当たり前となっています。そしてその便利パーツもまた何十何百何千というパーツを使っていて――と気が遠くなるほどの積み重ねがあります。特に **インターネット** と「便利なパーツはボランティアで無料でどんどん広めるべきだよね」的な価値観（ **オープンソース** ）のおかげで、わずか 50 年でここまで発展しました。GAFAM などデジタル企業が世界を席巻できているのは、ソフトウェアという世界だけがこの恩恵にあやかれているからです。そりゃ世界中のエンジニアがボランティアで協力しまくってるんだから最強だよね、ということです。最近では生成AIも生まれて世間を賑わせていますが、これもソフトウェアの恩恵です。ソフトウェアの天下はまだまだ続くでしょう。

まとめ:

- ソフトウェアは多くの便利パーツから成る
- 現代ではプログラミング≒パズルゲーム

# ソフトウェアとライブラリ
便利パーツのことを **ライブラリ(Library)** と呼びます。

ライブラリもまたソフトウェアの一つです。アプリ、Webサービス、ツール、コマンド、ライブラリ――全部ソフトウェアです。アプリは普段皆さんがスマホやパソコンから使うものですが、ライブラリはエンジニアがパーツとして使うものです。目的や使い方が違うのですね。

つまり **ソフトウェアとは「多数のライブラリから成るもの」** と言えます。

まとめ:

- ソフトウェアとは多くのライブラリから成るもの

# SBOM について
本題に入りましょう。

**SBOM** とは、あるソフトウェアが「どんなライブラリから成っているか」を記した一覧のことです。Software Bill of Materials の略で、和訳すると「ソフトウェアの部品票」です。ライブラリという部品を一覧化した票です。ちなみに **エスボム** と読みます。

たとえば 307 個のライブラリを使っているとするなら、そのソフトウェアの SBOM には 307 項目が記載されます。要はただのリスト、ただの一覧です。何も怖くない。

ただし、載せる情報が色々あるため、やたら煩雑に見えます。たとえばライブラリには名前、バージョン、開発者、入手先URL、ライセンス（利用規約的な条項文）……など色んな属性があります。1 項目分を表現すると、たとえばこうなります:

```
vuejs-core, v3.4.27, Yuxi (Evan) You, https://github.com/vuejs/core, MIT License
...
```

こういう情報が 307 項目分並んでいたとしたら――目が眩みそうですよね。ただ、それでも本質はただのライブラリのリストにすぎません。

まとめ:

- SBOMとはただの一覧
    - 使っているライブラリ（部品）を一覧化した票

# 脆弱性とは
SBOM とセットで語られる脆弱性についても説明しておきます。

**脆弱性(Vulnerability)** とは、あるソフトウェアに「攻撃されうる弱点」が見つかった、というものです。「あるソフトウェア」と「それが持つ弱点」のセットで語られます。

弱点の何が問題かというと、弱点を突かれて悪さをされるからです。管理者権限を奪われて好き勝手に壊されたり盗まれたりします。現実では人間には理性や倫理がありますし、法もありますから、（治安や状況にもよりますが現代の日本では基本的に）悪さはあまり発生しませんが、コンピュータの世界は違います。先ほど、電子的な世界は複製（コピペ）がしやすいと書きましたが、他にもデータの操作や加工や自動化もしやすかったりします。現実でひとりの人間が何件もの家に強盗で押し入るのは難しいですが、コンピュータだとできてしまいます。何ならプログラムを書いて自動化すれば、自分は何もしなくても何千何万というデータを盗んだり壊したり、もできちゃいます。悪事もバレにくく、捕まりにくいです。現実よりも行動しやすい世界観なのです。だからこそ、**弱点が見つかればあっという間に広められ、突かれてしまいます**。

さて、ここで問題です。そんな「弱点を持つライブラリ」を使っていたらどうなるでしょうか――答えは「弱点を突かれる可能性がある」です。危ないですね。極端なことを言えば、仮に 307 個のライブラリを使っているとしたら、これは「弱点を持ちうるソフトウェアを 307 個使っているに等しい」わけです。弱点はそうそう見つかるものではありませんが、見つからないとは言えません。交通事故や殺人が世の中で起きているように、脆弱性もそれなりに見つかります。決して他人事ではないのです。

まとめ:

- 脆弱性とは、あるソフトウェアに攻撃されうる弱点が見つかったというもの
- 電子的な世界では悪さをしやすいので、脆弱性を放置していると危ない

# 脆弱性に対処する
脆弱性に対処するには、どうすればいいでしょうか。

自分のソフトウェアが「弱点を持つライブラリ」を使ってるかどうかを見ればいいのです。もし使っていたなら、何とかして弱点を潰します。たいていはライブラリ側で「弱点を潰した最新版をつくりました」をしてくれるので、それを適用します。状況によっては「弱点が出てない古い版を使おう」とか「設定値を工夫したら回避できるのでそうしよう」などもあります。

ここで重要となってくるのが、そもそも弱点を持つライブラリをどうやって知るかです。一般的には、次のとおりです。

- 中央組織とウォッチャー
    - a: 脆弱性を管理する世界的な組織やデータベースがある
    - b: 各企業や脆弱性管理製品は a: をウォッチしている
- 中央組織に情報が集まる
    - c: 日々脆弱性が見つかると a: に報告が行く。真面目な人が報告したり、研究者が見つけたり、すでに被害が出ているものを調査して発覚したり、と色々あります
- 専任者によるチェックと周知
    - d: a: の人達は、新しい情報が来たらそれをデータベースに反映します
    - e: b: の人達や製品は新しい脆弱性があるとわかると、それを社内や利用者に通知します

つまり「脆弱性を集める組織やデータベース」があって、そこに情報が集まります。そこを日々ウォッチする企業や製品がいて、更新があればそれを私たち一般人に下ろしてきます。ここはもう少し細かくて、以下のとおり 3 つくらいのレイヤーがあります。

- 1: 企業の脆弱性専門部隊（PSIRT/ピーサートと呼ばれる事が多い）
- 2: 企業のエンジニアやその他ソフトウェアに関する仕事を担う人達
- 3: ソフトウェアの利用者

1 の人達が脆弱性をウォッチしていて、何かあったら社内に通達します。それで実際ソフトウェアの仕事に絡んでいる 2 の人達が対応に追われて、開発者は修正をしたり、広報者は社外への言い方を考えたりするわけです。その結果が 3、利用者やその他一般市民に降りてきます。もっとも、これは企業がある程度大きい場合の話で、小さな組織だとおそらく 1 はなくて 2 の人達が頑張ることになるでしょう。

まとめ:

- 脆弱性の対処とは、自身のソフトウェアが「脆弱性を持つライブラリ」を使っているかどうかを見る、に等しい
    - 見つかったらあとは何らかの対処をすればいい、まずは見つけることが肝心
- 現実的には中央的なデータベース、それをウォッチする人達などいくつかの役割が存在する

# SBOMと脆弱性
ようやくSBOMと脆弱性の話ができます。

脆弱性を探すには、自分のソフトウェアが「弱点を持つライブラリ」を使ってるかどうかを見ればいい、と書きましたが、ではそれをどうやって行うのでしょうか。毎回開発者が頑張って手作業で対応するのでしょうか。いいえ、大変ですよね。彼らだって忙しいですし、人間なのでヌケモレも出ます。

そこで SBOM の出番です。「うちのソフトウェアはこれこれのライブラリを使っています」というパーツ一覧をつくっておくのです。そうしたら **一覧を見るだけで（弱点を持つライブラリの）有無がわかります**。何なら一覧をプログラムで読める形式にしておけば、自動でチェックできます。

これがSBOMです。

# そんな簡単じゃない
もちろん、そんな簡単に行くものでもありません。

たとえば一覧を常に正しくつくれるのかとか、そんな面倒くさいことを律儀にやるのかとか、自動で有無を調べるためには具体的にどんな仕組みや解法を使えばいいのかとか、そもそもSBOMにはどんな属性を書けばいいのか（先ほど名前、バージョン、URL、ライセンスといったシンプルな例は示しました）など。あるいは「脆弱性が 2301 件ありました」と言われたらどうするのか、全部対応するのか、そんな時間やお金はあるのか、といったこともあります。

課題は山積みです。それでも、SBOMという発想に頼ることで脆弱性への対処は楽になります。

# まとめ
- SBOMとは一覧です
    - あるソフトウェアがどんなライブラリを使っているかを一覧化したものです
    - ただしプログラムで自動チェックできるような形式になっています（人間が読むものとしてはツライ）
- 脆弱性とは「あるソフトウェア」に「攻撃されうる弱点」が見つかったというもの
    - 電子的な世界は悪さがしやすいので、放置していてはいけません
- 脆弱性の対処とは、脆弱性を持つソフトウェアの有無を知ることです
    - 有ったとわかれば、あとは対処をすればいい
    - そのためにSBOMが使えます。脆弱性を持つソフトウェアがSBOMの中にあるかどうかを調べればいいだけだからです
- これらは理想論であり現実では課題が山積みですが、方向性は合っているはずです

# Chapter-3 SBOMとは

SBOM の概要と状況。

# SBOM とは
**SBOM(Software Bill of Materials), ソフトウェア部品票** とは、あるソフトウェアがどんなソフトウェアを使っているかを一覧化したものです。一覧の各要素は **コンポーネント（Component）** や **パッケージ（Package）** や **ライブラリ（Library）** と呼ばれることもあります。

# SBOMの目的

## 元々の目的は脆弱性
SBOM の本来の目的は脆弱性管理です。

あらかじめコンポーネントを一覧化しておけば、あるソフトウェア X に脆弱性が出たときに、一覧から X の有無を調べるだけで済みます。もっとも、一覧という意味では、すでに構成情報管理などのジャンルで古典的に管理されていましたが、している組織としていない組織にばらつきがありますし、やり方の人手による手作業ゆえに時間もかかれば調査の質にもばらつきがあります。このような状況を打開するために、プログラムで処理できる形式で一覧化して、X の有無もプログラムで自動的に調べてしまえばいいと考えました。

つまり脆弱性の対応を完了させるまでの時間――リードタイムを短縮したいのです。ゼロにはできませんが、みんなが同じ形式を使い、プログラムで自動化するようにすれば、だいぶ短縮できます。

## 色んな目的が合流
2024年現在では複数の目的が合流しています。

大別すると以下の 5 つになります。

- 透明性の確保
    - 食品の栄養成分表示と同様、構成要素を見える化したいという話です
    - ブラックボックスを解消すれば隠し立てできなくなり、健全性が高まります
- ライセンス管理
    - ソフトウェア開発≒オープンソースソフトウェアの使用、であることは[前章「前提知識」](prerequisite)で述べました
    - そしてオープンソースソフトウェアにはライセンスがついています
    - ライセンスを守らないと訴訟や評判落ちのリスクがあるため、守られているかの確認は必要です
    - SBOMにライセンス情報も含めてやれば、これらのリスク調査も行えるようになります
- サポート情報の管理
    - ソフトウェアも製品であり継続的なサポートがつきますが、無限にサポートするのは酷なので EOL（期限）があります
    - また製造元やサポート連絡先といった情報もあると便利です
    - このような情報も SBOM に持たせておくと活用がしやすいです
- 情報集約、分析、ナレッジ
    - SBOM は部品票なので、たとえば社内の全プロジェクトのSBOMを収集すれば膨大な情報になります
    - これを分析することでインサイトを発見したり、ナレッジとして活用したりできる余地があります
        - 例: 推奨コンポーネントや類似プロジェクトの提案、組織の傾向を可視化、よく使われているコンポーネントのランキング
- オーバーオールセキュリティ
    - 開発や運用を含む工程全体の目線でセキュリティを確保すべし、との潮流が盛り上がっています
        - 造語ですが **オーバーオールセキュリティ(Overall Security)** と呼ばせてください
    - ソフトウェアサプライチェーン、DevSecOps などこの手の概念は多数存在します
        - 昔から存在するのもありますし、最近提唱され始めたものもあります
    - SBOM は構成要素の一覧化という「別に目新しい発想ではないが面倒くさかった営み」を業界レベルで加速させるものであり、オーバーオールセキュリティの背中を押しています

# SBOM対応が騒がれる理由

## 理由1. 間に合わないから
今のうちに準備しておかないと間に合わないからです。

サイバーセキュリティの担保は世界的な関心事であり、そもそも事の発端は米国における大統領令への署名です。IT 先進国の影響は無視できませんし、現に経産省も実証実験や手引書の公開などを始めています。国内でも自動車や医療など一部業界では事例も増えています。今後、法や要件として要求される可能性は高いのです。

一方で、SBOM の生成や管理や提供は、片手間ですぐにできるものではありません。要求されたときに準備を始めても遅い――間に合わなければ案件の失注や信頼の低下など機会損失に繋がりかねません。

肌感覚として、エンジニアの人は **開発フェーズが一つ増えるほどのインパクトが想定される** と考えれば良いでしょう。 「ドキュメント作成」は（後回しになることも多いですが）フェーズの一つとして知られています。これの仲間として「証明の作成」フェーズが加わるイメージです。証明として SBOM があります（構成要素の証明）が、SBOM だけではなさそうです。オーバーオールセキュリティが盛り上がっており、たとえば「SBOM の正しさを保証するための署名もつくれ」などがあるかもしれません。実際に[in-toto](ref#in-toto) や [SBOMit](ref#sbomit)などこの手のツールも出始めています。

## 理由2. ビジネスチャンスだから
要は SBOM には法や要件といった「皆が従うべきルール」を新たにつくるポテンシャルがあります。ルールをつくる立場に絡んだり、ルールを守るために必要なツールを売ったりなど、ビジネスとして絡める余地は大いにありますよね。

実際、海外ではいくつもの団体が規格や製品を立ち上げて乱立状態にあります。たとえば SBOM の規格として SPDX、CycloneDX、SWID と 3 つ挙がっていますが、現状ツールがサポートしているのは SPDX と CycloneDX の 2 つです（SWID は無視されています）。一方で、Microsoft が持つ SBOM Tool は SPDX しかサポートしていませんし、国産の mjcheck4 は SWID で出力します――と、ここだけ見てもバラバラですね。

国内では大手ベンダーが公式サイトや[ブログ](ref#大手sierのブログ記事)などで情報を散りばめている他、SBOM、特に脆弱性管理を謳った製品も出す企業も出てきています（[yamory](ref#yamory)や[Snyk](ref#snyk)など）。また[富士キメラ総研による調査](ref#16)ではSBOM・脆弱性の市場規模は 2028 年で 100 億円にのぼると見られています。一方で、[タニウムの調査](ref#17) によると SBOM の認知度は「大企業のIT関係者」に限定しても 75% です。

# SBOMの歴史
詳細は深追いしませんが、2021年、米国にてバイデン大統領がサイバーセキュリティに関する大統領令に署名したことが始まりです。

- 参考: [Executive Order on Improving the Nation's Cybersecurity | The White House](https://www.whitehouse.gov/briefing-room/presidential-actions/2021/05/12/executive-order-on-improving-the-nations-cybersecurity/)

# SBOM の中身

## SBOMは概念、規格は2つ
まず SBOM 自体はソフトウェア部品票という概念にすぎません。実際にどんな形式でどんな情報を書くかという形式は「規格」として定義されねばなりません。

規格として現在スタンダードになっているのが [SPDX](ref#12) と [CycloneDX](ref#13) です。もう一つ、SWID という名前が挙がることがありますが、なぜ挙がっているのかわからないほど場違いな存在であり、実情としてもスルーされているので気にする必要はないと思います。

つまり SBOM をつくるとは、現状では SPDX か CycloneDX の形式に従ってつくることを意味します。

## SBOM の中身
SPDX にせよ CycloneDX にせよプログラムで読むためのデータフォーマットとなっており、人間が読むのは少々苦しいです。

かんたんに構造を説明すると、以下のようになっています。

```
(SBOM)
  -(メタ情報)
    -タイトル
    -作成日
    -...
  -(コンポーネント一覧)
    -(コンポーネント1)
      -(名前)
      -(バージョン)
      -(このソフトウェアの開発元)
      -(このソフトウェアの入手先URL)
      -...
    -(コンポーネント2)
      -...
    -...
  -(その他あると便利な情報をカテゴライズしてぶら下げる)
    -(細かい構造はカテゴリ次第で様々)
    -    
  -(その他あると便利な情報をカテゴライズしてぶら下げる)
    -...
  -...
```

煩雑な構造になりがちですが、コンポーネント情報を全部律儀に記載しているものだと思えば間違いはありません。

以下にいくつか例を挙げます（全部載せると長いので途中で端折ります）。

### SPDX、TagValue形式

```txt
SPDXVersion: SPDX-2.2
DataLicense: CC0-1.0
SPDXID: SPDXRef-DOCUMENT
DocumentName: hello
DocumentNamespace: https://swinslow.net/spdx-examples/example1/hello-v3
Creator: Person: Steve Winslow (steve@swinslow.net)
Creator: Tool: github.com/spdx/tools-golang/builder
Creator: Tool: github.com/spdx/tools-golang/idsearcher
Created: 2021-08-26T01:46:00Z

##### Package: hello

PackageName: hello
SPDXID: SPDXRef-Package-hello
PackageDownloadLocation: git+https://github.com/swinslow/spdx-examples.git#example1/content
FilesAnalyzed: true
PackageVerificationCode: 9d20237bb72087e87069f96afb41c6ca2fa2a342
PackageLicenseConcluded: GPL-3.0-or-later
PackageLicenseInfoFromFiles: GPL-3.0-or-later
PackageLicenseDeclared: GPL-3.0-or-later
PackageCopyrightText: NOASSERTION

Relationship: SPDXRef-DOCUMENT DESCRIBES SPDXRef-Package-hello

FileName: ./build/hello
SPDXID: SPDXRef-hello-binary
FileType: BINARY
FileChecksum: SHA1: 20291a81ef065ff891b537b64d4fdccaf6f5ac02
FileChecksum: SHA256: 83a33ff09648bb5fc5272baca88cf2b59fd81ac4cc6817b86998136af368708e
FileChecksum: MD5: 08a12c966d776864cc1eb41fd03c3c3d
LicenseConcluded: GPL-3.0-or-later
LicenseInfoInFile: NOASSERTION
FileCopyrightText: NOASSERTION
...
```

from: https://github.com/spdx/spdx-examples/blob/master/software/example1/spdx2.2/example1.spdx

### SPDX、JSON形式

```json
{
  "SPDXID": "SPDXRef-DOCUMENT",
  "name": "SBOM-SPDX-2d85f548-12fa-46d5-87ce-5e78e5e111f4",
  "spdxVersion": "SPDX-2.3",
  "creationInfo": {
    "created": "2022-11-03T07:10:10Z",
    "creators": [
      "Tool: sigs.k8s.io/bom/pkg/spdx"
    ]
  },
  "dataLicense": "CC0-1.0",
  "documentNamespace": "https://spdx.org/spdxdocs/k8s-releng-bom-7c6a33ab-bd76-4b06-b291-a850e0815b07",
  "documentDescribes": [
    "SPDXRef-Package-hello-server-src",
    "SPDXRef-File-hello-server"
  ],
  "packages": [
    {
      "SPDXID": "SPDXRef-Package-hello-server-src",
      "name": "hello-server-src",
      "versionInfo": "0.1.0",
      "filesAnalyzed": false,
      "licenseDeclared": "Apache-2.0",
      "licenseConcluded": "Apache-2.0",
      "downloadLocation": "NONE",
      "copyrightText": "NOASSERTION",
      "checksums": [],
      "externalRefs": [
        {
          "referenceCategory": "PACKAGE-MANAGER",
          "referenceLocator": "pkg:deb/debian/libselinux1-dev@3.1-3?arch=s390x",
          "referenceType": "purl"
        }
      ]
    },
    {
      "SPDXID": "SPDXRef-Package-SPDXRef-Package-cargo-hyper-0.14",
      "name": "hyper",
      "versionInfo": "0.14",
      "filesAnalyzed": false,
      "licenseDeclared": "NOASSERTION",
      "licenseConcluded": "MIT",
      "downloadLocation": "https://github.com/rust-lang/crates.io-index",
      "copyrightText": "NOASSERTION",
      "checksums": [{
        "algorithm" : "SHA256",
        "checksumValue" : "02c929dc5c39e335a03c405292728118860721b10190d98c2a0f0efd5baafbac"
      } ],
      "externalRefs": [
        {
          "referenceCategory": "PACKAGE-MANAGER",
          "referenceLocator": "pkg:cargo/hyper@0.14",
          "referenceType": "purl"
        }
      ]
    },
    ...
```

from: https://github.com/spdx/spdx-examples/blob/master/software/example11/spdx2.3/sbom.spdx.json

### CycloneDX、JSON 形式

```json
{
  "bomFormat": "CycloneDX",
  "specVersion": "1.2",
  "serialNumber": "urn:uuid:d7a0ac67-e0f8-4342-86c6-801a02437636",
  "version": 1,
  "metadata": {
    "timestamp": "2021-05-16T17:10:53+02:00",
    "tools": [
      {
        "vendor": "CycloneDX",
        "name": "cyclonedx-gomod",
        "version": "v0.6.1",
        "hashes": [
          {
            "alg": "MD5",
            "content": "a92d9f6145a94c2c7ad8489d84301eb9"
          },
          {
            "alg": "SHA-1",
            "content": "a5af6c5ef3f21bf5425c680b64acf57cc6a90c69"
          },
          {
            "alg": "SHA-256",
            "content": "dc215a651772356eca763d6fe77169379c1cc25c2bb89c7d6df2e2170c3972ab"
          },
          {
            "alg": "SHA-512",
            "content": "387953ab509c31bf352693de9df617650c87494e607119bc284b91ba9a0a2d284a2e96946c272dc284c4370875412eea855bc30351faedd099dbdbed209e4636"
          }
        ]
      }
    ],
    "component": {
      "bom-ref": "pkg:golang/github.com/ProtonMail/proton-bridge@v1.8.0",
      "type": "application",
      "name": "github.com/ProtonMail/proton-bridge",
      "version": "v1.8.0",
      "purl": "pkg:golang/github.com/ProtonMail/proton-bridge@v1.8.0",
      "externalReferences": [
        {
          "url": "https://github.com/ProtonMail/proton-bridge",
          "type": "vcs"
        }
      ]
    }
  },
  "components": [
    {
      "bom-ref": "pkg:golang/github.com/0xAX/notificator@v0.0.0-20191016112426-3962a5ea8da1",
      "type": "library",
      "name": "github.com/0xAX/notificator",
      "version": "v0.0.0-20191016112426-3962a5ea8da1",
      "scope": "required",
      "hashes": [
        {
          "alg": "SHA-256",
          "content": "8fd1da69f6a90db3db1910e4bba7bf1d1b3a28131c287896726d7ff526f19e5e"
        }
      ],
      "licenses": [
        {
          "license": {
            "id": "BSD-3-Clause",
            "url": "https://spdx.org/licenses/BSD-3-Clause.html"
          }
        }
      ],
      "purl": "pkg:golang/github.com/0xAX/notificator@v0.0.0-20191016112426-3962a5ea8da1",
      "externalReferences": [
        {
          "url": "https://github.com/0xAX/notificator",
          "type": "vcs"
        }
      ]
    },
    ...
```

from: https://github.com/CycloneDX/bom-examples/blob/master/SBOM/proton-bridge/proton-bridge-v1.8.0.bom.json

# SBOM はどうやってつくるか

## 基本的にはツールでつくる
上記のフォーマットを見てもらえればわかるとおり、人間が手作業でつくるのは現実的ではありません。そもそも SBOM は元からプログラムによる自動生成を想定しています。なので基本的にはツールを使ってつくります。

ツールを起動して、SBOM を生成したい対象（ソースコードをまとめたフォルダなど）を指定すると、その対象をツールが解析して SBOM をつくります。

## 国内では手作業文化も強そう
一方で、国内では票に記入するイメージで手作業でつくる想定も強いと思われます。実際、SPDX には SPDX Lite という「Excel などで手作業で書ける用の SPDX」が組み込まれていますし、[トヨタ](ref#3)ではすでに採用されているようです。

## 自動 vs 手作業
「ツールによる生成だけで済むのか？」は SBOM を知る人なら誰もが通る疑問でしょう。

### まず技術的には「いいえ」
まず技術的には「厳密にすべてのコンポーネントを洗い出せるほどではない」です。SBOM 生成ツールは「パッケージマネージャ」という使用ライブラリを管理する仕組みの設定ファイルを解析しているので、そこで扱われていない情報はそもそも拾えません。

パッケージマネージャ自体が比較的近年の発明であり、10 年以上前など古いソフトウェアや昔のプログラミング言語ではそもそも使われていないことも多く、「生成ツールを通しても全く生成されませんでした」もよく起きます。昔のセキュリティ製品はパッケージマネージャではなくソースコードなど各種ファイルを解析する機能を持っており、これらが SBOM の出力にも対応し出したというケースもあります。そういった機能は精度とメンテナンスに限界があります。たとえば Black Duck は、昔の解析機能と近年のパッケージマネージャベースの両方を持っていますが、前者は非推奨としています。

> Note: Black Duckを長く使用しているユーザーは、Signature Scannerを直接起動することに慣れているかもしれません。現在この方法は推奨されていません。宣言された依存関係を備えたパッケージマネージャファイルを検索して活用するためのDetectorの能力が失われるためです。 / [スキャンのベストプラクティス](https://sig-product-docs.synopsys.com/ja-JP/bundle/bd-hub/page/ComponentDiscovery/BestPracticesScanning/ToolTipsNew.html)より

つまり、

- ソースコードなどを直接解析する手法では限界がある
- 近年ではパッケージマネージャの設定ファイルを解析する
    - しかし、パッケージマネージャをどれだけ正しく使っているかは現場のエンジニア次第
    - 正しく使われていないと、設定ファイルにもその分の記載が漏れるから、そもそも検出できない

### クオリティをどこまで求めるか
あとはクオリティをどこまで求めるか次第だと思います。

ツールがつくった SBOM をそのまま信じるのであれば、答えは「イエス」でしょう。あるいはそもそもツールでつくれるよう現場のやり方やあり方を変えていく動きをすることになります。DX――デジタルトランスフォーメーションでも散々言われていますが、デジタルの流儀に私達が合わせにいくわけですね。

一方で、それじゃ信用ならんから人間でもチェックするぞ修正するぞとか、そもそもツールで出力できる状態になってないけどそうするつもりはないので手作業でつくるぞ、手作業ならクオリティも突き詰められるし IT に無知でも運用できるしな、とかいった場合は答えは「ノー」でしょう。

筆者の肌感覚では、国内の SI が絡む領域では後者ノー派が強いと思います。経産省の手引などでも手作業の必要性は想定されていますし、前述したように SPDX Lite なるフォーマットが事実として存在します。一方で、テクノロジー企業はデジタルにも造詣が深く、前者のやり方を採用している印象です。[yamory](ref#yamory)や[Snyk](ref#snyk)は前者寄りの製品です。

# SBOM のようで SBOM ではないもの

## SBOM と xBOM
SBOM とはあくまで「あるソフトウェア」が「どんなコンポーネント（ソフトウェア）」から構成されているか、の一覧です。一方で IT システムを取り巻く環境はそれだけではありません。

たとえば以下があります。

- 1: あるサーバーには、どんなソフトウェアがインストールされているか
- 2: あるクラウドサービスは、どんなサブサービスを使っているか
- 3: ある装置は、どんな部品から構成されているか
- 4: ある機械学習モデルは、どんなモデルやデータセットから構成されているか etc

これらは SBOM（**Software** BOM）で扱うことはできません。あえて名付けるなら Server BOM、Service BOM、Devide BOM、Machine-Learning BOM でしょうか。当然ながらこれらにはこれら用の規格が必要です。

2、3、4 はすでに存在しており、それぞれ SaaSBOM、HBOM（Hardware BOM）、ML-BOM と呼ばれます。

## SPDX と CycloneDX は SBOM 用？
答えを書くと、元々は「イエス」だったが最近は「ノー」となった、です。

まず CycloneDX は元々汎用的なデータ構造となっており、SBOM だろうが HBOM だろうが色んな BOM を柔軟に表現できるポテンシャルを持っていました。SaasBOM や ML-BOM といった言葉は、CycloneDX の公式サイトが使っているものです。

SPDX については、元々 SBOM のみを想定したデータ構造でしたが、それでは他の BOM に対応できないよね、ということで Version3 から CycloneDX と同様、汎用的なデータ構造になりました。やはり「色んな BOM を柔軟に表現できますよ」という路線です。

## SBOM は xBOM の一種にすぎない
SBOM という名前が盛り上がっているのは、2021 年の大統領令が発端だと思いますが、実情としては Software 以外にも様々な BOM が存在します。それに対応するべく、SBOM 規格の双頭はどちらも汎用的な構造となっています。

SBOM は BOM の一種にすぎません。この関係はぜひ頭に入れておいてほしいと思います。でないと、Software ではない BOM の話なのに SBOM を適用しようとする、みたいなおかしなことになりかねません。

## とはいえ SBOM がメインではある
とはいうものの、大統領令から始まる一連の流れはソフトウェアに注目しており、xBOM の中でも SBOM が最も重要かつ盛り上がっているのは間違いないと思います。そもそも脆弱性も、ソフトウェアの分野が盛り上がっているからこそ日々発生するものです。盛り上がっているからこそ悪事がビジネスになり、悪意も集まります。

一方で、xBOM もおざなりにはできません。たとえばハードウェアの分野にも部品の脆弱性というものはあります。部品にソフトウェアが内蔵されているパターンもあります（組み込みソフトウェアなど、インターネットで主に想定されるソフトウェアとは別ジャンルになりがちですが）。特に自動車や医療など人命がかかわる部分は、無視できるものではありませんし、国内でも他の業界より歩みが早いと感じます。しかし、SBOM のやり方がそのまま通じるとは限りません。それこそ、自動化できるだけのツールが業界レベルで整備されていないから手作業による作成が必然になってしまう、等もありえます。

ビジネスチャンスと書きましたが、まさにそうでもあります。世の中をリードするソフトウェア業界で取り沙汰されたSBOMの概念を持ち込み、ルールや規制やツールや事例などの形で上手く絡むことができれば儲かりますよね。企業としては一種のチャンスであり、開発者や現場作業者としては鬱陶しいノイズでもあります。

SBOMがメインという事実はありますが、無闇に踊らされず、一歩引いて冷静に向き合いたいものです。

# Chapter-4 📘第2部「SBOM の中身や扱い方を理解する」

SBOM を扱うことになるエンジニアの方向けに、技術的な情報や周辺知識の解説を端的に行っていきます。

# Chapter-5 SBOMの形式

形式（フォーマット）の話。

# フォーマット

## フォーマットには構造とファイルがある
SBOM のフォーマットには構造とファイルがあります。

構造とは SBOM をどのような概念を用いてどのように記述するかを定義したものです。現在では SPDX と CycloneDX の二つの規格が主流となっています。

ファイルとは txt や json など、どんなファイルフォーマットを使うかという話です。主に txt、json、XML、YAML などがあります。SBOM はただのリスト、ただの票ですので、プログラミング言語構文レベルの表現力は必要ありません。

つまり SBOM のフォーマットは構造とファイルの二点を指定することになります。以下に例を示します。

- SPDX, json
- SPDX, txt
- CycloneDX, yaml

## バージョン
SPDX にせよ、CycloneDX にせよ、規格ですのでバージョンがあります。

SPDX には 1.0 系、2.x 系と 3.x 系があります。CycloneDX には 1.x があります。

前項も踏まえると、SBOM のフォーマットは以下のような表現になります。

- SPDX 2.2, json
- SPDX 2.3, txt
- CycloneDX 1.6, yaml

# SPDX

## 概要
SPDX は Linux Foundation がリードする SBOM 規格です。

- https://spdx.dev/

正式名称は System Package Data Exchange です。元々は Software Package Data Exchange でしたが、バージョン 3.0 ではソフトウェアに限らず汎用的に扱う方向に進んでシステムとなりました。

## バージョンについて
1.0 系は古いので無視して構いません。

現在は 2.x 系が主流で、最新は 2.3 ですが、巷のツールは 2.2 以前を出力することもあります。

3.0 系は 2024 年に公開された最新版で、2.x との後方互換性はなく全面的に刷新されています。2.x との違いですが、元々 2.x 以前ではソフトウェアのライセンス管理に特化していました。しかし前章でも述べたとおり、SBOM 以外にも様々な BOM が必要であるため、汎用的な構造をつくれるように拡張性を内蔵する方向に刷新されました。

## 構造
バージョン 2.3 を取り上げます。

リファレンスは以下にあります。

- https://spdx.dev/use/specifications/
    - https://spdx.github.io/spdx-spec/v2.3/

全体構造は第5章(Clause)にあります。

- https://spdx.github.io/spdx-spec/v2.3/composition-of-an-SPDX-document/

画像も掲載されていますが、ここでも書くと、以下のように第一レベルにいくつかのカテゴリがあります。

```
(SPDX Document)/
  (Creation Info)/
  (Package Info)/
  (File Info)/
  (Snippet Info)/
  (Other Licensing Info Detect)/
  (Relationships between SPDX Elements)/
  (Annotation Info)/
  (Review Info)/
```

SBOM として最低限必要な部分については二つで、ドキュメントのメタ情報の Creation Info、コンポーネント情報の Package Info です。

それ以外にも色んな情報を載せられるようにはなっていて、検出したファイル自体の情報（File）、[組み込みのライセンス名一覧](https://spdx.github.io/spdx-spec/v2.3/SPDX-license-list/)に当てはまらないライセンスの情報（Other Licensing）、包含や依存や複製などコンポーネントとコンポーネントの関係（Relationships）などがあります。詳細はドキュメントの各章を見てください。

## 出力例
[HTMX](https://github.com/bigskysoftware/htmx) を対象に、[SBOM Tool ver2.2.5](https://github.com/microsoft/sbom-tool/releases/tag/v2.2.5) で SPDX 2.2, json を出力した例です。

### 出力コマンド

```
$ cd
d:\work\sbom_kamikudaku\htmx

$ mkdir sbom

$ sbomtool Generate -b sbom/ -bc ./ -pn "testpackage" -pv "ver1.2.3" -ps "test_namespace"
...
│ Total                │ 0.16 seconds         │ 217                  │ 11                  │
└──────────────────────┴──────────────────────┴──────────────────────┴─────────────────────┘
```

### 確認

```
$ code sbom\_manifest\spdx_2.2\manifest.spdx.json
```

出力はこちらにもアップロードしています: https://github.com/stakiran/sbom_kamikudaku/blob/master/spdx2.2_htmx.json

jq コマンドを用いて少し詳しく見てみましょう。

まず直下ですが、メタ情報の入った creationInfo、コンポーネント情報の入った packages などがありますね。

```
$ cat manifest.spdx.json | jq "keys"
[
  "SPDXID",
  "creationInfo",
  "dataLicense",
  "documentDescribes",
  "documentNamespace",
  "externalDocumentRefs",
  "files",
  "name",
  "packages",
  "relationships",
  "spdxVersion"
]
```

メタ情報を見てみましょう。作成日と作成者情報が埋められていますね。

```
$ cat manifest.spdx.json | jq ".creationInfo"
{
  "created": "2024-05-23T09:19:15Z",
  "creators": [
    "Organization: test_namespace",
    "Tool: Microsoft.SBOMTool-2.2.5"
  ]
}
```

次にコンポーネント情報を見てみます。名前（name）とバージョン（versionInfo）、あとは識別子として Package URL（externalRefsの0番要素）くらいはわかりますね。その他もライセンスやダウンロード場所などいくつか属性がありますが、情報なし（NOASSERSION）とあります。

```json
$ cat manifest.spdx.json | jq ".packages[0]"
{
  "name": "readable-stream",
  "SPDXID": "SPDXRef-Package-DC2DA7F8AAF47E0872212F8A1CC0CB6608DFAC69E8A513ACA466117BC41BC454",
  "downloadLocation": "NOASSERTION",
  "filesAnalyzed": false,
  "licenseConcluded": "NOASSERTION",
  "licenseDeclared": "NOASSERTION",
  "copyrightText": "NOASSERTION",
  "versionInfo": "2.3.8",
  "externalRefs": [
    {
      "referenceCategory": "PACKAGE-MANAGER",
      "referenceType": "purl",
      "referenceLocator": "pkg:npm/readable-stream@2.3.8"
    }
  ],
  "supplier": "NOASSERTION"
}
```

コンポーネント数を見てみましょう。218 個と出ました。これを信じるなら、HTMX は 218 個のコンポーネントを使っています。

```
$ cat manifest.spdx.json | jq -r ".packages[] | .name" | wc -l
218
```

# CycloneDX

## 概要
CycloneDX は OWASP がリードする SBOM 規格です。

- https://cyclonedx.org/

## バージョンについて
1.x 系が存在します。1.0 から 1.6 まであります。

当初から汎用的なデータ構造を謳っており、SPDX のような大きな刷新はありません。

## 構造
バージョン 1.5 を取り上げます。

リファレンスは以下にあります。json や Protocol Buffer のスキーマとして公開されています。

- https://cyclonedx.org/specification/overview/
    - https://github.com/CycloneDX/specification/tree/1.6/schema
        - https://github.com/CycloneDX/specification/blob/1.6/schema/bom-1.5.schema.json
        - https://github.com/CycloneDX/specification/blob/1.6/schema/bom-1.5.proto

bom-1.5.proto を少し読み解いてみましょう。

構成票を示す構造は messsage BOM として定義されています。一部だけ抜粋します。

```protobuf
message Bom {
  string spec_version = 1;
  optional int32 version = 2;
  optional string serial_number = 3;
  optional Metadata metadata = 4;
  repeated Component components = 5;
  ...
}
```

コンポーネント情報は Component として持ってそうですね。repeated なので配列です。

次にコンポーネントを示す構造ですが、message Component として定義されています。

```protobuf
message Component {
  // Specifies the type of component. For software components, classify as application if no more specific appropriate classification is available or cannot be determined for the component.
  Classification type = 1;
  ...

  // The person(s) or organization(s) that authored the component
  optional string author = 5;
  // The person(s) or organization(s) that published the component
  optional string publisher = 6;
  ...

  // The name of the component. This will often be a shortened, single name of the component. Examples: commons-lang3 and jquery
  string name = 8;
  // The component version. The version should ideally comply with semantic versioning but is not enforced. Version was made optional in v1.4 of the spec. For backward compatibility, it is RECOMMENDED to use an empty string to represent components without version information.
  string version = 9;
  ...

  // DEPRECATED - DO NOT USE. This will be removed in a future version. Specifies a well-formed CPE name. See https://nvd.nist.gov/products/cpe
  optional string cpe = 15;
  // Specifies the package-url (PURL). The purl, if specified, must be valid and conform to the specification defined at: https://github.com/package-url/purl-spec
  optional string purl = 16;
  ...
}
```

type はコンポーネントの種類を指しています。（ここでは抜粋しませんが） enum Classification として定義されており、アプリ、フレームワーク、ライブラリ、OS、デバイス、コンテナ、ファームウェアなど色々定義されています。

author や publisher はコンポーネントの開発元や供給元を書くフィールドです。

コンポーネント名は name、バージョンは version として定義されていますね。コメントに補足があり、バージョンは Semantic Versioning で書くべきだが強制ではないとありますね。

識別子としては cpe と purl がありますが、cpe の方は DEPACATED になっています。

――と、このような形で、スキーマの定義を読み解くことで追っていけます。

## 出力例
[HTMX](https://github.com/bigskysoftware/htmx) を対象に、[syft ver1.4.1](https://github.com/anchore/syft/releases/tag/v1.4.1) で CycloneDx 1.4, json を出力した例です。

### 出力コマンド

```
$ cd
d:\work\sbom_kamikudaku\htmx

$ syft ./ -o cyclonedx-json > cyclonedx14_htmx.json
 ✔ Indexed file system                                                                   .
 ✔ Cataloged contents              cdb4ee2aea69cc6a83331bbe96dc2caa9a299d21329efb0336fc02a

   ├── ✔ Packages                        [219 packages]
   └── ✔ Executables                     [0 executables]
...
```

### 確認
そのままだと未整形で読みづらいので、jq で Pretty Print したものを開きます。

```
$ jq . cyclonedx14_htmx.json > cyclonedx14_htmx_pretty.json
$ code cyclonedx14_htmx_pretty.json
```

出力はこちらにもアップロードしています: https://github.com/stakiran/sbom_kamikudaku/blob/master/cyclonedx14_htmx_pretty.json

同様に jq で少し見てみましょう。

まず直下ですが、ほぼコンポーネント情報（components）のみの出力に割り切っているようです。

```
$ cat cyclonedx14_htmx_pretty.json | jq "keys"
[
  "$schema",
  "bomFormat",
  "components",
  "metadata",
  "serialNumber",
  "specVersion",
  "version"
]
```

メタ情報を見てみましょう。作成日や使用ツール情報などが埋められています。

```
$ cat cyclonedx14_htmx_pretty.json | jq ".metadata"
{
  "timestamp": "2024-05-23T19:03:03+09:00",
  "tools": {
    "components": [
      {
        "type": "application",
        "author": "anchore",
        "name": "syft",
        "version": "1.4.1"
      }
    ]
  },
  "component": {
    "bom-ref": "08329a07b4eb8eac",
    "type": "file",
    "name": "./"
  }
}
```

コンポーネント情報を見てみます。

```json
$ cat cyclonedx14_htmx_pretty.json | jq ".components[0]"
{
  "bom-ref": "pkg:npm/%40sinonjs/commons@1.8.2?package-id=51a9235e6851e30d",
  "type": "library",
  "name": "@sinonjs/commons",
  "version": "1.8.2",
  "cpe": "cpe:2.3:a:\\@sinonjs\\/commons:\\@sinonjs\\/commons:1.8.2:*:*:*:*:*:*:*",
  "purl": "pkg:npm/%40sinonjs/commons@1.8.2",
  "properties": [
    {
      "name": "syft:package:foundBy",
      "value": "javascript-lock-cataloger"
    },
    {
      "name": "syft:package:language",
      "value": "javascript"
    },
    {
      "name": "syft:package:type",
      "value": "npm"
    },
    {
      "name": "syft:package:metadataType",
      "value": "javascript-npm-package-lock-entry"
    },
    {
      "name": "syft:location:0:path",
      "value": "D:\\work\\sbom_kamikudaku\\htmx\\package-lock.json"
    }
  ]
}

```

名前（name）とバージョン（version）、識別子としては Package URL（purl）と CPE（cpe）がわかります。cpe は DEPRECATED でしたが、syft はまだ出力するようですね。

その他には種類がライブラリであることや、npm パッケージである等の情報も入っています。

最後にこのフォーマットのバージョンを見てみましょう。

```
$ cat cyclonedx14_htmx_pretty.json | jq -r ".specVersion"
1.5
```

CycloneDX 1.5 のようです。筆者がアップロードしたファイル名には 1.4 とつけていますが、これは README のフォーマット解説欄で `cyclonedx-json: A JSON report conforming to the CycloneDX 1.4 specification.` と書いてあったからです。README 側の修正漏れでしょうか。

# フォーマットと現実
上述した構造フォーマットは規格であるため厳密に踏襲するべきですが、実情としてはそうなっていません。すでに NOASSERSION（情報なし）が入っていたり、DEPRECATED が反映されていなかったりなどは取り上げました。

もう一つ、2024/05 現在で SPDX の最新は 3.0、CycloneDX の最新版は 1.6 ですが、これらの形式で出力できるツールはまだありません。ちなみに本章で示した SBOM Tool と Syft は、オープンソースの中では最も高品質と思われるツールです。有償製品でしたら、すでに対応したものがあるかもしれません。

このようにフォーマットとツールとの間には微妙なギャップが存在することに注意してください。

# SBOM が満たすべき最小要素
現在のところ、SBOM のフォーマットに法的な制約はありません。2024年5月現時点ではまだ[NTIAの最小要素](ref#22)の定義になるべく従いましょうねという空気感です。ちなみに NTIA とは米商務省国家電気通信情報局です。

この文書では「これらは現時点では最小」「組織や政府はもっと多くの情報を求めるかもしれない」「ソフトウェアサプライチェーンの改善に伴う進化もありうる」と書いてあり、（最小といいつつ結構な情報量なのですが）スタートラインくらいのニュアンスなのだと感じます。

定義の部分は和訳を使わせてください。[経産省の手引](ref#1)から引用します。

![minimum_element](/images/sbom-kamikudaku/minimum_element.png)

最小要素としてはデータフィールド、自動化対応、運用方法定義の 3 つがあります。一方で、実文脈では最小要素＝データフィールドとなっていることがあります。たとえば SBOM 生成ツールが「NTIA の最小要素を満たした SBOM をつくれます」と謳う、などです。

いずれにせよ、最小要素という言葉が登場したら、この NTIA の定義を指していると理解してください。

## 最小要素にどこまで追いついているか
筆者が触れたことのあるツール（syft などオープンソース系ツールと、Black Duck など有償セキュリティ製品）をベースに、2024/05 現時点の所感を述べます。

### データフィールド
おおよそ明確に出力できていると感じます。

コンポーネント名については、ツールによって表記揺れが生じることがあります。同じコンポーネントでも Vue.JS だったり vuejs-core だったり Vue.js Core だったりするわけです。だからこそ CPE や PURL など一意な識別子も要求されています。タイムスタンプはコンポーネント毎というよりも SBOM ドキュメントのメタ情報として挿入されています。

足りないのは依存関係で、今のところ直接的な依存関係がメインで、Black Duck など有償レベルで推移的な依存関係も検出できることがあるというくらいです。依存関係について少し解説すると、たとえば製品Xがいくつかのコンポーネントから成っている場合、

- 製品X
    - コンポーネントA
    - コンポーネントB
    - コンポーネントC
    - ……


直接的な依存とは X → A や X → B のことです。一方、推移的な依存とは A や B が何に依存しているかという話です。改めて現状を言うと、良くて推移的な依存も少し検出できるかも、という程度です。すべての深さを隈なく辿って、すべての依存を出力することは到底叶いません。そもそも SBOM はパッケージマネージャの設定ファイルからつくるものですが、その設定ファイルに直接的な依存しか書かれていないからです。

### 自動化サポート
サポートされています。

事前のインストールが済んでいれば、コマンドライン一発で SBOM を生成できます。インストール自体の自動化については、有償製品だと難しいかもしれません。オープンソース系であれば可能です。インストールスクリプトが用意されているケースもありますし、なかったとしても最悪 GitHub などから Zip をダウンロードして展開・配置するだけです。CI/CD に組み込むこともできるでしょう。

フォーマットについては、SPDX、CycloneDX、SWID の 3 つが挙がっていますが、SPDX と CycloneDX の 2 つのみ使われています。どちらの出力にも対応するのが主流ですが、[Microsoft の SBOM Tool は SPDX のみサポート](ref#18)しています。

### プラクティスとプロセス
国内ではまだプロセスを定めるほどの段階には至っていないイメージです。あるいはすでに進めている組織があるかもしれませんが、まだ観測はできません。

SIer がリーチする領域はどうでしょうか。2022/08 の[経産省の企業ヒアリング事例](ref#3) では SBOM の運用自体が希少です。2023/05 の[タニウムの調査](ref#17)によると、SBOM導入済みの企業は14％とのこと。一年以上経った現在では、もう少し進んでいるかもしれません。個人的には自動車や医療など人命にかかわる分野が先行しているイメージはありますが、内情はわかりません。Web や雑誌などで企業事例を見かけることはありますが、プロセスの整備にまで踏み込んだ情報はまだ見当たらない印象です。一方で、大手ベンダーで唯一ソリューションが充実している日立ソリューションズでは、[SBOM ソリューションとしてガイドライン策定支援や特定業界向け支援などを用意しているようです](https://www.hitachi-solutions.co.jp/sbom/sp/solution/sbom/)。

Web 系やテック系ベンチャーの領域はどうでしょうか。SBOM というよりは脆弱性対応を行っているイメージがあります。[yamory](ref#yamory) や [Snyk](ref#snyk) のような製品を導入したり、コンテナの場合は Trivy が有名でしょう。そうでなくとも GitHub などプラットフォーム側がサポートしていることもあります。この界隈はプラクティスやプロセスの取り組みがあればすぐに情報が共有されますが、本件に関しては筆者は見たことがありません。もしあったら教えてほしいくらいです。

ちなみに[経産省の手引 Ver2.0](ref#2) ではプロセスや対応モデルの解説が拡充されており、まさに本件の対応を後押しするものでしょう。

# Chapter-6 SBOMのつくりかた

SBOM のつくりかた。種類や方式などを扱います。

# 手段

## オープンソース
オープンソース系 SBOM 生成ツールはいくつも存在します。GitHub で探すことができます。

ツールの一覧については、[OpenChain Project Japan Work Group の SBOM サブグループがメンテしている一覧](ref#4)や[経産省の手引Ver2.0の付録](ref#2)などが詳しいです。後者については後述の有償ツールも扱ってます。

以下に著名なツールをいくつかピックアップします。URL と 2024/05/25 時点の GitHub Star 数を載せます。

- syft
    - https://github.com/anchore/syft
    - 5.6k
- SBOM Tool
    - https://github.com/microsoft/sbom-tool
    - 1.5k
- OSS Review Toolkit(ORT)
    - https://github.com/oss-review-toolkit/ort
    - 1.5k
- Trivy
    - https://github.com/aquasecurity/trivy
    - 21.6k

ツールのコンセプトは様々です。syft と SBOM Tool は SBOM 生成ツールです。ORT は SBOM 生成もメイン機能ですが FOSS policy automation and orchestration toolkit を謳っています。Trivy はコンテナ用脆弱性スキャナーがファイルシステムのスキャンと SBOM 生成機能もサポートしましたというテイストです。

## 有償ツール
構成管理やセキュリティ診断――いわゆるSCAツールは有償製品として提供されていることが多いです。以下に例を挙げます。

- [Black Duck](ref#black-duck)
- [Snyk](ref#snyk)
- [yamory](ref#yamory)
- [Cybellum](https://cybellum.com/ja/)
- [FOSSA](https://fossa.com/)

いずれも SBOM 生成ツールというよりは、SCA ツールが SBOM 出力機能を備えています。yamory のように[SBOMをインポート](ref#21)できるものもあります。

雰囲気を見たければ、Black Duck を覗いてみると良いでしょう。Black Duck はスキャナーで構成票をつくり、構成票からレポートや SBOM をつくるものですが、前者のスキャナーは Synopsys Detect としてオープンソース化しています。また、ドキュメントもあります。

- コード
    - https://github.com/blackducksoftware/synopsys-detect
- ドキュメント
    - [使用： Synopsys Detect（Desktop）](https://sig-product-docs.synopsys.com/ja-JP/bundle/bd-hub/page/ComponentDiscovery/DetectDesktop.html)
    - [All Properties](https://sig-product-docs.synopsys.com/ja-JP/bundle/integrations-detect/page/properties/all-properties.html)

## プラットフォーム
構成管理系のプラットフォームも SBOM 出力機能をサポートしていることがあります。

たとえば [GitHub がそうです](ref#10)。リポジトリの画面から Insights > Dependency Graph > Export SBOM と辿ることでダウンロードできます。

他には AWS も Amazon Inspect が対応していたり、IBM Cloud でも CycloneDX で生成できるようです。

- [Amazon Inspector による SBOM のエクスポート - Amazon Inspector](https://docs.aws.amazon.com/ja_jp/inspector/latest/user/sbom-export.html)
- [cyclonedx 形式でのソフトウェア部品表 (SBOM) の生成 | IBM Cloud 資料](https://cloud.ibm.com/docs/devsecops?topic=devsecops-generate-cyclonedx-sbom&locale=ja)

## 自作
既存の手段が気に入らない場合、自製することもできます。

処理としては「対象をスキャンしてコンポーネント情報を収集」と「収集した情報を SBOM に変換」の 2 ステップがあり、前者が難しそうです（スキャン方式は後述）。オープンソースの解析処理を拝借することはできるでしょう。[SBOM Tool は CycloneDX をサポートしない](ref#18) そうですが、その理由として「Component Detector を使えば自分でつくれるから」とあります。他にも上記のオープンソース系ツールを Fork して改造するなどもできます。

## (手作業)
SBOM は自動化を想定したものですが、特に日本では高品質な SBOM をつくる雰囲気と DX のしづらさの両面があり、手作業に頼りがちに感じます。

実際、SPDX には手作業で記述する用のサブセット [SPDX Lite](ref#19) が定義されていますし、トヨタはすでに SPDX Lite を導入しています。

# スキャン方式
SBOM はソースコード等をスキャン（解析）することでつくります。このスキャンの方式がいくつかあります。

## パッケージマネージャ
現在ソフトウェア開発ではパッケージマネージャを使うのが主流ですが、この設定ファイルをスキャンします。Node.js の NPM でいうと package.json、Python の pip でいうと requirements.txt、Java の Maven では pom.xml など。もちろん他にも色々あります。

どのプログラミング言語、どのパッケージマネージャに対応しているかは SBOM 生成ツール（あるいはその内部で使われているスキャン機能）次第です。ドキュメントで示されていることが多いので、確認すると良いでしょう。

## ファイルマッチング
あるソフトウェア X は xxx というファイルを持っているはずだから、xxx というファイルがあったならおそらく X を使っているはずだ――と推測できます。これがファイルマッチングです。

主に有償ツールが備えていますが、[Black Duck が現在非推奨にしている点](what_is_sbom#まず技術的には「いいえ」) はすでに述べました。素人の筆者に細かい事情はわかりませんが、推測するに、単純なファイルとソフトウェアの対応では精度が出ないことと、そもそも対応をデータとして持っておくことが大変であることが関係していると思います。

## スニペットマッチング
あるソフトウェア X はこれこれのスニペット（コード片） Y を使っているから、もしソースコードの中に Y が見つかったら、それはおそらく X からコピペしたものだろう――と推測できます。これがスニペットマッチングです。

これも有償ツールの機能という印象です。

参考までに、Black Duck ではスニペットマッチングにより検出した結果は「コンポーネントの候補」として利用者に見せます。利用者は各候補がコンポーネントなのか、それとも誤検出だから違うのかといった判断を下します。つまり疑わしいので一応検出したけど、判断するのはあくまで利用者に任せているわけですね。そうして判断を反映した後、その構成票から SBOM をエクスポートするわけです。

参考: [スニペットマッチについて - Black Duck Documentation](https://sig-product-docs.synopsys.com/ja-JP/bundle/bd-hub/page/ComponentDiscovery/Snippets.html)

## バイナリスキャン
ソースコードや設定ファイルなどはテキストファイルであり、中身を解析できますが、バイナリやアーカイブなどはかんたんには解析できません。しかし、これらを解析するスキャンを持つツールもあります。同様に有償ツールの機能という印象です。

当然ながら、精度面でも他の方式と比べると落ちます。

## コンテナスキャン
コンテナイメージもバイナリの一種ですが、解析可能な構造になっています。コンテナイメージはレイヤと呼ばれる概念でファイルシステムを差分的に表現しており、これを解析して再構築したファイルシステムをスキャンすればファイルマッチングやパッケージマネージャの設定ファイル解析に帰着できます。

Trivy などまさにコンテナイメージの解析を生業とするツールもあります。

## 環境スキャン
サーバーやクライアントPCの場合、OS のパッケージシステムを見ることでインストール済ソフトウェアを知ることができます。Linux の場合は yum、apt、pacman などがありますし、Windows だと「アプリと機能」から見ることができます（内部的にはレジストリに情報があります）。こういったツールの設定ファイルやデータストアの中身を解析するのです。また環境変数にも一部のソフトウェア（特にミドルウェア）の情報が含まれていることがあり、情報収集手段として使うことができます。

とはいえ、現在環境スキャンを内蔵したツールは筆者の知る限り、ありません。強いて言えば、Trivy は `trivy fs` にてファイルシステムのスキャンが行えるため、これでルートディレクトリを指定して丸ごとスキャンする、などはありえます。Linux はこのやり方でカバーできますが、Windows ではできませんし[そもそも想定されていません（対応していません）](https://aquasecurity.github.io/trivy/v0.27.1/docs/vulnerability/detection/os/)。

## まとめ
スキャン方式の主流はパッケージマネージャです。

ファイルやスニペットは有償製品が備えていますが、Black Duck を見る限りでは精度面で劣りそうです。バイナリスキャンも同様で、やはり精度は落ちますが、コンテナに限っては Trivy など進んだツールもあります。

環境スキャンを行うツールは無さそうですが、ディレクトリを丸ごと調べれば他の方式で代用できます。

# Chapter-7 SBOMの扱い方

SBOMの扱い方。閲覧、編集、整形や抽出など。つくりかたについては扱いません（[前章](howtogenerate)を参照してください）。

# SBOMの読み方
SBOM は機械可読用のフォーマットで書かれており、そのままでは読みづらいです。読む際のハードルは二つあって、SBOM がどう書かれているを知って読み解くことと、読みやすいよう変換や抽出の処理を適宜行うことです。

## どう書かれているかを知る
これは SPDX や CycloneDX の仕様を知ることに等しいです。[前章](sbom_format)を参照してください。

最初のうちは、仕様のドキュメントを見ながら SBOM とにらめっこすることになると思います。そこまで大掛かりな仕様でもないため、早ければ一日を待たずに要領をつかめるでしょう。少なくともプログラミング言語、スクリプト、その他 IaC ほど複雑ではありません。しかし、ドキュメントはほぼ英語であるため、英語の読解や翻訳ツールは必須です。

## 読みやすくするための変換や抽出を行う
以下 JSON で出力される場合を想定します。

### まずは整形
まず SBOM ツールによって生成された SBOM は、そのままでも読みやすい場合と読みづらい場合があります。後者の場合、Pretty Print が必要です。

以下に整形前と整形後（Pretty Print）の比較を載せます。

```json
// 整形前
{"$schema":"http://cyclonedx.org/schema/bom-1.5.schema.json","bomFormat":"CycloneDX","specVersion":"1.5","serialNumber":"urn:uuid:978f1aa6-7f51-4bd9-b5db-e8dfc181355e","version":1,"metadata":{"timestamp":"2024-05-23T19:03:03+09:00","tools":{"components":[{"type":"application","author":"anchore","name":"syft","version":"1.4.1"}]},"component":{"bom-ref":"08329a07b4eb8eac","type":"file","name":"./"}},"components":[{"bom-ref":"pkg:npm/%40sinonjs/commons@1.8.2?package-id=51a9235e6851e30d","type":"library","name":"@sinonjs/commons","version":"1.8.2","cpe":"cpe:2.3:a:\\@sinonjs\\/commons:\\@sinonjs\\/commons:1.8.2:*:*:*:*:*:*:*","purl":"pkg:npm/%40sinonjs/commons@1.8.2","properties":[{"name":"syft:package:foundBy","value":"javascript-lock-cataloger"},{
```

```json
// 整形後
{
  "$schema": "http://cyclonedx.org/schema/bom-1.5.schema.json",
  "bomFormat": "CycloneDX",
  "specVersion": "1.5",
  "serialNumber": "urn:uuid:978f1aa6-7f51-4bd9-b5db-e8dfc181355e",
  "version": 1,
  "metadata": {
    "timestamp": "2024-05-23T19:03:03+09:00",
    "tools": {
      "components": [
        {
          "type": "application",
          "author": "anchore",
          "name": "syft",
          "version": "1.4.1"
        }
      ]
    },
    "component": {
      "bom-ref": "08329a07b4eb8eac",
      "type": "file",
      "name": "./"
    }
  },
  "components": [
    {
      "bom-ref": "pkg:npm/%40sinonjs/commons@1.8.2?package-id=51a9235e6851e30d",
      "type": "library",
      "name": "@sinonjs/commons",
      "version": "1.8.2",
      "cpe": "cpe:2.3:a:\\@sinonjs\\/commons:\\@sinonjs\\/commons:1.8.2:*:*:*:*:*:*:*",
      "purl": "pkg:npm/%40sinonjs/commons@1.8.2",
      "properties": [
        {
          "name": "syft:package:foundBy",
          "value": "javascript-lock-cataloger"
        },
        {
          "name": "syft:package:language",
          "value": "javascript"
        },
        {
          "name": "syft:package:type",
          "value": "npm"
        },
        {
          "name": "syft:package:metadataType",
          "value": "javascript-npm-package-lock-entry"
        },
        {
          "name": "syft:location:0:path",
          "value": "D:\\quicklaunch\\sbom_kamikudaku\\htmx\\package-lock.json"
        }
      ]
    },
    ...
```

JSON の Pretty Print 方法は色々ありますが、たとえば[jq コマンド](ref#jq)が使えます。Python などプログラミング言語でも JSON を扱うライブラリはありますので、できる方は実装しちゃっても良いでしょう。オンラインツールも多数ありますし、最近ですと ChatGPT にお願いすることもできます（あまり大きなデータには不向きですが、jq コマンドの使い方を聞くこともできます）。特にオンラインの手段に頼る場合はコンプライアンスに注意してください。

筆者のおすすめは jq コマンドです。最初の導入とコマンドに慣れるところで多少苦戦するかもしれませんが、慣れたら便利です。ChatGPT に使い方を聞くこともできるのでそれほど困りません。

jq を用いた例については、[前章](sbom_format#確認-1)でも触れましたが再載します。

```
$ jq . cyclonedx14_htmx.json > cyclonedx14_htmx_pretty.json
```

### 次に必要に応じて抽出
整形ができたら、あとはテキストエディタや IDE などで JSON ファイルを読むだけです。

必要に応じて抽出できるといいでしょう。[前章](sbom_format#確認-1) でも少しだけ使っていますが、jq コマンドを使えば指定部分だけを取り出せます。RDB を SQL で操作するように、SBOM（のJSONファイル）も jq で操作できます。無論、そのためには、SPDX や CycloneDX など構造の理解も必要です。

## 余談: 変換ツールを使う
本項では自分で手を動かして JSON を直接読む想定で書きましたが、そこまでしなくても読む手段はあります。

一つはツールにインポートしてツール上で読むことです。特に有償ツールは内部で独自の構成管理構造を持っていて、そこから各種形式（Web画面はもちろんレポートビューやらSBOMやらPDFやらの出力も）で見せる仕組みになっています。ということは、ツールに情報をインポートできれば、ツール側が用意する手段で読めます。今回の場合、SBOM をインポートできるかどうかがポイントです。[Black Duck](https://community.synopsys.com/s/article/Black-Duck-SBOM-Import)や[yamory](ref#21)が対応しています。

もう一つ、比較的読みやすい形式に変換するツールがあります。[sbom2doc](https://github.com/anthonyharrison/sbom2doc) はアスキー的に見やすく表示しますし、[mdbom](https://github.com/HaRo87/mdbom) は Markdown 形式に変換します。もちろん、同じような発想で、ご自身で変換ツールをつくることもできます。話がビジネスに逸れますが、特に国内では（単に SPDX や CycloneDX を渡して終わりではなく）様々な形式が要求されることが想定されます。業界によっては顧客ごとに要求が変わるオーダーメイドがざらですし、そうでなくとも非エンジニアに「SBOMを読め」は酷でしょう。今のうちに色んな形式で出せる仕組みをつくっておくと備えになるかもしれません。

# SBOMの編集
生成した SBOM を編集する方法を見ていきます。

## 編集を自動化するには
できれば編集も自動化したいですよね。現状ではツールはまだ無いと思います。

行うとしたら、以下が必要です。

- 1: SBOM ファイルの指定箇所を指定内容に修正できるコマンドラインツール
- 2: 1 を使って具体的な修正を行うレシピ（コマンドラインなのか、何らかの設定ファイルにするのか）
- 3: 2 を実行する仕組み
- 4: 3 を CI/CD パイプラインなどに組み込むこと

## 手作業で編集するには
アプローチは三つあります。

一つは直接修正です。SBOM は JSON などテキストファイルですので、その中身を直接修正します。この場合は json 構造の直接編集に等しくなりますが、YAML や XML など他のフォーマットでも同様です。Linter や Validator などで文法チェックができるとなお良いですね。

もう一つがツール上での修正です。

- 1: 修正したい SBOM をツールにインポートして、
- 2: ツール上の管理画面で（インポートした SBOM からつくられた）構成票などを編集して、
- 3: それを SBOM でエクスポートする

というものです。ファイル直接編集よりは敷居が低そうですが、1 と 3 の間で微妙に「クセ」が入り込むデメリットがあります。たとえば Black Duck だと、エクスポート時には Creation Info などのメタ情報が（Black Duckによる固定値で）固定されます。仮にインポート時に正しい情報を入れていたとしても、それはなかったことになります。そもそも、1 のときに検出されなかったコンポーネントは、3 でも当然復活しません。2 の時に気付いて追加できていないと、いつの間にか情報を失ってしまいます――といったことを考える羽目になります。

最後の一つが、編集用のツールをつくって、それを使うことです。たとえば GUI で編集できるエディターがあると便利かもしれません。この手のツールは筆者の知る限り、まだ無いと思います。

## 余談: 手作業で編集するという発想はナンセンス？
※長めのコラム（より率直に言えばポエム）になります。

生成した SBOM をそのまま保存したり、誰かに渡したりできれば理想ですが、そうもいかない場合があるかもしれません。特に国内では 積極的な手作業を容認してでも SBOM の完全性を高く求める傾向があると感じます。

[NTIAの最小要素](ref#22)としては「誤りの許容」があり、和訳の方を引用しますが許容について書いてあります。

> NTIA は、「ソフトウェアサプライチェーンの管理方法は日々進化しているため、SBOM を導入・運用する初期フェーズに
おいて完全性が欠如する可能性がある」としており、その上で、「偶発的な誤りに対しては明確に許容すべきである」としてい
る。これにより、ツールの継続的な改善が促進されるとしている。 / [経産省の手引 Ver1.0](ref#1)より抜粋

筆者としても許容してもいいと思いますし、むしろ許容で済むように自動化で済む方法や仕組みに努力をするのがいいと思うのですが、軽いでしょうか。もちろん理想はまさに NTIA が同文書で述べているとおり、その組織にあった運用を定義することですが、手作業はできる限り減らした方がいいし、手作業したからといって完全性を担保できるかというとそうでもないし、むしろ人によって品質にバラツキやムラが生じるとも思っています。そもそも完全なSBOMなど現時点では難しいので、現存するツールの中で高品質なものを使ってそれの出力を信じるのを基本にする、でいいと思っています。

そもそも手動編集自体がナンセンスとも感じます。GitHub やネットで検索しても情報はほぼヒットしません。ナンセンスすぎて誰も取り扱ってないからではないでしょうか。唯一、[Redditのスレッドを一件見つけました](https://www.reddit.com/r/cybersecurity/comments/1674nq1/tool_to_manually_edit_sbom/)が、編集したいという質問主に対し、回答者は「そもそもなぜ編集する？」と前提から疑っています。筆者も回答者寄りの感覚です。

一方で、国内ではそうもいってられません。技術に造形の深いテック企業や一部のベンチャーは少数派ですし、高品質を求める国民性もあります。SBOM についても「高品質なものをつくれ」「技術には投資する気はないが人を使えばいけるだろう」となってもおかしくはありませんし、すでになりつつあるように感じます。

何よりもネックなのがソフトウェアの浸透度でしょう。たとえばパッケージマネージャを用いた開発は今どき当たり前であるどころか、前提とさえ言えると思いますが、国内では（界隈にもよりますが SIer で見ると）むしろ少数派です。Git を知らないエンジニアや、オープンソースを（そもそも想定されてないか厳しい管理や申請を通さなければ）使えない現場も多いです。利用者の視点で見ても、デジタルが及んでおらずマンパワーで頑張っている部分がたくさんあります。DX も叫ばれていますし、IT後進国との揶揄さえあります。このような現状ですと、当然ながら SBOM が想定する「パッケージマネージャベース」「オープンソースベース」のソフトウェア開発 **に当てはまらない** ケースが多くなります。当てはまらないので SBOM に関する既存の恩恵も使えず、かといってデジタルで上手くやるための投資もせず、でも法や要件として今後必要なのは明らかだから対応はしないといけなくて、じゃあ今までどおり手作業で頑張るしかないな――となってしまうわけです。

SBOM が現場の首を絞め、現場から忌み嫌われる存在になれば形骸化につながります。自動化ベースの一貫したセキュリティを確保できなければ、今後ますますサイバー犯罪に付け入られるかもしれません。考えすぎかもしれませんが、どうにも胸騒ぎがするのです。

文化も絡むため、個人で今すぐ何かができるはずもありませんが、逆を言えばビジネスの余地が多く転がっているとも言えます。国内の SBOM が盛り上がるのはこれからです。SBOM を学ぶ一人のエンジニアとして、ビジネスマンとして上手く生かしたい、できれば変えていきたい――そう願ってやみません。

# Chapter-8 SBOMと脆弱性

SBOMを用いて脆弱性をどう扱うか。

# SBOMを用いて脆弱性の有無を調べる
SBOM の元々の目的は脆弱性であり、現在でも主目的の一つです。SBOM を使うことで脆弱性対処（特に見つける部分）のリードタイムを短縮できます。

## Narsion を探す
探し方は単純で、**脆弱性を持つ Narsion が SBOM に含まれるかどうかを調べれば** 良いのです。Narsion は造語ですが Name + Version を指します。

脆弱性とは、あるソフトウェアのあるバージョンに存在する「攻撃されうる弱点」でした。SBOM とは何のソフトウェアを使っているかの一覧でした。なので、SBOM が正しくつくられているならば、その弱点を持つソフトウェアかつバージョン（つまりは Narsion）が含まれているかどうかを調べればいいだけです。

たとえば Log4j 2.15.0 に脆弱性があるぞとわかった場合、SBOM の中を見て「Log4j 2.15.0」があるかどうかを調べればいいのです。ない場合は一安心ですし、ある場合は「最悪 Log4j 2.15.0 の脆弱性を突かれて何かされるリスクがある」わけですから対処が必要になります。

ポイントはバージョンも含まれていることであり、同じ Log4j であっても 2.20.0 ならセーフです。とはいえ複数のバージョンを範囲で含んでいるケースも多いので、脆弱性情報をよく確認する必要があります。Log4j の脆弱性 [CVE-2021-44228](https://nvd.nist.gov/vuln/detail/CVE-2021-44228) を例にすると、「2.0-beta9 ～ 2.15.0」「セキュリティ リリース 2.12.2、2.12.3、および 2.3.1 は除く」「2.16.0 から当該機能は完全に削除」などが書いてあります。

## Narsion のマッチング
具体的な処理を言うと、SBOM 側のコンポーネントと、脆弱性 DB 側のコンポーネントをマッチングさせれば良い、と言えます。

脆弱性として「Log4j 2.15.0」が報告された場合、SBOM の中に「Log4j 2.15.0」があれば、一致する（マッチする）と判断できます。この SBOM（のもととなっているソフトウェア）は Log4j 2.15.0 の脆弱性の影響を受ける可能性があると言えるわけです。

マッチングアルゴリズムは主に3つあります。

### 1: コンポーネント名を使ったマッチング
脆弱性 Log4j 2.15.0 を探したい場合、SBOM 内を `Log4j 2.15.0` という文字列で検索すれば良いです。あるいは `Log4j` を検索した後、マッチした分についてバージョンが `2.15.0` のものを検索する、もアリでしょう。方法は色々ありますが、原始的な文字列一致で頑張るというわけです。

これは単純ですが、上手くいきません。

というのもコンポーネント名やバージョン名には **表記揺れが生じる** からです。たとえば `Log4j` だったり `log4j` だったり `Apache Log4j` だったりします。どんな表記を使うかのルールなんてありませんし、仮にあったとしても守られないでしょう。製品側にも正式な名称は存在しますが、同様に守られるとは限りません。そもそも正式名称と「パッケージマネージャ等で使う用の名称」は違います――など、挙げればきりがありません。表記は揺れます。

### 2: 固有の識別子を使ったマッチング
CPE や PURL(Package URL) など、コンポーネントには固有の識別子があります。これを使えばマッチングは可能です。表記揺れの余地もありません。

デメリットはこれら識別子がそもそも普及しきっていないことです。また識別子は存在しているが、ツール側が対応していなくて SBOM に書き込まれないケースもあります。

現状、業界ではこのアプローチを目指している空気感があると思います。今後の啓蒙と周知に期待したいところです。

参考:

- CPE
    - [共通プラットフォーム一覧CPE概説 | 情報セキュリティ | IPA 独立行政法人 情報処理推進機構](https://www.ipa.go.jp/security/vuln/scap/cpe.html)
- Package URL
    - [package-url/purl-spec: A minimal specification for purl aka. a package "mostly universal" URL, join the discussion at https://gitter.im/package-url/Lobby](https://github.com/package-url/purl-spec)


### 3: データベースとルールベースによるマッチング
1: と 2: の合わせ技であり、複数のルールや条件をあらかじめつくっておくことで「これこれの条件に当てはまったらこれこれのコンポーネントだとみなす」とするものです。

単純な例でいうと、「もし `Log4j` or `log4j` or `Apache Log4j`」であれば「Log4j」とみなすとか、大文字小文字は無視して比較するとか、空白の詰め方（パディング）も一意に処理して比較する、といった形になります。もちろん CPE や PURL を用いたルールも使えます。このように例やルールを貯めていきます。

バージョンについても色んな書き方を想定して、それらすべてを吸収できるように例とルールを拡充させます。 Ver1.2.3 の一つを見ても `1.2.3` とか `ver 1.2.3` とか `v1.2.3` など表記揺れが生じます。SBOM の規格では Name と Version は分かれているはずですが、Name の方にバージョン番号も入れちゃうツールもあったりします。一筋縄にはいきません。

せめてもの救いは、SBOM 生成ツールごとに冪等性があることでしょうか。たとえば「Log4j」「v2.15.0」と出力するツールがあったとしたら、このツールは（実装が変わらない限り）常に「Log4j」「v2.15.0」と出力します。なので、一度対応やルールを追加したら、その分は吸収できます。もちろん実装が変わらない保証はなく、メンテナンスは必要です。

デメリットは最初に一通り精度が出るまで貯める作業に時間がかかることと、その後もメンテナンスが必要になることです。力技ゆえの限界があります。

### 4: 推論的なマッチング
自然言語処理や生成 AI の力を使ってマッチングを試みるアプローチです。本質的には「表記が揺れている名前のマッチング」となるでしょう。

筆者も詳しいことはわかりません。少なくとも製品やサービスとしては見たことがないし、まだ無いと思いますが、関連した研究ならすでに論文があったりするのでしょうか。

デメリットは、そもそも事例が無いことや当然実現できるかわからないこと、そして精度 100% でない以上、人間によるレビューが必要になることです。

# 脆弱性データベース
SBOM とは関係がありませんが、取り上げておきます。

脆弱性は専用の組織が取りまとめています。データベースは公開されており、ここにアクセスすることで脆弱性情報をゲットできます。

## NVD
最も有名なのは米国の NVD でしょう。実際は Mitre 社に委託されており、情報としては CVE のデータベースになります。

- [CVE Website](https://www.cve.org/)
    - データはウェブサイト上で検索するか、[CSVなどのダウンロード](https://www.cve.org/Downloads)ができます

## JVN
国内でも JPCERT/CC と IPA による JVN（Japan Vulnerability Notes）があります。NVD の情報をまとめている他、[情報セキュリティ早期警戒パートナーシップ](https://www.ipa.go.jp/security/guide/vuln/partnership_guide.html)に基づいて報告された脆弱性も扱っています。たとえば NVD では捉えられていない国産ソフトウェアの脆弱性も扱っています。

- [Japan Vulnerability Notes](https://jvn.jp/)
    - データは[JVN iPedia](https://jvndb.jvn.jp/)から取得できます。API もあります

## その他のデータベース
その他にも脆弱性データベースは存在します。

- Black Duck
    - [Black Duck オープンソース Knowledge Base | シノプシス](https://www.synopsys.com/ja-jp/software-integrity/software-composition-analysis-tools/knowledgebase.html)
    - オープンにはなっておらず、Black Duck の製品内からアクセスできます
    - NVD 以外にもオープンソースの脆弱性情報を広く扱っています、公式サイトでは「247,000+ 独自の脆弱性データ」とあります
- GitHub
    - [GitHub Advisory Database](https://github.com/advisories)
    - [github/advisory-database](https://github.com/github/advisory-database)
    - NVD 以外にも様々なデータベースから情報を集めているようです

# 脆弱性管理の分担
エンジニアひとりひとりが脆弱性データベースを日々モニタリングするのは現実的ではありません。

## PSIRT
一般的には PSIRT（Product Security Incident Response Team, ピーサート）など専用のチームをつくって専任させ、彼らがまとめた情報を社内に展開するアプローチを取ります。その際、脆弱性データベースに載っている情報を垂れ流すのではなく、わかりやすい形で提示します。組織内で独自のプロセスやシステムがあるなら、それらにも則ります。特に脆弱性は最新の情報がいつくるかわかりませんからウォッチが必要です。新着があれば、すみやかに整理して、展開しなければなりません。

PSIRT を用意できるほどの企業規模でない場合、専任者を立ち上げるか、エンジニア達で各々対応するかになるでしょう。

## SBOM は役に立つか
役に立ちます。

すでに述べたとおり、SBOM 生成ツールを使って SBOM をつくり、上記の脆弱性データベースの情報を使ってマッチングを行うことで脆弱性を調べられます。特に自動化できることが大きいです。従来は PSIRT の人達が手作業で仕事していましたが、全部、あるいは大部分を自動化できる余地があります。

とはいえ、脆弱性の知見のない素人が一からつくるのは骨が折れますから、脆弱性管理製品を使うのが一般的です。Black Duck、yamory、Snyk などはすでに紹介しました。またオープンソースで、コンテナに絞るのなら Trivy が使えます。GitHub も上述したように Advisory Database を公開しており、GitHub 利用者はシームレスに脆弱性情報と向き合えます。このように製品を使う場合は（脆弱性管理という文脈では）SBOM の出番はありません。強いて言えば、これら製品が SBOM のインポートに対応していることがあり、入手した SBOM をインポートして脆弱性を診断するくらいでしょうか。

## SBOM を脆弱性管理に適用する
少人数の組織で新しく始める場合は、自分たちでつくるなり製品を探して使うなりすればよいでしょう。

問題はすでに PSIRT など専用チームが存在していて、ここに新たに SBOM を導入する場合です。既存のプロセスを変えて、SBOM を使うプロセスを新たにつくらねばなりません。ツールの導入や学習も必要でしょう。また PSIRT に限らず、現場や社内全体への影響もあると思います――と、業務改善レベルの大仕事になるでしょう。技術的な難易度よりも、組織や業務をいかに改善するかという別ベクトルの難しさが待っています。

# 脆弱性情報のコミュニケーション
脆弱性情報も中々に煩雑であり、各自が好き勝手な形式でやりとりしていてはコミュニケーションコストが無駄にかかります。

脆弱性についても共通言語化の試みや事例があります。

## CPE
- Common Platform Enumeration
- [共通プラットフォーム一覧CPE概説 | 情報セキュリティ | IPA 独立行政法人 情報処理推進機構](https://www.ipa.go.jp/security/vuln/scap/cpe.html)

SBOM が登場する以前から存在するもので、ソフトウェアを一意に識別するための識別子体系です。すでに NVD など脆弱性プラットフォームで利用されており、SBOM において脆弱性コンポーネントとマッチングする際にも使えることはすでに述べたとおりです。

## CVE
- (Common Vulnerabilities and Exposures
- [共通脆弱性識別子CVE概説 | 情報セキュリティ | IPA 独立行政法人 情報処理推進機構](https://www.ipa.go.jp/security/vuln/scap/cve.html)

CPE と同様に、SBOM 登場以前から存在するもので、脆弱性各々を識別するための識別子体系です。[CVE-2007-5000](https://cve.mitre.org/cgi-bin/cvename.cgi?name=CVE-2007-5000)のような書き方をします。

## CVSS
- Common Vulnerability Scoring System
- [共通脆弱性評価システムCVSS概説 | 情報セキュリティ | IPA 独立行政法人 情報処理推進機構](https://www.ipa.go.jp/security/vuln/scap/cvss.html)

脆弱性の影響度を0.0～10.0で表現する体系です。数字が大きいほど深刻であることを意味します。また段階としては低、中、高、緊急の 4 段階があってわかりやすいですが、体系自体や計算方法はそれなりに複雑です。また 2023/11 にはバージョン 4.0 になりました。

## VEX
- Vulnerability-Exploitability eXchange
- [Vulnerability-Exploitability eXchange (VEX) – An Overview](https://www.ntia.gov/files/ntia/publications/vex_one-page_summary.pdf)

NTIA が定めた脆弱性を記述するフォーマットです。SBOM と同様、プログラムで扱うことを想定しています。

内容としては特に脆弱性に対する対処要否と、もし要なら具体的に何をすればいいかといった「対処に関する情報」の共有を重視しています。脆弱性の状態としては以下 4 段階があります。

- Not affected, 影響なし（対処は不要です）
- Affected, 影響あり（対処が必要です）
- Fixed, 製品側で修正済み
- Under Investigation, 調査中

スキーマについては以下から参照できます。

- https://github.com/oasis-tcs/csaf/blob/master/csaf_2.1/json_schema/csaf_json_schema.json
- https://github.com/oasis-tcs/csaf/blob/master/csaf_2.0/json_schema/csaf_json_schema.json

# Chapter-9 SBOMとライセンス

SBOMを用いてライセンスをどう扱うか。

# SBOM を用いてライセンスリスク判断のヒントを得る
オープンソースソフトウェアにはライセンスがあり、ソフトウェアを使う以上は遵守が必須です。破っていることが発覚した場合、最悪訴訟されたり、評判に傷がついたりする可能性があります。

SBOM にはライセンス情報を持たせることもできるため、ライセンスリスクの判断に役立つことがあります。SBOM がライセンス情報を含むかどうかは SBOM 生成ツール次第です。規格としては SPDX は Ver 2.x ではライセンス管理がメインですし、CycloneDX にもライセンス情報を扱う構造はありますが、実際に生成ツールがそれらに則ってライセンス情報を書き込むかは別問題です。

ここまで「判断のヒント」とか「リスクの判断に役立つ」といった言い方をしてきましたが、意図的です。**ライセンスリスクの有無を直接判定することはできません**。ライセンスも所詮は文章にすぎないため、最終的には人間が判断しなくてはなりません。[Black Duck のドキュメント](https://sig-product-docs.synopsys.com/ja-JP/bundle/bd-hub/page/Licenses/Overview_LicenseProcess.html) では「推奨されるワークフロー」の解説があったり、法務担当者が登場してきたりと中々煩雑な運用が想定されています。

# SBOM にはどんなライセンス情報が載るか
まず各コンポーネント毎の属性として「このコンポーネントが持つライセンス」があります。実際に書き込まれるかは SBOM 生成ツール次第ですが、ともかく、属性としては用意されています。

次に、SPDX 2.x ではライセンス情報を別途保存します。検出したライセンスの全文を保存するフィールドがあります（[Other licensing information](https://spdx.github.io/spdx-spec/v2.3/other-licensing-information-detected/)）。また、ライセンスの扱い方がそもそも規定されていて、よくあるライセンスには固定的な名前が使われ、これに該当しないライセンスを Other Licence として扱います。前者のよくあるライセンスは [SPDX license List](https://spdx.github.io/spdx-spec/v2.3/SPDX-license-list/) としてまとめられています。

# ライセンス管理の分担
脆弱性と同様、ライセンスについても素人には荷が重いです。一般的には OSPO（Open Source Program Office）など専用のチームをつくって全社や現場を支えてもらいます。古典的にはルールやプロセスを敷いて、現場のエンジニアに任せるというあり方がメインでしょう。

SBOM を使うと、この OSPO の人達が楽をできるかもしれません。極論、SBOM 内に含まれた情報だけを使うようにすれば運用はシンプルになります。SBOM を提出してもらうようにするだけでもだいぶ違いますし、システムに登録してもらえるとなお良いです。さらにライセンス情報の表示、確認、結果のレビューといったワークフローまでシステム化できたら、あとは人間が判断するだけです。

ですが、ライセンスもやはりSCAツールなどセキュリティ製品を使うのが主流だと思います。むしろ SBOM はおまけでしょう。元々 SCA ツールでライセンス管理は出来ているが、SBOM が盛り上がってきた、なので SBOM 出力機能もつけた――という流れです。そういう意味では、ライセンス管理については、SBOM どうこうというよりも素直に製品を使うのがベストなのかもしれません。

もちろん独自の仕組みをつくることも可能です。特に SPDX はライセンス管理を重視しているだけあって、ライセンス情報の本質をある程度捉えていますから参考にできます。

# Chapter-10 SBOMと透明性

SBOM がもたらす透明性について。

# 透明性を確保する意義
SBOM は「何を使っているかの一覧」です。食品の栄養成分表示と同様、何を使っているかが見えます。

内部のカラクリが見えることを透明性があると表現しますが、透明性があると健全性が上がります。逆に透明性がない、いわゆるブラックボックスの状態だと、いいかげんな運用はもちろん、悪用さえもできてしまうので健全ではなくなります。形骸化や腐敗が生じます。要は見えていると悪さがしずらくなり、水準が上がります（というより下がりにくくなります）。

また競争力が増えるメリットもあります。どのソフトウェアが何を使っているかがわかるので、他の利用者や事業者が参考にできるわけです――もっともオープンソースであればすでに見えるようになっているはずですが、SBOM の普及に伴い、オープンソースとは無縁だった組織や製品にも透明性がもたらされます。それらが何を使っているかも見えるようになるのです。

# 課題
SBOM によって透明性がもたらされますが、現実はそうかんたんには行きません。

## 素人には読めない
SBOM は機械可読用のフォーマットですから、エンジニアではない素人が読むのは難しいです。読むためのハードルが上がると、形骸化に繋がります。たとえば「読まれる可能性が低いから」「一応記述することにはなってるけど、読まれる可能性はほぼないから」と虚偽の内容が書き込まれるかもしれません。

## 書いてある内容が正しいとは限らない
SBOM はただのテキストファイルですから改ざんの可能性や所有者の真正性を証明できません。

ここを何とかするためには、デジタル署名などの仕組みが必要です。この件はすでに議論が行われており、[in-toto](ref#in-toto)などツールもあります。

## 公開の仕組みがない
SBOM を誰がどこに置いて、それをどのように見せるのか――という部分はまだ何も決まっていません。「SBOM の共有」「SBOM の受け渡し」といった言い方をされることもありますが、本質は同じことです。

[経産省の手引Ver1.0](ref#1)では以下のように述べられています。

> SBOM 共有について、様々な方法が想定される。例えば、製品内から SBOM を確認できるよう製品に組み込む方法、利用者がアクセス可能なリポジトリにおいて公開する方法、Webページ上で公開する方法、共通の SBOM ツールを用いて SBOM データを共有する方法等が想定される。利用者に対する SBOM 共有を行う場合、SBOM 対象ソフトウェアの特性や更新の頻度、利用者における SBOM 利用状況等を踏まえ、どの方法で共有するかを検討することが望まれる。

要は方法が色々あるのと、シチュエーションも色々あります。たとえばウェブサイトのように広く公開して誰でも閲覧できるようにする文脈もあれば、特定顧客や別チームに受け渡す文脈、あるいはドキュメントのように製品に同梱する（利用者は読める）文脈もあります。しかし、透明性という意味では、誰でも見れるよう広い公開を目指すべきでしょう。

## 公開のインセンティブがない
オープンソースと同じで、「世の中のためにとりあえず公開しましょう」「公開する労力くらいは背負います」との精神を持てる人や組織は少ないです。

そもそもネガティブな理由により公開という発想そのものが潰されがちです。情報は何かと機密情報扱いにしがちですし、要らぬ情報を公開してしまうリスクが頭をよぎるのでだったら最初から公開なんてさせないと倒すのもあるあるですし、資本主義的には競争は苛烈ですから隠せるものはできるだけ隠します。また文化的にも、昔はインターネットに何でも公開するノリが強かったですが、今は技術と手段、何より多様性の文化が進んで、コミュニティが分かれています。Google 検索は唯一のアクセス手段ではなくなっていますし、インターネットレベルでみんなが集まる場所は X（旧 Twitter）くらいでしょう、そして X にも若者はあまりません――と、文化的にも広い公開は下火になっています（なっていると感じます）。

加えて、わざわざ **公開の手間を負うほどのインセンティブがそもそもありません**。

オープンソースの他にはサステナビリティがそうだと思いますが、中長期的な視座に立って貢献するという崇高な意識がなければ中々受け入れられないでしょう。SBOM に関する法の整備は今後進むと思いますが、透明性の観点ではどこまで重視されるかわかりません。期待はできないと思います。公開を是とするような文化や価値観を上手くつくれたら、ワンチャンあるかもしれません。いずれにせよ、スケールもスパンも大きな話です。

# Chapter-11 SBOMとオーバーオールセキュリティ

SBOMとオーバーオールセキュリティの関わり。

# オーバーオールセキュリティ
造語ですが、**オーバーオールセキュリティ(Overall Security)** とは開発や運用を含む工程全体の目線でセキュリティを確保することです。ソフトウェアサプライチェーン、DevSecOps、SBD（セキュリティ・バイ・デザイン）、SDLC（ソフトウェア開発ライフサイクル）など、この手の概念は多数存在しますが、その「この手の概念」を扱う言葉がないので便宜上つくりました。

# SBOM はオーバーオールセキュリティを加速させる
SBOM は構成要素の一覧化という「別に目新しい発想ではないが面倒くさかった営み」を業界レベルで加速させるものであり、これがオーバーオールセキュリティの背中を押します。

セキュリティは、特に予防という意味では「可視化」が重要です。対象を可視化できたら、あとはそれぞれについて考えれば良いだけですし、そもそも[前章でも述べたとおり](sbom_and_transparency)可視化されていることが健全性を生みます。

SBOM はただの一覧ですが、法や要件として要求されそうなほどクリティカルなものであり、上手く便乗できればビジネスとして美味しいです。この流れに乗らんとするセキュリティ企業も増えてきています。大手ベンダーやセキュリティベンダーは、既存の製品やソリューションに SBOM の解説を入れたり、製品に SBOM 出力機能を追加したりして間口を広げています。

そもそも業界自体が SBOM を通じて「結局重要なのは SBOM 単体よりもその先」に気付きつつあります。特にソフトウェアサプライチェーンは「その先」として言語化された本質の好例でしょう。製造業のサプライチェーンの概念を、ソフトウェアの世界にも適用しようとしています。また二大規格である SPDX と CycloneDX がどちらも汎用的なデータ構造を取れるようにしたのも、Software BOM だけでなく XXXX BOM、ソフトウェア以外の構成管理も扱えるようにするためです。

# オーバーオールセキュリティのVFARP
オーバーオールセキュリティとして何を行えばいいでしょうか。筆者は 5 つに絞れると思っています。VFARP という形でまとめてみました。

- Visualization/可視化
    - どのフェーズで何を使っているかを見える化します
    - フェーズそのものも全部見える化します
- Format/フォーマット
    - どんな形式で可視化するか
    - 特に手作業だと限界があるため自動化したいが、プログラムは人間じゃないのでどんな形式か事前に想定させないと動かない
        - SBOM の場合、SPDX や CycloneDX といった形で仕様化されている
        - 脆弱性についても VEX がある
- Attribute/属性
    - 何を可視化するか
    - SBOM の場合、[NTIAの最小要素](ref#22)が鉄板（コンポーネントの名前、バージョン、製造者名 etc）
- Review/レビュー
    - 可視化したものを検査する
    - 問題のあるものを使っていたり、使ってるものが足りなかったりするといけないため
- Process/プロセス
    - 可視化とレビューをどうやるかを決める
    - そうしないとばらつきが生まれて信用できないため
    - [NTIAの最小要素](ref#22)がいう「プラクティスとプロセス」そのもの


# Chapter-12 📘第3部「SBOM の課題とアイデアを知る」

SBOM に関する発展的なトピックであり、筆者の見解に基づくビジネスアイデアを整理しています。

注意点:

- ビジネスとは組織やチーム内部の改善も含みます
    - またマーケティングフレームワークで行うような市場分析や顧客の絞り込みなどは行っていません
- 課題については文中で直接的あるいは間接的に言及することがある程度です、明示的に詳しくまとめてはいません
- 第3部では概要欄で用語を定義していきますが、基本的に造語です

# Chapter-13 統合管理

# 1: コンポーネントマネジメント

## 概要
**コンポーネントマネジメント** とは、ソフトウェアコンポーネント情報を一元管理することです。

## 結局コンポーネントの一元管理が必要
SBOM を作成するとは「対象ソフトウェアが使っているコンポーネントをリストアップする」ことに他なりませんが、このとき各コンポーネントの情報をどうやって入手するかという問題があります。

古典的には SBOM 作成者が各々で調べて記入しますが、言うまでもなく大変です。たとえば会社全体で Vue.js を使うプロジェクトが 400 あったとしたら、400 PJ の各担当者がそれぞれ自分で Vue.js に関する情報を調べて記入する羽目になります。Excel や社内システムなどに、律儀に名前、バージョン、入手先、ライセンス――を記入するわけです。とんでもなく無駄です。

現代の SBOM 生成ツールを導入していると、解析対象のパッケージマネージャの設定から取得されますので各担当者が手作業で記入する必要はありません。しかし、すでに述べたとおり、[ツールが完璧に各フィールドを埋めてくれるとは限りません](sbom_format#フォーマットと現実)し、特に国内では「ツールが出した出力をそのまま使う」が許されず、[要求水準に至るよう手作業の修正を求められる](howtoadjust#余談%3A-手作業で編集するという発想はナンセンス？)かもしれません。ツールが Vue.js のライセンス欄を埋めてくれなかった場合、自分で確認して追記することになります。

つまり、SBOM 作成時には各コンポーネントの各属性を埋める必要があるのですが、その情報を各担当者が手作業で調べるのは無駄なわけです。どうすればいいかというと、コンポーネント情報を一元化すれば良いのです。全社的なデータベースを立ち上げ、コンポーネント情報を集約します。Vue.js の各属性情報も全部入れます。そうすると、全社員はここからデータを取ってくるだけでよくなりますし、システムとの連携を上手くつくりこめば自動反映ができて手作業自体をなくせます。

## データベースとマッチングが必要
コンポーネントマネジメントを実現するためには、データベースとマッチングが必要です。

データベースは全社的なシステムとして自由にアクセスできるのが望ましいです。自動化も想定するとアクセスの回数も時間帯もばらつくでしょう。高い可用性も求められます。

マッチングについては、あるコンポーネント情報をどうやって引くかという話であり、[脆弱性を検出するときと本質的には同じ問題](sbom_and_vuln#narsion-のマッチング)です。厄介なのは、現状コンポーネントデータベースなるものが世の中に無いことでしょう。そのような権威や組織もありません。ゆえに、コンポーネント情報は私たちが自ら頑張って集めなくてはなりません。あるいは業界を巻き込むなり、オープンソースにするなりして皆を巻き込むか。

# 2: SBOMマネジメント

## 概要
**SBOMマネジメント** とは、SBOM の情報自体を統合管理することです。

## データドリブン経営やナレッジマネジメントに繋げる
SBOM とは、あるソフトウェアがどんなソフトウェアを使っているかの一覧ですが、これは見方を変えれば「あるチームやプロジェクトがどんなソフトウェアを使っているか」と見ることもできます。仮に社内に 2000 の開発プロジェクトがあり、そのすべてについて SBOM を作成したとすると、これは 2000 プロジェクト分の「どんなソフトウェアを使っているか」が存在することに等しいです。データの宝庫に思えてきませんか。

## 活用例
多数考えられます。

### ナレッジマネジメント
どんなプロジェクトの、どんな製品で、どんなソフトウェアを使っているか――そんな傾向がわかるので、似た開発を行う場合には参考にできます。

また情報の紐づけ方次第で Know Who や Know How とも繋げられます。たとえば「このソフトウェアを使っている他のプロジェクト」や「この部門の、Vue.js を用いたプロジェクト」といったこともわかるようになります。

### リスク対応のリードタイム短縮
脆弱性やライセンスリスクの情報（特に対処状況）を紐づければ、ソフトウェア情報からそれらの情報を辿れるようになります。

前述のコンポーネントマネジメントは「コンポーネント情報を各担当者が各々調べさせるのではなく一箇所に集める」ものですが、同じ発想は「脆弱性の対処」や「ライセンスリスクの判断」にも適用できます。ただし、単に「X という脆弱性はこう対処しました」との情報を共有するだけではあまり役に立ちませんし、探しづらいので、このSBOMマネジメントも必要なのです。要はSBOMマネジメントにて管理する情報――コンポーネントもそうですし、プロジェクトや部門といったメタ情報もそうですが、これらと「対処情報」を結びつけることで、前者の文脈から後者を引けるようになりますし、前者の文脈があるからこその後者の対処なのだと文脈ありきの情報として使うことができます。

### データドリブン経営
成果を出しているプロジェクト、出していないプロジェクト、あるいは赤字など足を引っ張っているプロジェクトのソフトウェア利用傾向も見えるかもしれません。

成果に限らず、状態との因果関係も導けるかもしれません。たとえばエンゲージメントや勤務時間と組み合わせることで、それらの高低をソフトウェア利用情報というパラメーターで推し量れるでしょう。

### 分析の練習や試行
SBOMマネジメントをデータ基盤として整備し、社内から自由に使えるようにすれば、データサイエンティストなど分析を生業とする人達にとっての格好の題材となるでしょう。

また最近では生成AIも盛り上がっておりますので、データの食わせ方やプロンプトの書き方を試すのもアリです。

### コンポーネントマネジメントの足しに
SBOM はまさにコンポーネント情報を含んでますので、前述したコンポーネントマネジメントのデータ拡充にも役立ちます。

# ポテンシャル
コンポーネントマネジメント（以下COMPM）はできればグローバルで管理したいです。脆弱性はすでに NVD ベースで管理されていますが、中央集権的ですので、オープンソースのように誰にでも開かれたあり方がいいかなと思います。世の中にはコンポーネントが多数、いや無数と言えるほど存在しているでしょうから、中央集権的管理では限度があります。GitHub や DockerHub といったイメージになるのでしょうか。だとしたら、コンポーネントマネジメントプラットフォームなるものを最初に立ち上げて、広めた企業が勝者になるのかもしれません。

COMPM は社内で管理することもできますが、コンポーネントは多いですから、一社だけで情報をまかなうのが厳しい気がしています（筆者が無知なだけで、Google や OpenAI のようにクロールをすれば可能かもしれませんが）。あるいはオープンソースソフトウェア以外に絞るのであれば、現実的な規模になるかもしれません。たとえば Apple やトヨタなど、自社製品のサプライチェーンを牛耳れる力を持つ企業であれば、ソフトウェア以外のジャンル（たとえばハードウェア）について「自社が使うコンポーネントすべてを管理する」も可能かもしれません。別に名だたる大企業でなくても、業界次第では十分現実的な規模に収まる気がします。というより、オープンソースソフトウェアが規模が桁違いなのです。一方で、Black Duck で知られるシノプシス社は独自のデータベースを持っていたりしますから、投資をすれば可能だったりするのでしょうか。

次にSBOMマネジメント（以下SBOMM）については、グローバルの管理は難しそうですね。会社名ならさておき、プロジェクト名や部門名や個人名を全世界に晒すわけにはいきません。SBOMMについては、社内向けの取り組みになるでしょう。

いずれにせよ、社内で成功できたら次は外に売りだせます。COMPM の場合は API を提供してデータアクセスできるようにする等が考えられます（パブリッククラウド、最近でいうと OpenAI もそうですが API を使わせるのは立派なビジネスモデルですよね）。SBOMMについては、ちょっとイメージが湧きません。SBOMMのデータは機密性が高いでしょうから、直接データを売るのは難しそうです。しかしデータの販売だけがすべてではなく、これら統合管理の SI やコンサルは行えそうです。

# Chapter-14 現場を助けるツール

# 1: SBOM Editor Command

## 概要
**SBOM Editor Command** は、SBOMファイルの編集を行うコマンドラインツールです。

## CI/CDで済ませたい
生成した SBOM をそのまま使えれば良いですが、国内では厳しいかもしれません。日本では高い品質（SBOMに対しては完全性）が求められますし、デジタルの力で済ませる発想や投資もあまりしません――という点はすでに考察しました。つまり、ツール出力で足りてない部分は手作業で補うことが想定されます。

しかしながら、毎回手作業をしていては手間ですし、それこそ品質にばらつきが生じます。ならばせめて、どのように修正するかをレシピ化して、そのレシピのとおりにSBOMファイルを修正するプログラムがあれば良いです。CI/CD にも組み込めます。

そのためにはレシピの記述と、レシピを読み込んでSBOMを実際に編集するツールの二つが必要です。これが SBOM Editor Command です。

発想としては IaC と同じです。IaC も、従来インフラ作業は手作業で行っていたのを「レシピ化して」「そのとおりに解釈するプログラムに任せる」ことで自動化できるようにしたものです。そういう意味で、SBOM Editor Command は SBOM Edit As Code と呼ぶこともできます。略し方が迷いますね、SEaC でしょうか、いやわかりづらいですね。

## 所詮はJSONファイル
SBOM Editor Command の実装自体は難しくありません。所詮は JSON ファイル（他にも TagValue txt や YAML や XML もありますが本質的にはテキストフォーマットであり同じ）ですので、単にそれの所定部分を書き換える処理をつくるだけです。IaC のようにインフラ実環境に反映するから実環境とコードとの差分の対応が必要――みたいなこともありません。SBOM.json みたいなファイルをただ編集するだけです(※1)。

中身は SPDX や CycloneDX といった規格に基づいて書かれていますが、これらも規格を理解してパースすれば良いだけです。ただの設定フォーマットであり、言語的な複雑性はありませんので、プログラミング言語のような構文解析は要りません。

- ※
    - 1 一方で、もしソースコードとSBOMとの対応を厳格にする潮流が今後出てくるとしたら、IaC と同様、複雑なあり方になる可能性もあります。IaC は「インフラの実環境」と「インフラコード」の対応を取りますが、SEaC は「（そのSBOMが指す）ソースコード」と「SBOM」の対応を取るようになるイメージです。

# 2: SBOM Viewer

## 概要
**SBOM Viewer** は SBOM の中身を見やすく表示するツールです。コマンドラインもありえますが、どちらかといえば GUI として、非技術者向けに易しく表示するイメージです。

## SBOMは素人には読めない
SBOMは規格に基づいてJSONなどで書かれた機械可読用ファイルですから、人間が読むのは苦しいです。慣れたエンジニアであればエディタやIDE、あるいは本書でも少し紹介しましたが jq コマンドなどを用いて読み進めていけますが、そうではない素人の方には荷が重いです。

「SBOMはエンジニアが読むものじゃないのか？」と思われがちですが、国内の議論ではすでに監査部門だとか顧客への受け渡しとかいったシチュエーションが想定されています。開発だけが現場ではないのです。

というわけで、素人でも読めるような処置が必要かもしれません。そこで SBOM Viewer です。

## 一覧を表示するだけ
SBOM Viewer と書くと仰々しいかもしれませんが、単にコンポーネント一覧をわかりやすく表示するだけです。

## より踏み込むならグラフビューも
SBOM におけるソフトウェアの依存関係は階層的です。あるソフトウェアが A, B, C, D と 4 つのコンポーネントを使っていたとしても、ソフトウェア A はさらに別のコンポーネント X を使っています、そのソフトウェア X もさらに別のコンポーネントを――と、それこそネットワークのように依存が広がっています。そもそもオープンソースはそういう世界です。先人がつくりあげてきたソフトウェアに依存して新たなソフトウェアつくって、が積み重なっています。

理想論としてはこれら依存関係をすべて可視化することですが、それが[現実的でないことはすでに述べました](sbom_format#データフィールド)。しかし、これはあくまで現時点では、にすぎず、もしかしたら今でもやろうと思えばある程度は可能かもしれません。特にグラフ構造として視覚的に見せることができたら、見た目のインパクトもあって訴求面では強そうです。少し話が逸れますが、Obsidian などのノートツールも、ノート全体の接続関係をグラフビューで可視化できます。ノートは元来一覧で俯瞰するものでしたが、グラフというネットワークの構造で見せるという新しい世界を見せてくれたわけです。そうでなくとも、グラフはソーシャルグラフなど関係の可視化において有用性が示されています。一方で、ノードが多すぎると意味をなさなくなる本質も知られています（[毛玉問題](https://networkscience.wordpress.com/2016/06/22/no-hairball-the-graph-drawing-experiment/)）。

# Chapter-15 フォーマット

# 1: Server BOM

## 概要
**Server BOM** とは、あるサーバーがどんなソフトウェアを使っているか――たとえばインストールされているかを一覧化したものです。

ここでサーバーとは「OSを搭載した一台のコンピュータ」と捉えてください。私たちが普段使っているクライアント PC も含みますし、Web アプリや業務用途で動かしているサーバーも含みます。

## サーバーは SBOM では捉えられない
サーバーの構成管理は古典的に行われており、構成情報を採取・監視するセキュリティツールや社内システムが導入された企業も珍しくないでしょう。本書で言えば、[参考文献17](ref#17)で挙げてるタニウムも、まさにそのような製品を扱っています。主に従業員各々のPCを管理する文脈が強いです。

また昨今の SaaS 開発においても、物理サーバーにせよ、仮想マシンにせよ、クラウドのインスタンスにせよサーバーを立ち上げて利用する形態はよくあります。これらの構成管理はあまり注目されていない印象ですが、もし行うのであれば、同様にサーバーにどんなソフトウェアがインストールされているかを見ることになるでしょう。

SBOM が盛り上がってきたことで、これらもSBOMで捉えようとする動きを見かけるのですが、SBOM では扱えません。[すでに述べたとおり](what_is_sbom#sbom-と-xbom)、SBOM は「ソフトウェア」が「どんなソフトウェアを使っているか」を表現するものであって、「サーバー」が「どんなソフトウェアを使っているか」を表現するものでないからです。後者には後者用のフォーマットが必要であり、それが Server BOM です。

## Server BOM はどんな感じのものになる？

### どんな仕様？
Server BOM は筆者のアイデアであり、現状 SBOM のように具体的な規格が存在するわけではありません。

Server BOM として必要な情報は以下になると思います。

- コンポーネント情報
    - SBOM と同様です
- OS情報
    - OS の名前、OS のバージョン、アーキテクチャ、インストール日
- 環境情報
    - コンピュータ名
    - CPU、RAM、ディスクなどスペックに絡むもの
    - タイムゾーン、ロケール
    - ネットワークアダプター情報

管理を検討する際はいたずらに情報を持たせたくなりがちですが、SBOM と同様であればコンポーネント情報と「サーバー自体を識別する情報」程度で十分です。一方で、サーバーの管理をしている人達向けに付加価値をつけたいのであれば、もっと情報を拡充させる必要があるでしょう。

ちなみに、サーバー単体の情報しか扱っていないことに注意してください。サーバー同士の関係やシステム全体の目線で管理したいのなら、SaaSBOM や System BOM ともいうべき別の概念を使うべきです。

### コンポーネントはどうやって採取する？
OS のパッケージマネージャから取得できます。

Linux の場合、所定のディレクトリをスキャンすれば取得できます（Trivy もその意図でファイルシステムスキャンをサポートしています）。パッケージマネージャ用の設定ファイルを見ればいいだけですね。Windows の場合、レジストリに保存されていますのでレジストリの走査が必要です。レジストリの実態もファイルですが、ファイルから直接読み込むことはできないので、レジストリを読み込む処理を使うことになります。

ただし、これは「インストールされたソフトウェア」に限ります。インストールを経ずにどこかに配置しただけというケースもあり、これを検出するためにはディレクトリスキャンとファイルマッチングが必要でしょう。環境変数の値もヒントになるかもしれません。いずれにせよ、OS のパッケージマネージャを見るよりは難易度が上がるはずです。専用のマッチングアルゴリズムや対応パターンの蓄積が求められるからです。

### 規格化する？
筆者の好みで言えば、SBOM と同様、きちんと規格化して業界全体にガバナンスと啓蒙を効かせたいと考えます。一方で、企業単体でビジネスを行うのであれば、せいぜい製品内部の設計として持っておく程度でも済みます。

米国からスタートした SBOM の潮流は「ソフトウェアの」構成に注目しており、「サーバーの」構成は見ていません。CycloneDX は SaaSBOM などシステムのBOMというユースケースを想定してはいますが、サーバー単体のBOMは捉えていません。一方で、従業員のクライアントPCを管理する重要性（エンドポイントセキュリティ）は叫ばれていますし、国内では SI 文化があってサーバーの構築やメンテナンス自体がそれなりにビジネスになってもいます。規格化できるだけのボリュームやニーズはあると思うんですよね。

# 2: Parameter BOM
※SI の話になります。パラメーターシートについては[NTTデータのカンファレンス資料](https://www.slideshare.net/slideshow/infrastructure-as-code-2019-nttdata-sasaki-takai/177912142)がわかりやすいと思います。

## 概要
**Parameter BOM** は、SI におけるパラメータシートを SBOM の概念で捉えたもので、「サーバー」が「どんなパラメーターを使っているか」を一覧にしたものです。

名前がここまでの流儀と違いますがお許しください。ここまでの流儀に従えば「パラメーター」が「どんなソフトウェアを使っているか」と読めますが、Parameter BOM ではそうではなく、「サーバー」が「どんなパラメーターを使っているか」と定義しています。

## パラメーターのコンポーネント化
[コンポーネントマネジメントの項](idea_integrated_management#1%3A-コンポーネントマネジメント)でも扱いましたが、現場の担当者各々が必要な設定情報を自力で調べて記入するという無駄はあるあるです。そうではなく、コンポーネントという概念として各情報を定義して、それを全社的に使えるデータベースに置けば、全員ここから情報をひくだけで済みます。何なら自動で対応付けられるようにできれば手作業すら要らなくなります。それがコンポーネントマネジメントでした。

Parameter BOM の狙いも同じで、パラメーターのコンポーネント化です。パラメーターシートの作成は担当者が手作業で一から設計したり、過去の設計書からコピペして（特にテンプレート部分を）流用したりするものですが、コンポーネント化を実現できたら楽できます。データベースから使うコンポーネント（パラメーター）を選ぶだけで済みます。ソフトウェアの世界でもすでに一からつくるのではなく既存のライブラリ（つまりはソフトウェアコンポーネント）に頼るようになっていたり、そうでなくともローコードなど「部品を組み合わせる」で済む潮流も出てきています。筆者はパズルゲームとたとえますが、SI のパラメーターシートについても、同じくパズルゲーム化したいのです。そのためにはパズルで使うピース（コンポーネント）が必要で、ピースを整備していこうというわけです。

## 独自情報の記入は必要
パラメーターシートではパラメーターごとに設計根拠の記載が推奨されます。なぜその値にするのか、という根拠を書いておくのです。システムや顧客にはそれそれ事情がありますから、事情に合った値を設定するわけですが、その値の根拠が無いと後々メンテナンスで詰んでしまいます（よくて調べ直しですがコストがかかります）。ソフトウェア開発でも設計ドキュメントや「Why を説明したコメント」が無くて嘆く状況はありがちですが、同じですね。

さて、Parameter BOM にしたからといって設計根拠の必要性はなくなりません。つまり BOM の中に設計根拠という「人間の意図や判断」も書くことになります。これは SBOM とは明確に異なる性質です。SBOM はそのような情報は扱わず、同じ対象からは常に同じ一覧が生成されます。強いて言えば、SPDX のバージョン 2.x には（廃止されてますが）[Review Infomation](sbom_format#構造) があり、レビュー記録を書き込めるようになっていました。これと同じです。

# 3: Super BOM

## 概要
**Super BOM** とは、色々な SBOM フォーマットを出力するための上位の構造を指します。

## SPDX か CycloneDX で出せばいいんでしょ、とはならない可能性
理想を言えば SPDX なり CycloneDX なりがあるのですから、それらで出力すればそれでおしまいです。一方で、ここまで何度も述べてきたとおり、特に国内では（高い完全性を求められたり顧客側の事情に則ったりするときに）独自のフォーマットが要求される可能性があります。そうでなくとも現在 SBOM 生成ツールはSPDX と CycloneDX の「両方の」出力形式をサポートするのがセオリーとなっています。

要するに出力形式にバリエーションがあることを想定しておく必要があるのです。

## 上位の構造があれば変換は自在
色んな形式に対応したい場合、ある形式から別の形式に変換する、というアプローチは分が悪いです。たとえば from SPDX to CycloneDX、逆に from CycloneDX to SPDX の変換を考えた場合、変換元にない情報は変換先には置けません。SPDX は CycloneDX が持たない情報（ライセンス系）を持っていますし、CycloneDX もまた SPDX がメインの属性として扱っていないものをメインにしてます（CPE や PURL など識別子系）。同じ次元の構造を変換した場合、データの欠損が生じることがあるのです。

このような限界を越えるためには、これらの形式の上位に、より汎用的な構造を置く必要があります。これを Super BOM と名付けてみました。

Super BOM があれば、（持っている情報の範囲内では）どんな形式もつくれます。SPDX も CycloneDX も、その他の独自構造もです。形式の定義と生成の処理は実装が必要ですが、賢くつくれば拡張性はどうとでもできますし、本質的にはただのテキストデータをどうつくるかにすぎません。たかが知れています。

## protobom という例
Super BOM の試みはすでに実在します。

protobom です。

- 公式のGitHub
    - https://github.com/protobom/protobom
    - https://github.com/protobom/protobom/blob/main/api/sbom.proto
- ニュース
    - [「Protobom」でSBOMの作成、異なるフォーマットへの変換が容易に？　OpenSSFが発表：2大フォーマットの「SPDX」「CycloneDX」に対応　OSSとして公開 - ＠IT](https://atmarkit.itmedia.co.jp/ait/articles/2404/26/news081.html)
    - [米政府と OpenSSF がパートナーシップ：SBOM 管理のためのツール Protobom とは？ – IoT OT Security News](https://iototsecnews.jp/2024/04/17/us-government-and-openssf-partner-on-new-sbom-management-tool/)

## ただのデータベースでも良いかも
自社の都合のみを考えるのであれば、わざわざ Super BOM の規格化は要らないでしょう。自社内あるいは製品内に構成管理データベースをつくって、Super BOM レベルの情報を揃えればいいだけです。必要な構造はデータベース（をラップしたサービス）から出力させれば済みます。

こちらについてもすでに例はあります。たとえば Black Duck はスキャンをするとまず内部的な構成票をつくり、この構成票から HTML レポートなり csv エクスポートなり SBOM なりを出力します。この「内部的な構成票」が Super BOM にあたります（ただし SBOM のような規格化はされていないでしょう、もちろん設計はあるとは思いますが）。

# 4: Simple BOM

## 概要
**Simple BOM** とは、Narsion（名前とバージョン）とコンポーネントの識別子のみを並べたシンプルな SBOM フォーマットです。

## そんなに要る？
文脈として手作業による作成やメンテナンスを想定しています。

手作業用のフォーマットとして SPDX Lite がありますが、これはしばしば煩雑になりがちです。[OpenChain Japan Work Group](ref#19) が[サンプルを公開](https://github.com/OpenChain-Project/OpenChain-JWG/tree/master/subgroups/sbom-sg/outcomes/SPDX-Lite/sample)していますが、Excel で書くような「ザ・票」のテイストですよね。以下にサンプルのxlsxの列を引用します。

```
パッケージ名
パッケージSPDX識別子
パッケージバージョン
パッケージファイル名
パッケージダウンロード位置（入手先）
解析したファイル（手作業の場合false）
ホームページ
結論されたライセンス
宣言されたライセンス
ライセンスへのコメント
（特記事項等）"
著作権テキスト
パッケージに関する コメント
ライセンス識別子
抽出されたテキスト
ライセンス名
ライセンス コメント
```

たしかに SPDX には則っていますが、本当にこれだけの記載が必要なのでしょうか。

目的や文脈次第ですが、Name と、Version と、あとは脆弱性情報と照合する際に Name だけだと表記揺れの問題で苦労する可能性があるから CPE や PURL といった識別子くらいで十分とは考えられないでしょうか。この 3 つ程度なら手作業でも比較的マシです。ファイル形式についても、Excel のような仰々しいものはいらなくて、単に txt で事足ります。それ以外の情報については、この 3 つがわかっていれば後から引けます。

ちなみに SPDX 2.3 では、必須パラメーターは以下の 4 つだけです。

- Name（コンポーネント名）
- SPDXID（SBOMファイル中で各情報を一意に識別するための識別子、ファイル内で一意にできるなら自由）
- Download Location（コンポーネント入手先）
- Package CheckSum（コンポーネント自体のハッシュ値）

だからといってこれらを必ずサポートしているかというと、現状そうはなっていません。特に Package Checksum は上記サンプルにも書いてないですし、SBOM 生成ツールも出してないです。結局、今のところは、規格をベースにしつつも細かい部分はアレンジしても良いのではないでしょうか。もちろん今後はわかりません。法や要件などで厳密に定められた場合は、そのとおりにせざるをえなくなります。とはいえ法でそこまで細かく定めるとは考えづらいですし、要件についても本当に必要な構造をちゃんと決めればいいだけです。Simple BOM で済むケースもあると思うのです。

# 5: SBOP(Software Bills Of Pointer of material)

## 概要
**SBOP(Software Bills Of Pointer of material)** とは、「コンポーネントを指す識別子（ポインター）」のみを列挙した SBOM を指します。

## コンポーネント情報がわかれば何でもいい
Simple BOM はさらにシンプルにできます。そもそも SBOM は、究極的には何のコンポーネントを使っているかがわかればいいのです。だったらいっそ、コンポーネントの識別子だけを一覧にするのはどうでしょうか。CPE や PURL を使うこともできますし、ソフトウェアの数も天文学的というほどではないですから、それこそ以下のように、連番でもいいわけです。

```
10924
43789
117962
83945
9187
405
111219
...
```

人間が見ても理解できませんが、データベースに問い合わせればどのコンポーネントかは一意にわかります。逆を言えば、そのデータベースにあたるものは必要です。前の章で述べた[コンポーネントマネジメント](idea_integrated_management#1%3A-コンポーネントマネジメント)もその一つです。

## メタ情報をどこに入れるか
Simple BOM にも当てはまりますが、ここで議論したのはコンポーネント一覧の部分だけです。一方で SBOM には作成者や「そもそもこれは何のソフトウェアの一覧なのか」といったメタ情報も必要です。メタ情報はどこに入れるべきでしょうか。

大まかに 3 種類くらいあるでしょう。

- 1: 現在の規格のとおり、普通に SBOM の中に入れる
    - この場合、SBOP = メタ情報 + 識別子の一覧 になります
    - メタ情報もシンプルに絞れば「一行目にメタ情報を書く」くらいで事足りるでしょう
- 2: メタ情報も別ファイルにする
    - この場合、たとえば SBOP.txt と Meta.txt でワンセットになります
- 3: 製品に同梱する
    - 製品に SBOP ファイルを置いておけば、その製品の SBOM だと明示できます
    - ライセンスがそうですよね。LICENSE といったファイルで同梱していますが、LICENSE ファイル自体にメタ情報はありません（厳密に言えば著者名と期間はありますが）

最も無難なのは 1 ですね。

個人的には 3 の扱いが綺麗だと感じますが、グローバルなコンポーネントマネジメントが必要になりますから非現実的です。

2 は一見すると直感的ではないですが、[前の章で述べたように](idea_helper_tools#所詮はjsonファイル) SBOM を IaC のように捉えるとするならば価値がありそうです。要は SBOP.txt の方が IaC でいうコードに相当し、（IaC はコードからインフラをつくるように）SBOP.txt からプロジェクトをつくる、のような世界観を捉えられるからです。この IaC 的な世界観をつくるには「コンポーネント情報のみが記述されたもの」という概念が必要ですが、SBOP.txt はそのスタートラインになるでしょう。


# Chapter-16 DevPst

# DevPst
本章では DevPst なる概念を提唱します。実践を通じて体系化したものではなく、筆者が机上で考えたものです。

## 定義
**DevPst(DevPSIRT)** とは、開発者と PSIRT が積極的に連携するあり方を指す概念です。

名前は DevOps から取っています。開発者（Dev）と運用者（Ops）は元々役割が分かれており、開発者がつくったものを運用者に引き継ぐという分業体制でしたが、これだと色々不都合があったのです。開発は運用のことを考えて開発し、運用もまた開発の事情を考慮し、とお互いに歩み寄った方が全体としてはうまくいきます。DevOps は今ではよく知られ、取り組まれている概念だと思います。

この考え方を開発者（Dev）と PSIRT（Pst）にも当てはめようじゃないか、というわけです。DevPSIRTだとおさまりが悪いので、略して DevPst としています。

## Developer と PSIRTer
便宜上の用語を二つ定めます。

DevPst における開発者を **開発者(Developer)** と呼びます。PSIRT に属する者を **PSIRTer** と呼びます。

## 目的とアプローチ

### 目的
脆弱性対応のリードタイムを削減することです。

### アプローチ
そのために SBOM と、そこから盛り上がっている[オーバーオールセキュリティ](sbom_and_overallsecurity#オーバーオールセキュリティ)を活用します。

ただし開発者単体や PSIRTer 単体では力不足になりがちなので、お互いが歩み寄って知見を共有し協力します。開発者は自動化を行ったり、中長期的に楽をしたり競争力に繋げたりするための仕組みを考えたり（もちろんつくることも）するのに強いです。PSIRTer は脆弱性を含むセキュリティの知見が豊富です。開発者は後者を知りませんし、PSIRTer も前者を知りません。お互いが持ち寄ることで補強できます。

## 想定組織
チームあるいは部門として PSIRT がすでに存在しており、ソフトウェア開発が生業の一つとなっている、中規模・大規模な組織を想定しています。

なので、たとえば以下は想定外です。

- 中規模・大規模であるが、ソフトウェア開発を生業としていない組織
    - SBOM が想定している「ソフトウェア開発」をしていない場合、業界が貯めてきた知見を生かせません
    - おそらく PSIRT も存在していないのではと思います
- ソフトウェア開発をしているが、小規模な組織
    - おそらく PSIRT が存在しないか、詳しい担当者が数人いるか、あるいは開発者達が兼任して対応しているかになっていると思います

## 連携の戦略
DevPst では下記の 3 つの戦略を取ります。

- ミックスイン
    - 1: 開発チーム内に PSIRTer 寄りの役割を配置します
    - 2: PSIRT に開発者寄りの役割を配置します
    - チームのあり方としてミックスインが当たり前になる、あるいは選択肢の一つとして浮上することを目指します
- 統合管理の実現
    - [すでに述べたとおり](idea_integrated_management) SBOM、脆弱性、その他オーバーオールセキュリティの運用には全社的な基盤があると便利です
    - このような基盤をつくるための投資を行います
    - 内製を行える開発者と、セキュリティ――特に脆弱性の知見を持つ PSIRTer はどちらも必要です
- インナーリスト
    - 社内全体から相談を受けたり、社内全体への啓蒙を行ったりする専任者を設けます（社内で立ち回るため Inner）
    - 専任者は Dev も Psv もどちらの知見も持つ、高度な人材です
    - 役割(-list)として表面的に広く啓蒙や相談を担うエバンジェリストと、必要に応じてプロジェクトやメンバーに深く入り込んで支援するスペシャリストがあります

## 初動
以下のとおり、まずはファーストステップを行って感触を見ます。

- 1: 投資
    - DX と同様、予算や権限を握る経営者が「やる」と決めて、投資に踏み切ります
    - 特に PSIRT 側は体制や予算が最小限であり、人員に余裕がないことが多いため明示的な確保が必要です
- 2: ファーストステップの開始
    - 試行する戦略とメンバーを決めます
    - 少人数で具体的な活動を行う場合は、メンバーは専任が望ましいです
    - 大人数で広く議論したりボランティアベースで進めたりする場合でも、旗振り役や積極的な推進者が欲しいところです（この人も専任が望ましい）
- 3: ファーストステップの完了とフィードバック
    - ファーストステップを通じて組織内の現状や肌感覚を集めます
    - 意見の収束は少人数で、ファーストステップのメンバーが行ないます
    - この後の本格的な活動をどうするかも決めます
        - 大々的にチームを立ち上げるのもアリですし、ファーストステップレベルの小さな取り組みをもう一度繰り返す（セカンドステップ）のもアリです

## おわりに
DevPst という概念についてかんたんに提唱してみました。

もちろんやり方は一つではなく、これらの戦略や初動はあくまで筆者の案にすぎませんが、本質は Dev と PSIRT がお互いに歩み寄って補強することです。そうすることで、よりフレキシブルでアジリティの高い組織になると思うのです。

# Chapter-17 📘付録1 本書のGPTs

# [ChatGPT - SBOMを噛み砕くGPTs](https://chatgpt.com/g/g-0sQwq3gMu-sbomwonie-misui-kugpts)
試験的に、本書を食わせた ChatGPT をつくってみました。

GPTs によって実装されており、ChatGPT Plus 利用者が利用できます。ChatGPT 無料ユーザーにも最近解禁されましたが、Limit は厳しいようです。

## 更新履歴:
- 2024/05/31 v0.1
    - とりあえず公開
    - 品質: 低
    - 感想: 書籍の内容をまともに読んでくれていない感じ、要調整


# Chapter-18 📘参考情報

# 経済産業省

## 1
[「ソフトウェア管理に向けたSBOM（Software Bill of Materials）の導入に関する手引」を策定しました （METI/経済産業省）](https://www.meti.go.jp/press/2023/07/20230728004/20230728004.html)

手引の Ver1.0 です。

## 2
[サイバー攻撃への備えを！「SBOM」（ソフトウェア部品構成表）を活用してソフトウェアの脆弱性を管理する具体的手法についての改訂手引（案）を公表します （METI/経済産業省）](https://www.meti.go.jp/press/2024/04/20240426001/20240426001.html)

手引の Ver2.0 ですが、2024/04/26 時点では案です。

## 3
[OSS の利活⽤及びそのセキュリティ確保に向けた管理⼿法に関する事例集](https://www.meti.go.jp/policy/netsecurity/wg1/ossjirei_20220801.pdf)

## 4
https://github.com/OpenChain-Project/OpenChain-JWG/tree/master/subgroups/sbom-sg/outcomes/SBOM/SBOM_Tools_Document

OpenChain Project Japan Work Group の SBOM サブグループによる、ツールやドキュメントの一覧です。

# 大手SIerのブログ記事

## 5
[「初めての人のためのSBOM入門セミナー」でSBOMに関する講演をおこないました｜OSS管理ブログ｜ソフトウェア部品管理ソリューション｜日立ソリューションズ](https://www.hitachi-solutions.co.jp/sbom/sp/blog/2023020103/)

日立ソリューションズの記事です。

## 6
[SBOMが解決する課題と関連資料の紹介 : NECセキュリティブログ | NEC](https://jpn.nec.com/cybersecurity/blog/230414/index.html)

[第35回 「SBOM」とは | サイバーセキュリティ | NECソリューションイノベータ](https://www.nec-solutioninnovators.co.jp/ss/insider/column35.html)

NECとNECソリューションイノベータの記事です。

## 7
[全世界待望！SBOMによるサプライチェーンセキュリティ強化 | DATA INSIGHT | NTTデータ - NTT DATA](https://www.nttdata.com/jp/ja/trends/data-insight/2023/0207/)

NTTデータの記事です。

# プラットフォーマーのWebページ

## 8
[Google Developers Japan: SBOM in Action: 「ソフトウェア部品表」で脆弱性を見つける](https://developers-jp.googleblog.com/2022/08/sbom-in-action.html)

Googleの記事です。

## 9
[Windows のセキュリティ基盤 - Windows Security | Microsoft Learn](https://learn.microsoft.com/ja-jp/windows/security/security-foundations/)

Microsoftのドキュメントです。サプライチェーンの一要素として SBOM が一言紹介されています。

## 10
[セルフサービスのSBOMが登場 - GitHubブログ](https://github.blog/jp/2023-04-06-introducing-self-service-sboms/)

[リポジトリのソフトウェア部品表のエクスポート - GitHub Docs](https://docs.github.com/ja/code-security/supply-chain-security/understanding-your-software-supply-chain/exporting-a-software-bill-of-materials-for-your-repository)

GitHubもSBOM出力機能をサポートしています。

## 11
[Software Bill of Materials (SBoM) - Practicing Continuous Integration and Continuous Delivery on AWS](https://docs.aws.amazon.com/ja_jp/whitepapers/latest/practicing-continuous-integration-continuous-delivery/software-bill-of-materials-sbom.html)

AWSのドキュメントです。

# 規格

## 12
[SPDX – Linux Foundation Projects Site](https://spdx.dev/)

## 13
[OWASP CycloneDX Software Bill of Materials (SBOM) Standard](https://cyclonedx.org/)

## 14
[Specifications – SPDX](https://spdx.dev/use/specifications/)

SPDXの仕様ドキュメント。オンラインドキュメントとして整備されています。

## 15
[CycloneDX Specification Overview](https://cyclonedx.org/specification/overview/)

https://github.com/CycloneDX/specification/blob/1.6/schema/bom-1.6.proto

CycloneDXの仕様ドキュメント。GitHub 上に JSON や Protocol Buffer の形で定義されています。

# その他

## 16
[プレスリリース：『2023 ネットワークセキュリティビジネス調査総覧 市場編／ベンダー戦略編』まとまる（2023/12/27発表 第23140号）](https://www.fcr.co.jp/pr/23140.htm)

## 17
[タニウムが国内のSBOM市場調査の結果を発表 | タニウム合同会社のプレスリリース](https://prtimes.jp/main/html/rd/p/000000025.000089232.html)

## 18
> Thanks for your feedback. However, we have decided to focus on SPDX format for the SBOM generation, and as Aasim mentioned you can extend our tool to generate a CycloneDX format SBOM. But please feel free to leverage the component detector to build a CycloneDX format SBOM generator yourself.

https://github.com/microsoft/sbom-tool/issues/210

## 19
[SPDX Lite | OpenChain Japan Work Group (JWG)](https://openchain-project.github.io/OpenChain-JWG/subgroups/licensing/SPDX_Lite.html)

## 20
[導入進めるトヨタ　供給網で統一書式 | 日経クロステック（xTECH）](https://xtech.nikkei.com/atcl/nxt/mag/nc/18/040800414/040800002/)

トヨタの事例は[経産省の事例集](#3)でも紹介されています。

## 21
[SBOMインポート機能をリリース | yamory Blog](https://yamory.io/blog/start-sbom-import/)

## 22
The Minimum Elements For a Software Bill of Materials (SBOM): https://www.ntia.doc.gov/files/ntia/publications/sbom_minimum_elements_report.pdf

## 23
[「SBOM」の認知率は41.3％、導入率は33.1％--ビジョナル調査 - ZDNET Japan](https://japan.zdnet.com/article/35212809/)

# ツール

## in-toto
[in-toto | A framework to secure the integrity of software supply chains](https://in-toto.io/)

## SBOMit
[SBOMit](https://sbomit.dev/)

## yamory
[yamory | 脆弱性管理クラウド](https://yamory.io/)

## Snyk
[【公式】Snyk株式会社のウェブサイトです](https://go.snyk.io/jp.html)

## Black Duck
[Black Duckソフトウェア・コンポジション解析（SCA） | Synopsys](https://www.synopsys.com/ja-jp/software-integrity/software-composition-analysis-tools/black-duck-sca.html)

## jq
https://jqlang.github.io/jq/

# Chapter-19 📘参考情報2

SBOM に関するビジネスを検討する際に使えそうな情報。筆者の備忘含む。

# 分類や段階

## CISA, 6分類
https://www.cisa.gov/sites/default/files/2023-04/sbom-types-document-508c.pdf

- design
- source
- build
- analyzed
- deployed
- runtime

## yamory, SBOMの活用段階
https://www.jnsa.org/seminar/2023/iot/data/Session3.pdf

- 1: 手動でSBOMを作成
- 2: SBOMを機械的に生成
- 3: SBOMをDB化、リスク照合も⾃動化、リスク管理
- 4: BOMもインポート管理できる

## OWASP, BOM成熟モデル
- [BOM Maturity Model - SCVS](https://scvs.owasp.org/bom-maturity-model/)
- https://github.com/OWASP/Software-Component-Verification-Standard/blob/BOM_Maturity_Model/BOM_Maturity_Model/1.0.0-rc.1/bom-maturity-model-1.0.0-rc.1.json

.

- 難易度は3段階
    - 3: 高入手困難。手作業、人による観察、または制限されたドメイン内に保持されているデータへのアクセスが必要な場合
    - 2: 中程度。ドメイン固有のツールやプラグインを使用した自動観察によって取得するのが中程度に困難
    - 1: 低。既存のツールを使用した直接の自動観察により取得が簡単
- サポートレベルは6段階
    - 0 サポートされてない
    - 1 データフィールドとフィールド名がないためいちいちパースが必要
    - 2 フィールドはサポートしてないがフィールド名と値を個別に格納する程度はできている
    - 3 データフィールドをサポートする拡張機能を開発している
    - 4 公式拡張機能？を通じて仕様をサポートしている
    - 5 仕様はデータフィールドをネイティブにサポートしている

# パッケージマネージャの仕様
`npm spec` のように `spec` を入れて検索するか、GitHub のスキーマ定義系のファイルを漁るなどすれば出てきます。馴染みない言語であたりをつけられない場合は ChatGPT などに聞いてあたりをつけます。

## NPM
[package.json | npm Docs](https://docs.npmjs.com/cli/v9/configuring-npm/package-json)

## Yarn
[Manifest (package.json) | Yarn](https://yarnpkg.com/configuration/manifest)

## Pip
- [Requirement Specifiers - pip documentation v24.0](https://pip.pypa.io/en/stable/reference/requirement-specifiers/)
- [PEP 508 – Dependency specification for Python Software Packages | peps.python.org](https://peps.python.org/pep-0508/)

## Poetry
[pyproject.toml ファイル - Poetry documentation (ver. 1.1.6 日本語訳)](https://cocoatomo.github.io/poetry-ja/pyproject/)

## RubyGems
[Specification Reference - RubyGems Guides](https://guides.rubygems.org/specification-reference/)

## go.mod
[go.mod file reference - The Go Programming Language](https://go.dev/doc/modules/gomod-ref)

## Cargo
[The Manifest Format - The Cargo Book](https://doc.rust-lang.org/cargo/reference/manifest.html)

## Nuget
[NuGet の .nuspec ファイル リファレンス | Microsoft Learn](https://learn.microsoft.com/ja-jp/nuget/reference/nuspec)

## winget
[winget-cli/schemas/JSON/manifests/v1.7.0/manifest.singleton.1.7.0.json at master · microsoft/winget-cli](https://github.com/microsoft/winget-cli/blob/master/schemas/JSON/manifests/v1.7.0/manifest.singleton.1.7.0.json)

## Scoop
[Scoop/schema.json at master · ScoopInstaller/Scoop](https://github.com/ScoopInstaller/Scoop/blob/master/schema.json)

## RPM
[第3章 ソフトウェアのパッケージ化 Red Hat Enterprise Linux 7 | Red Hat Customer Portal](https://access.redhat.com/documentation/ja-jp/red_hat_enterprise_linux/7/html/rpm_packaging_guide/packaging-software)

# オーバーオールセキュリティ

## CNCF, Secure Software Factory
https://github.com/cncf/tag-security/blob/main/supply-chain-security/secure-software-factory/secure-software-factory.md


## CNCF, Software Supply Chain Best Practices
https://github.com/cncf/tag-security/blob/main/supply-chain-security/supply-chain-security-paper/sscsp.md

## Linux Foundation, SLSA(Supply-chain Levels for Software Artifacts)
https://slsa.dev/

# SCA

## Black Duck
[Black Duckにようこそ 2024.1.0](https://sig-product-docs.synopsys.com/ja-JP/bundle/bd-hub/page/Welcome.html)

## Black Duck スキャナー
- Synopsys Detect(Detector Scan)
    - 設定値: [All Properties](https://sig-product-docs.synopsys.com/ja-JP/bundle/integrations-detect/page/properties/all-properties.html)
    - 実装: https://github.com/blackducksoftware/synopsys-detect/tree/master/detectable/src/main/java/com/synopsys/integration/detectable/detectables
    - 実装(ドキュメント): https://github.com/blackducksoftware/synopsys-detect/tree/master/documentation/src/main/markdown/packagemgrs
- 署名スキャン(Signature Scan)
    - [署名スキャナコマンドラインの使用によるコンポーネントスキャンの実行](https://sig-product-docs.synopsys.com/ja-JP/bundle/bd-hub/page/ComponentDiscovery/CommandLine.html)

# カンファレンス

## FOSDEM2024
[FOSDEM 2024 - Software Bill of Materials devroom](https://fosdem.org/2024/schedule/track/software-bill-of-materials/)

# BOM規格

## protobom
- https://github.com/protobom/protobom
- https://github.com/protobom/protobom/blob/main/api/sbom.proto

## KBOM(Kubernetes Bill of Materials)
https://github.com/ksoclabs/kbom

## HBOM by CISA
https://www.cisa.gov/resources-tools/resources/hardware-bill-materials-hbom-framework-supply-chain-risk-management


