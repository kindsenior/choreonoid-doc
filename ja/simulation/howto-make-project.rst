
シミュレーションプロジェクトの作成
==================================

.. sectionauthor:: 中岡 慎一郎 <s.nakaoka@aist.go.jp>

本ページでは、コレオノイド上でシミュレーションを行うための手順について説明します。

.. contents:: 目次
   :local:


プロジェクトについて
--------------------

コレオノイドでは、ユーザの操作対象となるデータの多くは「アイテム」という統一された形態で管理されます。
アイテムは「アイテムビュー」上に表示される個々の項目で、それらはツリー形式で管理されます。
これを「アイテムツリー」と呼びます。

アイテムツリー上には、多様な種類のアイテムを複数読み込んで、同時に扱うことができます。
逆に、例えばシミュレーションを行うためには、シミュレーション対象となるモデルであったり、制御プログラムであったり、シミュレーション結果の動作データといった、いくつかのデータを同時に扱う必要があります。
それらのデータをアイテムとして実装しアイテムツリー上に読み込むことで、複数データの統一的な管理・操作を行うのが、コレオノイドの基本になります。

そして、いくつかのアイテムが読み込まれたアイテムツリーの状態を、コレオノイドでは「プロジェクト」と呼びます。プロジェクトは「プロジェクトファイル」としてファイルに保存することができます。
プロジェクトファイルには、アイテムの状態に加えて、ビューやツールバー、その他各種機能に関するコレオノイド全体の設定を同時に保存されます。
このプロジェクトファイルを後ほど読み込むことで、プロジェクトの状態を復帰させることができます。

:doc:`samples` は、どれも「プロジェクト」として保存されたものになっていますので、それらを読み込むことで、プロジェクトがどういうものか概要を知ることができるかと思います。

以下では、シミュレーションを行うために、アイテムツリーをどのような状態に構築していけばいいのか、すなわちどのようなプロジェクトを構築していけばいいのかについて、 :doc:`samples` のひとつである "SR1Walk.cnoid" の構築手順を通して説明します。


空のプロジェクトの用意
----------------------

まず、コレオノイド上でプロジェクトを空の状態にします。実はコレオノイド上では同時に複数のシミュレーションを扱うことが出来ますので、必ずしも空にしておく必要はないのですが、ここでは説明を分かりやすくするため、空の状態から新しくプロジェクトを構築していくことにします。

と言っても、単にコレオノイドを起動しただけの状態で、プロジェクトは空になっていますので、この状態から始めてもらえればOKです。

.. image:: images/project_empty.png

既にプロジェクト（なんらかのアイテム）が読み込まれている場合でも、アイテムについて右クリックすると出るメニューで「カット」を選択することで、プロジェクトを空にすることも出来ます。ただし、プロジェクトがプロジェクトファイルから読み込まれている場合には、プロジェクト上書き保存の操作を行うことで対象ファイルの内容も変わってしまいますので、注意が必要です。これについては、あらかじめ別のファイル名で「名前を付けてプロジェクトファイルを保存」をしておけば問題ありません。


ワールドアイテムの作成
----------------------

まず、アイテムツリーのルートとして、「ワールドアイテム」を作成する必要があります。メインメニューの「ファイル」-「新規」から「ワールド」を選択してください。出てくるダイアログで適当な名前を指定して（デフォルトの名前でも結構です）「生成」ボタンを押すと、ワールドアイテムが生成されます。

.. image:: images/world.png

ワールドアイテムは、コレオノイド上でひとつの仮想世界を表すオブジェクトとなっており、仮想世界に含まれるモデルや、仮想世界と関連付けられる機能については、基本的にワールドアイテムの小アイテムとしてツリー上に読み込むことになります。
複数のワールドアイテムを用意して、それぞれに関連するアイテムを小アイテムとして格納することで、複数の仮想世界を同時に扱うことも可能です。


ロボットモデルの読み込み
------------------------

次にシミュレーション対象のロボットモデルを読み込みましょう。
まず、アイテムビュー上で先程作成したワールドアイテムを選択状態にします。
この状態でアイテムの読み込み操作を行うと、読み込まれたアイテムがワールドアイテムの小アイテムとして追加されることになります。（何も選択されていない状態で読み込み操作を行うと、ツリーのルートに追加されることになります。この状態からアイテムをワールドアイテムへドラッグして小アイテムとすることも可能です。）


モデルの読み込みは、メインメニューの「ファイル」-「読み込み」-「OpenHRPモデルファイル」から行います。このメニューを実行するとファイル読み込みのダイアログが表示されますので、読み込みたいモデルファイルを選択してください。

ここではサンプルロボットに対応する "SR1.yaml" を読み込むことにします。
このファイルはコレオノイドのソースまたはインストール先の share ディレクトリ（環境によっては、share/choreonoid-x.x) 以下にある "model/SR1" というサブディレクトリに格納されていますので、そこから"SR1.yaml"を選択してください。なお、"SR1.wrl" というファイルもあり、そちらはOpenHRP3のフォーマットで書かれたメインのモデルファイルとなっていて、そちらのファイルを読み込むことも可能です。これに対して "SR1.yaml" のファイルは、コレオノイドで利用する付加的な情報が書かれたファイルになっており、これを読み込むと同時に"SR1.wrl"の内容も読み込まれることになります。今回はどちらを読み込んでもOKですが、基本的に .yaml のファイルも用意されている場合は、そちらを読み込むようにします。


ファイルの読み込みに成功すると、ワールドアイテムの小アイテムとして、"SampleRobot"という名前でアイテムが追加されたかと思います。これがモデルに対応するアイテムで、アイテムの型は "BodyItem" となっています。

.. image:: images/load_model.png

.. "SampleRobot" は、モデルファイルを修正して、"SR1"となるようにしたい。

読み込んだだけでは、シーンビュー(3Dグラフィックのビュー)上にはモデルは表示されません。ここで、"SampleRobot"アイテムの左端のチェックボックスをクリックしてチェックを入れてください。これを行うことで、シーンビュー上でもサンプルロボットのモデルが表示されるようになります。

.. image:: images/load_model2.png

環境モデルの読み込み
--------------------

ロボットのモデルを読み込みましたが、このモデルだけを対象に動力学シミュレーションを行うと、ロボットが重力によってどこまでも落ちて行ってしまいます。ここではロボットを床の上で歩かせるシミュレーションを行いたいので、床のモデルも読み込むことが必要です。

先ほどのロボットモデルの読み込みと同様に、ワールドアイテムを選択状態にして、床のモデルを小アイテムとして読み込みます。床のモデルは share ディレクトリ以下の "model/misc" に "floor.wrl" というファイルがありますので、これを読み込んでください。（床に関しては .yaml のファイルは用意していませんので、こちらを読み込んでください。）

.. image:: images/load_floor.png

読み込みに成功したら、"Floor"と表示されている読み込んだアイテムについて、先ほどと同様にチェックを入れることで、シーンビュー上に青い板のような床のモデルが表示されるかと思います。なお、シーンビュー上での表示は行わなくても、モデル自体はシミュレーションにおいて使われることになりますので、このような床のモデル等については、必ずしも表示をオンにしなくてもOKです。例えば、シーンビュー上ではデフォルトで床に相当する位置にグリッド線が表示されますので、表示上はそれで十分かもしれません。逆にモデル表示をオンにしていると、シーンビュー上でモデルをドラッグする際に操作対象となってしまい、ロボットの操作に集中したい場合には煩わしいこともありますので、そのような場合には表示をオフにするのも有りです。

.. image:: images/load_floor2.png

このようにして、シミュレーションで必要なモデルを読み込んでいきます。必要なモデルはいくらでも読み込むことができます。
今回のシミュレーションでは、ロボットと床に関する以上の２つのモデルを読みこめばOKです。


干渉検出設定について
--------------------

シミュレーションでは各物体間の干渉検出を行うことが必要ですが、これについては、シミュレータの方で自動的にモデル間の干渉を検出するようになっています。より正確に言うと、異なるモデル間の全てのリンクのペアを検出対象として扱うようになっています。一方で、各モデルの自己干渉については今のところ検出しません。このあたりは、干渉検出にかかるコストと必要な干渉検出との兼ね合いで、干渉検出対象をどのようにするかの設定を行えることが望ましいため、今後の改良でそのような設定も可能としたいと思います。

以上はシミュレーションにおける干渉検出に当てはまりますが、一方でコレオノイドではシミュレーションとは独立して、現在のモデルの状態について干渉検出を行い、干渉があればその結果をシーンビュー上でビジュアライズする機能も持っています。ここではこの機能について簡単に説明します。

まず、モデル間の干渉検出を行うには、ワールドアイテムのプロパティのひとつである「干渉検出」を"true"にしてください。プロパティの表示と編集は「プロパティビュー」上で行います。どれかひとつのアイテムを選択状態にしておくと、プロパティビューにそのアイテムのプロパティ一覧が表示されますので、ここではワールドアイテムのみを選択状態にします。そして「干渉検出」の項目の右側でダブルクリックを行うことにより、"true"と"false"を切り替えることが可能です。

.. image:: images/conflict_true.png

「干渉検出」を"true"にすることにより、干渉検出処理が内部的に行われるようになりますが、これだけではまだ結果を見ることができません。結果をみるためには、まずワールドアイテムについてもアイテムビュー上のチェック状態をオンにします。

その上で「シーンツールバー」の「干渉線の表示」ボタンをオンにしておくと、干渉が生じている点に緑の線が表示されるようになります。

.. image:: images/conflict_toolbar.png

また、「運動学バー」上の「干渉リンクのハイライト表示」をオンにしておくと、干渉が生じているリンクを黄色い線でアウトライン表示するようになります。

.. image:: images/conflict_toolbar2.png


各モデルの自己干渉については、モデルに対応する"BodyItem"のプロパティで検出のオン・オフを切り替えます。
モデルのアイテムを選択状態にするとプロパティビュー上に「自己干渉」というプロパティが表示されますので、これを"true"にしてください。これで自己干渉についても同様に、干渉検出が行われるようになります。また結果の表示については、上記の設定が同様に適用されます。

.. image:: images/conflict_true2.png

以上のようにして干渉検出のビジュアライズを行うことができますが、対象モデルやご利用の環境によっては干渉検出が重くなり、コレオノイド全体の動作も重くなってしまうことがあります。そのような場合には、必要でなければ干渉検出をオフにしておく方がよいかもしれません。

今回のモデルで干渉検出を行った例は以下のようになります。

.. image:: images/conflict_example.png


初期位置・姿勢の設定
--------------------

各モデルについて、シミュレーション開始時の位置・姿勢を設定します。
まずコレオノイドの位置・姿勢編集機能を使って、各モデルの位置姿勢を望みの状態にしてください。
望みの状態にセットできたら、ワールドアイテムを選択状態にして、シミュレーションツールバーの初期状態設定ボタン("Store body positions to the initial world state")を押します。

.. image:: images/init_set_button.png

あるいは、モデルのアイテムを選択状態にしてこのボタンを押すと、選択しているモデルのみ初期状態の更新を行います。
うまく初期状態の更新が行えたかどうかについては、メッセージビューに表示されるメッセージで確認してください。

.. image:: images/init_set_message.png

初期状態がうまく設定できていれば、シミュレーションツールバーの初期状態呼び出しボタン("Restore body positions from the initial world state")を押すことで、モデルの現在の状態が初期状態になります。通常、この状態がシミュレーション開始時の状態として使われることになります。

.. image:: images/init_call_button.png

.. 上記のボタンについて翻訳が抜けていたので、翻訳をつけたマイナーアップデート後に書きなおす。

ロボットの制御プログラム（コントローラ）は通常どのような常態から開始しても対応可能なように設計することが望ましいのですが、特定の初期状態から制御を開始しないとうまく動かないものもあり得ます。SR1Walkサンプルのコントローラは決められた関節角軌道を単に再生するだけのものですので、ロボットの初期状態は時刻０の関節角と一致させる必要があります。


コントローラアイテムの作成
--------------------------

ロボットを制御するためのプログラム（コントローラ）を用いる場合は、その設定を行います。

まず、コントローラには実装方式・接続方式などに関して様々な形式のものがあり得ます。
コレオノイドではベースとなる「コントローラアイテム(ControllerItem)」をプラグインによって拡張できるようになっており、これによって様々な形式のコントローラに対応することが可能です。現在のところは、コレオノイドの独自形式である「シンプルコントローラ(SimpleController)」の形式と、CORBA通信を用いるOpenHRP3の形式である「OpenHRP3.xコントローラ」を利用することが可能です。OpenHRP3形式を用いる場合は、コレオノイドのビルド時にOpenHRPプラグインもビルドするようにしておいてください。

SR1Walkのサンプルでは、シンプルコントローラ形式のコントローラを用意しています。
このコントローラとの接続設定を行うには、シンプルコントローラアイテムをロボットモデルの小アイテムとして用意し、アイテムのプロパティで必要な設定をしておきます。

まず、SampleRobotのアイテムを選択状態にした上で、メインメニューの「ファイル」-「新規」-「シンプルコントローラ」を選択してください。表示されるダイアログで、適当に名前を設定します。これについては、実際のコントローラの名前を反映した名前にしておくとよいでしょう。生成ボタンを押すと、ロボットモデルの小アイテムとしてシンプルコントローラアイテムが生成されます。

.. image:: images/add_controller.png

次に、コントローラアイテムを選択状態にして、プロパティビュー上で「コントローラDLL」の項目に対応するプログラムを記述します。SR1Walk用に用意されたコントローラは"SR1WalkPatternController"という名前になりますので、これを記述してください。なお、実際のファイルはこの名前に .so, .dll, .dylib といった拡張子がついた、ダイナミックリンクライブラリ（共有ライブラリ）形式のファイルで、コレオノイドインストール先の lib/choreonoid-x.x/simplecontroller ディレクトリ以下に入っています。このディレクトリに入っていれば、プログラム名のみで指定を行うことが可能です。そうでない場合は、絶対パスで記述することにより指定できます。いずれの場合も、拡張子については省略することが可能です。プロジェクトファイルのOS間の可搬性を確保するため、通常は拡張子を指定しません。

.. image:: images/controller_property.png

以上の設定により、サンプルロボットモデルに コントローラ "SR1WalkPatternController" を関連付けることが出来ました。これで、シミュレーションの際にはこのコントローラによってロボットの制御が行われることになります。

なお、他の形式のコントローラを用いる場合でも、対応するコントローラアイテムを制御対象モデルの小アイテムとして生成し、必要なプロパティ設定を行う、という手順は同様になります。


シミュレータアイテムの作成
--------------------------

最後に、シミュレーションの実際の計算処理を行うエンジンである「シミュレータアイテム」を用意し、ワールドアイテムと関連付けておく必要があります。

シミュレータアイテムについても、そのベース部分をプラグインで拡張することで様々なエンジンを利用可能となるように設計されており、実際のエンジンに対応する様々なシミュレータアイテムを実装可能です。
現在のところコレオノイド本体では、我々が開発したエンジンを組み込んだ「AISTシミュレータアイテム」と、オープンソースの動力学エンジンで広く使われているもののひとつである"Open Dynamics Engine" を組み込んだ「ODEシミュレータアイテム」が利用可能となっています。（ODEシミュレータについては、ODEプラグインをビルドしておく必要があります。）

ここでは、AISTシミュレータを使うことにしましょう。シミュレータアイテムについても、ワールドアイテムの小アイテムとして生成することで、シミュレーション対象のワールドとの関連付けを行います。そこで、まずワールドアイテムを選択状態にし、メインメニューの「ファイル」-「新規」-「AISTシミュレータ」を実行して、アイテムの生成を行います。

.. image:: images/simulator_item.png

シミュレータアイテムについても、その設定は基本的にプロパティビュー上で行うことができます。
ここではまず「記録モード」を「末尾」にして、「時間長」に「13.4」を指定します。
SR1WalkPatternControllerは13.4秒分のパターンを再生するだけのコントローラで、それ以上動作を続けると挙動がおかしくなりますので、こうしておく必要があります。もちろん、一般的にはどれだけ制御を続けても正常に動くようにコントローラを設計するのが望ましいです。

.. image:: images/simulator_property.png

ここでは説明しませんが、他のプロパティを調整することで、シミュレーションに関わる様々な挙動を調整することが可能です。ここでは残りは全てデフォルトにしておくことにします。

このようにシミュレータアイテムを生成してプロパティの設定を行うのですが、シミュレータアイテムはひとつのワールドアイテムに対していくら生成してもかまいません。複数のシミュレータアイテムを用意しておくことで、異なるシミュレータや異なるプロパティ設定によるシミュレーション結果の比較などを、より効率的に行うことが可能です。


タイムステップの設定
--------------------

シミュレーションにおけるタイムステップ（時間刻み幅）は、コレオノイド全体の時間を管理する「タイムバー」上で設定します。
タイムバーの右端の設定ボタンを押すと出てくる設定ダイアログにて表示される「内部フレームレート」が、シミュレーションのタイムステップも決定するパラメータとなっています。今回のサンプルではコントローラがタイムステップ 2 [ms] = 500 [fps] を想定していますので、「内部フレームレート」にも「500」を指定しておくことが必要です。

.. image:: images/timebar.png

このようにコントローラの対応にも合わせる必要が出てきますが、一般的にはタイムステップを細かくする（＝フレームレートを上げる）ことで、よりシミュレーションが安定かつ高精度になります。一方でシミュレーション速度は遅くなってしまうので、両者のトレードオフを考慮しながら適切な値を設定することが重要です。


プロジェクトの保存
------------------

以上で歩行サンプルのプロジェクト設定はひととおり完了になります。

これでもうシミュレーションも可能ですが、せっかくこれだけの設定を行いましたので、再度利用できるように、これまでの設定をプロジェクトファイルとして保存しておきましょう。メインメニューの「ファイル」-「名前を付けてプロジェクトを保存」を実行し、適当な名前をつけて保存してください。保存したファイルは拡張子 ".cnoid" のファイル名で保存され、コレオノイド起動時のコマンドラインパラメータや、メインメニューの「ファイル」-「プロジェクトの読み込み」などで読み込むことが可能です。

なお、ファイルツールバーの「プロジェクト保存」のボタンを押すことでも、プロジェクトファイルの保存を行うことができます。この場合、まだプロジェクトファイルへの保存がされていなければ、「名前を付けて保存」と同様の動作になりますし、既に保存がされている場合は、ファイルメニューの「プロジェクトに保存」と同様に上書き保存となります。
実はコレオノイドはまだ十分に安定しているとは言えない部分もあり、操作によってはクラッシュしてしまうこともあり得ますので、プロジェクト作成中はこまめにこのボタンを押して保存しながら作業を進めることをお勧めします。

.. image:: images/toolbar_save.png

では、シミュレーションを行いましょう。次のページ (:doc:`howto-exec-playback-simulation`) に進んでください。
