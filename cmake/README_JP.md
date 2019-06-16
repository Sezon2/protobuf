このディレクトリには、Windows上でMSVCを使用して  
protobufをビルドするために使用できるCMakeファイルが含まれています。  
プロジェクトは、コマンドプロンプトからVisual Studio IDEを使用して構築できます。  
  
[CMake](http://www.cmake.org)、[Visual Studio](https://www.visualstudio.com)、およびオプションで[Git](http://git-scm.com)が必要です。  
続行する前にコンピュータにインストールされている必要があります。  
  
ほとんどの指示はコマンドプロンプトに表示されますが、適切なGUIツールを使用して同じ操作を実行できます。  

環境設定
====================================================================================================
①スタートメニューから適切なコマンドプロンプトを開きます。  
　※例）VS2013 x64 Native Tools コマンドプロンプト

     C:\Program Files (x86)\Microsoft Visual Studio 12.0\VC\bin\amd64>  

②作業ディレクトリに移動します。

     C:\Program Files (x86)\Microsoft Visual Studio 12.0\VC\bin\amd64>cd C:\Path\to
     C:\Path\to>
     ※「C:\Path\to」は実際の作業ディレクトリへのパスです。  

③ビルド後にprotobuf headers / libraries / binariesがインストールされるフォルダーを作成します。

     C:\Path\to>mkdir install  

④cmakeコマンドがコマンドプロンプトから利用できない場合は、システムのPATH変数に追加します。

     C:\Path\to>set PATH=%PATH%;C:\Program Files (x86)\CMake\bin  

⑤gitコマンドがコマンドプロンプトから利用できない場合は、システムのPATH変数に追加します。

     C:\Path\to>set PATH=%PATH%;C:\Program Files\Git\cmd  

⑥これで準備は整いました。

ソースを入手する
====================================================================================================

最新の安定版ソースパッケージはリリースページから入手できます。

    https://github.com/protocolbuffers/protobuf/releases/latest

例）  
C++だけが必要な場合は`protobuf-cpp-[VERSION].tar.gz`をダウンロードしてください。  
C++とJavaが必要な場合は`protobuf-java-[VERSION].tar.gz`をダウンロードしてください。  
└→すべてのパッケージにはすでにC++ソースが含まれています。  
C++と他の複数の言語が必要な場合は`protobuf-all-[VERSION].tar.gz`をダウンロードしてください。  

あるいは、gitを使ってprotobuf gitリポジトリからクローンを作成することもできます。

     C:\Path\to> git clone -b [release_tag] https://github.com/protocolbuffers/protobuf.git

最新のコードを入手したい場合は、[release_tag]はv3.0.0-beta-1のようなgitタグ、
またはmasterのようなブランチ名です。

①プロジェクトフォルダに移動します。

     C:\Path\to>cd protobuf
     C:\Path\to\protobuf>

②git cloneを使用している場合は、必ずサブモジュールを更新してください（リリース.tar.gzまたは.zipパッケージを使用している場合は、この手順をスキップできます）。

```console
C:\Path\to> git submodule update --init --recursive
```

③protobufソースのcmakeフォルダに移動してください。

     C:\Path\to\protobuf>cd cmake
     C:\Path\to\protobuf\cmake>

④これでCMake設定の準備が整いました。

CMakeの設定
====================================================================================================

CMakeは、さまざまなネイティブビルドシステム用に、さまざまな[generator](http://www.cmake.org/cmake/help/latest/manual/cmake-generators.7.html)をサポートしています。  
[Makefile](http://www.cmake.org/cmake/help/latest/manual/cmake-generators.7.html#makefile-generators)と[Visual Studio](http://www.cmake.org/cmake/help/latest/manual/cmake-generators.7.html#visual-studio-generators)ジェネレータにだけ興味があります。

一時ファイルをprotobufソースコードから分離するためにシャドウビルディングを使います。

一時的な "build"フォルダを作成して作業ディレクトリを変更してください。

     C:\Path\to\protobuf\cmake>mkdir build & cd build
     C:\Path\to\protobuf\cmake\build>

Makefileジェネレータは1つの設定でプロジェクトをビルドすることができるので、  
設定ごとに別々のフォルダを作成する必要があります。

■Release構成で使い始めるには：

     C:\Path\to\protobuf\cmake\build>mkdir release & cd release
     C:\Path\to\protobuf\cmake\build\release>cmake -G "NMake Makefiles" ^
     -DCMAKE_BUILD_TYPE=Release ^
     -DCMAKE_INSTALL_PREFIX=../../../../install ^
     ../..

カレントディレクトリにnmake Makefileを生成します。

■Debug構成を使用する：

     C:\Path\to\protobuf\cmake\build>mkdir debug & cd debug
     C:\Path\to\protobuf\cmake\build\debug>cmake -G "NMake Makefiles" ^
     -DCMAKE_BUILD_TYPE=Debug ^
     -DCMAKE_INSTALL_PREFIX=../../../../install ^
     ../..

カレントディレクトリにnmake Makefileを生成します。

■Visual Studioソリューションファイルを作成するには：

     C:\Path\to\protobuf\cmake\build>mkdir solution & cd solution
     C:\Path\to\protobuf\cmake\build\solution>cmake -G "Visual Studio 14 2015 Win64" ^
     -DCMAKE_INSTALL_PREFIX=../../../../install ^
     ../..

現在のディレクトリにVisual Studioソリューションファイル「protobuf.sln」が生成されます。

gmockディレクトリが存在せず、protobufの単体テストを構築したくない場合は、  
テストを無効にするためにcmakeコマンド引数 `-Dprotobuf_BUILD_TESTS=OFF` を追加する必要があります。

Visual Studio 15 2017用のVisual Studioファイルを作成するには、  
上記のVisual Studioソリューションファイルを作成し、CmakeCacheファイルを編集します。

	C:Path\to\protobuf\cmake\build\solution\CMakeCache

その後、Visual Studioソリューションファイルをもう一度作成します。

コンパイル
====================================================================================================

■protobufをコンパイルする：

     C:\Path\to\protobuf\cmake\build\release>nmake

または

     C:\Path\to\protobuf\cmake\build\debug>nmake

そして、コンパイルが終了するのを待ちます。

■IDEを使いたい場合：

  * 生成されたprotobuf.slnファイルをMicrosoft Visual Studioで開きます。
  * 必要に応じて「Debug」または「Release」構成を選択してください。
  * [ビルド]メニューから[ソリューションのビルド]を選択します。

そしてコンパイルが終了するのを待ちます。

テスト
====================================================================================================

単体テストを実行するには、最初に上記のようにprotobufをコンパイルする必要があります。
コンパイルが終わったら実行します:

     C:\Path\to\protobuf\cmake\build\release>nmake check

または

     C:\Path\to\protobuf\cmake\build\debug>nmake check

Visual Studioソリューションからプロジェクトチェックを構築することもできます。  
それは奇妙に聞こえるかもしれませんが、上手くいきます。

■次のような出力が表示されるはずです：

     Running main() from gmock_main.cc
     [==========] Running 1546 tests from 165 test cases.

     ...

     [==========] 1546 tests from 165 test cases ran. (2529 ms total)
     [  PASSED  ] 1546 tests.

■特定のテストを実行する：

     C:\Path\to\protobuf>cmake\build\release\tests.exe --gtest_filter=AnyTest*
     Running main() from gmock_main.cc
     Note: Google Test filter = AnyTest*
     [==========] Running 3 tests from 1 test case.
     [----------] Global test environment set-up.
     [----------] 3 tests from AnyTest
     [ RUN      ] AnyTest.TestPackAndUnpack
     [       OK ] AnyTest.TestPackAndUnpack (0 ms)
     [ RUN      ] AnyTest.TestPackAndUnpackAny
     [       OK ] AnyTest.TestPackAndUnpackAny (0 ms)
     [ RUN      ] AnyTest.TestIs
     [       OK ] AnyTest.TestIs (0 ms)
     [----------] 3 tests from AnyTest (1 ms total)

     [----------] Global test environment tear-down
     [==========] 3 tests from 1 test case ran. (2 ms total)
     [  PASSED  ] 3 tests.

※注意）テストはソースフォルダから実行する必要があります。

すべてのテストに合格したら、安全に続行します。

インストール
====================================================================================================

■指定したインストールフォルダにprotobufをインストールする：

     C:\Path\to\protobuf\cmake\build\release>nmake install

または

     C:\Path\to\protobuf\cmake\build\debug>nmake install

Visual StudioソリューションからプロジェクトINSTALLをビルドすることもできます。  
それはそれほど奇妙に聞こえないし、上手くいく。

■これにより、インストールした場所に以下のフォルダが作成されます：
  * bin - protobuf protoc.exeコンパイラが含まれています。
  * include - C++ヘッダーとprotobuf *.protoファイルが含まれています。
  * lib - protobufパッケージ用のリンクライブラリとCMake設定ファイルが含まれています。

■必要に応じて以下の作業を行ないます：
  * ヘッダを置きたい場所にincludeディレクトリの内容をコピーしてください。
  * ビルドツールを配置した場所（おそらくPATHのどこかにある）にprotoc.exeをコピーします。
  * リンクライブラリ「libprotobuf[d].lib」「libprotobuf-lite[d].lib」および「libprotoc[d].lib」をライブラリを配置する場所にコピーします。

アプリケーションのデバッグビルドをコンパイルするときに、  
MSVCデバッグライブラリとリリースランタイムライブラリの競合を避けるために、  
"d"接尾辞を付けてlibprotobufd.libのデバッグビルドとリンクする必要があるかもしれません。  
同様に、リリースビルドはリリースlibprotobuf.libライブラリとリンクする必要があります。

DLLと静的リンク
====================================================================================================

静的リンクは、プロトコルバッファライブラリのデフォルトになりました。  
Win32で各DLLに別々のヒープを使用することによる問題、  
およびMSVCのSTLライブラリの異なるバージョン間のバイナリ互換性の問題により、  
静的リンケージのみを使用することをお勧めします。  
ただし、必要に応じて、libprotobufとlibprotocをDLLとして構築することは可能です。  
これを行うには、以下行います。

  * cmakeを呼び出すときに追加のフラグ `-Dprotobuf_BUILD_SHARED_LIBS=ON` を追加します
  * 上記のセクションと同じ手順に従ってください。
  * あなたのプロジェクトをコンパイルするとき、 `#define PROTOBUF_USE_DLLS` を必ず行ってください。

ソフトウェアをエンドユーザーに配布するときは、  
libprotobuf.dllまたはlibprotoc.dllを共有の場所にインストールしないことを強くお勧めします。  
代わりに、これらのライブラリをあなたのバイナリの隣に、あなた自身のアプリケーション自身のインストールディレクトリに置いてください。  
C++はリリース間のバイナリ互換性を維持することを非常に困難にするので、  
これらのライブラリの将来のバージョンはドロップイン置換として使用できない可能性があります。

プロジェクト自体がサードパーティソフトウェアによる使用を目的としたDLLである場合は、  
ライブラリのパブリックインターフェイスでプロトコルバッファオブジェクトを公開しないこと、  
およびプロトコルバッファをライブラリに静的にリンクすることをお勧めします。

ZLib サポート
====================================================================================================

libprotobufにGzipInputStreamとGzipOutputStream(google/protobuf/io/gzip_stream.h)を含める場合は、
いくつか追加の手順を実行する必要があります。

zlibライブラリーのコピーを入手してください。  
zlib.netでプリコンパイルされたDLLは動作します。  
■あなたはそれを準備する必要があります：

  * zlibの2つのヘッダがあなたの `C:\Path\to\install\include` パスにあることを確認してください。
  * zlibのリンクライブラリ(*.libファイル)があなたの `C:\Path\to\install\lib` ライブラリパスにあることを確認してください。

自分でソースからコンパイルすることもできます。

■ソースを入手する：

     C:\Path\to>git clone -b v1.2.8 https://github.com/madler/zlib.git
     C:\Path\to>cd zlib

■コンパイルとインストール：

     C:\Path\to\zlib>mkdir build & cd build
     C:\Path\to\zlib\build>mkdir release & cd release
     C:\Path\to\zlib\build\release>cmake -G "NMake Makefiles" -DCMAKE_BUILD_TYPE=Release ^
     -DCMAKE_INSTALL_PREFIX=../../../install ../..
     C:\Path\to\zlib\build\release>nmake & nmake install

protobufプロジェクトでは、以前と同じように *debug* バージョンを作成するか、
*Visual Studio* ジェネレータを使用することができます。

■ *install* 内の *bin* フォルダをシステムの *PATH* に追加します：

     C:\Path\to>set PATH=%PATH%;C:\Path\to\install\bin

cmakeを起動するときには、 `-Dprotobuf_WITH_ZLIB=ON` フラグでprotobufを再設定する必要があります。

上記のようにZLIBを自分でコンパイルした場合は、さらにオプション `-Dprotobuf_MSVC_STATIC_RUNTIME=OFF` を無効にしてください。

zlib_includeまたはzlib_libに対してNOTFOUNDが報告された場合は、ヘッダーまたは.libファイルを正しいディレクトリーに入れていない可能性があります。

もしあなたがすでにZLIBライブラリとヘッダをあなたのシステムの他の場所に持っているならば、代わりにそれらを見つけるために次の設定フラグを定義することができます。

	-DZLIB_INCLUDE_DIR=<zlibヘッダーを含むdirへのパス>
	-DZLIB_LIB=<zlibを含むディレクトリへのパス>

通常通りprotobufをビルドしてテストします。

コンパイラの警告に関する注意
====================================================================================================

protobufライブラリとコンパイラをビルドしている間、以下の警告は無効にされました。  
あなたはあなた自身のプロジェクトでそれらのいくつかを同様に無効にしなければならないかもしれません、または彼らと一緒に暮らすでしょう。

* C4018 - 'expression'：符号付き/符号なしの不一致
* C4146 - 単項マイナス演算子が符号なしタイプに適用され、結果はまだ符号なしです。
* C4244 - 'type1'から 'type2'への変換、データが失われる可能性があります。
* C4251 - 'identifier'：クラス 'type'はクラス 'type2'のクライアントによって使用されるdllインタフェースを持つ必要があります。
* C4267 - 'size_t'から 'type'への変換、データ損失の可能性
* C4305 - 'identifier'： 'type1'から 'type2'への切り捨て
* C4355 - 'this'：ベースメンバの初期化子リストで使われます。
* C4800 - 'type'：値を強制的に 'true'または 'false'に強制します（パフォーマンス警告）。
* C4996 - 'function'：非推奨と宣言されました。

プロトコルバッファライブラリをDLLとしてコンパイルしている場合は、C4251が特に重要です（前のセクションを参照）。  
プロトコルバッファライブラリは、そのパブリックインタフェースでテンプレートを使用します。  
MSVCは、DLLからテンプレートクラスをエクスポートするための合理的な方法を提供していません。  
ただし、実際には、テンプレートのエクスポートはいずれにしても必要ではないようです。  
任意のテンプレートの完全な定義はヘッダファイルで利用可能であるため、DLLをインポートする人はテンプレートのインスタンスを自分のバイナリにコンパイルすることになります。  
プロトコルバッファの実装は、静的なテンプレートメンバが一意であることに依存していないため、問題はありませんが、それでもMSVCは警告を表示します。  
だから、私たちはそれを無効にします。  
残念ながら、この警告は単にプロトコルバッファを使用するコードをコンパイルするときにも生成されます。つまり、コードで無効にする必要があるかもしれません。  
