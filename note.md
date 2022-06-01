#java_note

参考
https://zenn.dev/khasunuma/books/javase8-study/viewer/chapter03

忘れやすいポイント

ファイル構成
```java
package package_name ;

[ import package_name . { ClassName | * } ; [ import ... ; ] ... ]

// ClassName : クラス名
[ public ] [ final ] class ClassName {
    // コンストラクタ定義
    [ constructor [ constructor ... ] ]

    // フィールド定義
    [ field [ field ... ] ]

    // メソッド定義
    [ method [ method ... ] ]
}
```

メソッド呼び出し
```java
// (1) メソッド呼び出し式
// 仮引数なしのため、実引数も設定しない
instance.run()
// 仮引数 String str に対して実引数 "Hello" を設定
sb.append("Hello")

// (2) メソッド呼び出し式 (static メソッド)
// 仮引数 int year, int month, int dayOfMonth に対して実引数 2016, 9, 9 を設定
LocalDate.of(2016, 9, 9)

// (3) メソッド呼び出し式 (メソッドチェーン)
// 上記 (2) の結果 (LocalDate クラスのインスタンス) に対してさらにメソッド呼び出し式 (1) を適用
LocalDate.of(2016, 9, 9).withDayOfMonth(10)
```

文字リテラル
|Esc	|意味	|Unicode|
|---|---|---|
|\b	|backspace (BS)	|\u0008|
|\t	|horizontal tab (HT)	|\u0009|
|\n	|linefeed (LF)	|\u000a|
|\f	|form feed (FF)	|\u000c|
|\r	|carriage return (CR)	|\u000d|
|\"	|double quote (")	|\u0022|
|\'	|single quote (')	|\u0027|
|\\	|backslash ()	|\u005c|

文字列操作
```java
// 結合
String join = "aaa" + "bbb";

// 分割
String[] record = "aaa,bbb,ccc".split( "," );

// 長さ
int length = "abcdef".length();

// 切り出し
"abcd".substring( 0, 2 )   // abc

// 検索
int result = "abcd".indexOf( "cd" ) // 見つかった場合はその位置、見つからなかった場合は-1が返る
```

キャスト
```java
// (1) 数値型のサイズ縮小
int intValue = 10;
byte byteValue = (byte) intValue;

// (2) 数値 (整数型) から数値 (浮動小数点型) へのサイズ拡大
int x = 3;
int y = 2;
double d = (double) x / (double) y;

// (3) スーパークラスの参照からサブクラスの参照への変換
// number のインスタンスが BigDecimal であること
BigDecimal decimal = (Number) number;
```

配列のコピー
```java
int[] from = new int[] { 1, 2, 3 };
int[] to = new int[5];

System.arraycopy( from, 0, to, 0, from.length );
```

ファイル入力(try-catch文)
```java
import java.io.*;

BufferedReader reader = null;
try {
    reader = new BufferedReader( new FileReader( filename ) );
    String line;
    while ( ( line = reader.readLine() ) != null ) {
    }
} catch ( IOException e ) {
    // エラー処理:  
} finally {
    if ( reader != null ) {
        try {
        	reader.close();
        } catch ( IOException e ) {}s
    }
}
```

ファイル出力
```java
PrintWriter writer = null;
try {
    writer = new PrintWriter( new BufferedWriter( new FileWriter( filename ) ) ); 
    writer.println( "abc" );
    writer.println( "def" );
    writer.println( "fgh" );
} catch ( IOException e ) {
    // エラー処理:   
} finally {
    if ( writer != null ) {
        writer.close();
    }
}
```

var
```java
//var x; のように右辺式を省略したり、var x = 123, y = 1.234; のように複数の変数を宣言することはできません。
var 変数名 = 初期値;
```

例外
|例外クラス|	チェック|	意味|
|---|---|---|
|Exception	|○	|すべての例外のスーパークラス|
|IOException	|○	|I/O 処理でエラーが発生した|
|SQLException	|○	|JDBC での SQL 実行時にエラーが発生した|
|RuntimeException	|×	|被チェック例外 (ランタイム例外) のスーパークラス|
|NullPointerException	|×	|引数が null である|
|ClassCastException	|×	|データ型のキャストに失敗した|
|IllegalArgumentException	|×	|引数に不正な値が渡された (引数が null の場合にも用いる場合がある)|
|NumberFormatException	|×	|文字列が数値に変換できない書式になっている|
|ArithmeticException	|×	|異常な算術演算が行われた、例えば数値を 0 で割ったなど|
|UnsupportedOperationException	|×	|メソッドの実行はサポートされていない|
|UncatchedIOException	|×	|(後述の catch 節にて) IOException を非チェック例外に変換する|

例外の再スロー
```java
public int read(byte[] buf) throws IOException {
    ...
    try {
        ...
    } catch (IOException e) {
        ...
        // 例外の再スロー
        throw e;
    }
    ...
}
```
例外の再スローは、例えば例外をキャッチした記録だけを取り、その他の処理を呼び出し元に委ねる場合などに使用します。

例外の付け替え
```java
public int read(byte[] buf) throws Exception {
    ...
    try {
        ...
    } catch (IOException e) {
        ...
        // アプリケーション独自の ApplicationException に付け替える
        throw new ApplicationException("ERROR-00600");
    }
    ...
}
```
例外の付け替えは、チェック例外を非チェック例外に変える場合や、Java 標準の例外をアプリケーション独自の例外に変える場合などに使用します。特に非チェック例外への変換は、呼び出し元でのキャッチが不要になるため、特別なリカバリ処理を要しないアプリケーションに向いています。


例外連鎖
付け替えた例外オブジェクトに元の例外オブジェクトを関連付けてスローすることができます。これを「例外連鎖」といい、元の例外オブジェクトを「原因例外」と呼びます。例外連鎖は原則として Java 標準 API に含まれる例外クラスにはすべて備わっており、例外クラスのコンストラクタの引数に原因例外を設定する書式が存在しています。例外連鎖を使用するか否かはプログラマの裁量に委ねられますが、より多くの情報を呼び出し元に伝えることができるため、可能な限り使用した方が良いでしょう (呼び出し元で原因例外の情報が不要であれば無視することもできます)。

ただし、例外連鎖の実装は必須ではなく、特にユーザーが実装した例外クラスでは例外連鎖がサポートされていない場合があります。反対に、例外連鎖を必須とする例外クラス (コンストラクタの引数に原因例外を必ず設定しなければならない) も存在します。Java 標準 API の InvocationTargetException や UncheckedIOException は例外連鎖が必須となる代表例です。

```java
public int read(byte[] buf) {
    ...
    try {
        ...
    } catch (IOException e) {
        throw new UncheckedIOException(e);
    }
    ...
}

public void doProcess() {
    ...
    try {
        ...
    } catch (UncheckedIOException e) {
        // e.getCause() で原因例外 (ここでは IOException のインスタンス) を取得する
        // 原因例外が設定されていない場合は e.getCause() の値が null となるため、その対策をしておく
        // (ただし UncheckedIOException に限っては必ず原因例外を持つため、対策はなくても可)
        if (e.getCause() != null) {
            // e.getCause().printStackTrace() で標準エラー出力に例外のスタックトレースを出力する
            // スタックトレースは例外がスローされてから Java VM に届くまで (= プログラムが停止するまで) の詳細な記録である
            // 原因例外を含むすべての例外の情報を含み、プログラム中で例外がスローされた箇所が特定されている
            e.getCause().printStackTrace();
        }
    }
    ...
}
```

## 例外処理で避けたいこと
### catch 節で何もしない
try-catch 文で例外をキャッチしても、catch 節で何も処理をしないと、原因をわからなくするどころか例外自体をなかったことにしてしまいます。これを俗に「例外を握りつぶす」行為といい、Java の開発現場で最も忌避される手法です (その割に例外を握りつぶしてしまう人が後を絶たないのは困りものですが)。例外を握りつぶすことは、例外処理の機構そのものを破壊することとほぼ等しいため、絶対に行わないようにしましょう。

### RuntimeException の濫用と例外連鎖の不使用の組み合わせ
RuntimeException は標準で用意されている汎用の非チェック例外であり、呼び出し元で try-catch 文を使用する必要がなくなるため、使い方によっては非常に便利です。しかし、みだりに使用するのも考え物です。特にチェック例外を catch 節で RuntimeException に付け替え、その際に原因例外を設定しない場合、スタックトレースでは RuntimeException に付け替えたところまでしか追跡することができず、エラーの原因を示す例外を特定できなくなってしまいます。そもそも RuntimeException は非チェック例外すべてのスーパークラスであり、何が原因で例外スローに至ったのかクラス名だけではわからない欠点があります。非チェック例外をスローする場合は RuntimeException のスローはできるだけ減らし、原因を示すような名前を持つ非チェック例外 (RuntimeException のサブクラス) を必要に応じて作成してスローするのが良いでしょう。

### Throwable のキャッチおよびスロー
try-catch 文で Throwable をキャッチするようなコーディングは避けてください。Exception、RuntimeException だけでなく Error までキャッチしてしまう恐れがあるためです。Error は Java VM のエラーなど深刻な状態を表すため、特別な理由がなければキャッチすべきではない例外オブジェクトです。仮に Error をキャッチしてもアプリケーションで対処できることはありません。

ファクトリメソッド
```java
public final class LocalDate {

    private LocalDate(int year, int month, int dayOfMonth) {
        ...
    }

    public static LocalDate of(int year, int month, int dayOfMonth) {
        // 実際の LocalDate では複数のメソッドで処理しているが、ここでは省略する
        ...
        return new LocalDate(year, month, dayOfMonth);
    }
    ...
}
```
インスタンスの生成において、コンストラクタによる初期化に加えて追加の処理を必要とする場合などは、ファクトリ・メソッドと呼ばれるメソッドが用意される場合があります。コンストラクタとファクトリ・メソッドを同一クラス上に定義する場合には、ファクトリ・メソッドを public な static メソッドとして定義し、コンストラクタを private (継承を許可する場合は protected) スコープで定義します。ファクトリ・メソッドにはインスタンス生成に関わる処理を隠ぺいするだけでなく、コンストラクタの表現力不足 (コンストラクタでは同じ引数リストを持つものを複数定義できないが、メソッドであれば名前を変えることで同じ引数リストを持つものを定義可能、等) を補う意味もあります。以下に Java 標準 API の LocalDate クラスの実装を一部簡略化して示します。

LocalDate クラスは日付を表しますが、日付の表現には様々な方法がある (年-月-日、年-月の名前-日、年-年始からの通算日、等) だけでなく、イミュータブルなクラスとして設計されているため、日付の加算・減算等でもインスタンスの生成が必要となります。これらをすべてコンストラクタで実現しようとすると無理が生じるため、コンストラクタは private スコープとしてクラスの内部に隠ぺいし、代わりにファクトリ・メソッドを豊富に用意することで対応してます。

なお、Byte、Integer、Long、Float、Double の各クラスは public なコンストラクタと valueOf ファクトリ・メソッドの両方を持つ特殊な例です。これらのクラスの valueOf ファクトリ・メソッドは値の全部または一部をキャッシュするため、コンストラクタでインスタンスを生成する場合と比較して効率が良いとされます。

シングルトン
```java
public final class Singleton {
    // private な static フィールドに唯一のインスタンスを設定する
    private static final Singleton instance = new Singleton();

    private Singleton() {
        ...
    }

    public static Singleton getInstance() {
        // instance の値は常に同じであるため、このメソッドの戻り値は常に同じ値となる
        return instance;
    }
    ...
}
```

Collection
|メソッド名	|説明|
|---|---|
|boolean add(E e)	|コレクションに要素を追加する|
|boolean addAll(Collection <? extends E> c)	|コレクションにすべての要素を追加する|
|void clear()	|コレクションの全要素を削除する|
|boolean contains(Object o)	|要素がコレクションに含まれている場合は true、そうでなければ false を返す|
|boolean isEmpty()	|コレクションの要素数が 0 の場合は true、そうでなければ false を返す|
|Iterator<E> iterator()	|イテレータを返す (後述)|
|boolean remove(Object o)	|コレクションの要素を削除する|
|int size()	|コレクションの要素数を返す|
|<T> T[] toArray(T[] a)	|コレクションを T 型/クラスの配列に変換する (後述)|

Iterator
|メソッド	|説明|
|hasNext()	|次の要素がある場合は true、そうでなければ false (要素数が 0 の場合は常に false)|
|next()	|次の要素を取得する|
|remove()	|現在の要素 (= 前回 next() で取得した要素) を削除する|

List
|メソッド名	|説明|
|---|---|
|boolean add(int index, E element)	|index 番目に要素を挿入する|
|E get(int index)	|index 番目の要素を取得する|
|int indexOf(Object o)	|要素が最初に見つかった index を返す|
|int lastIndexOf(Object o)	|要素が最後に見つかった index を返す|
|int remove(int index)	|index 番目の要素を削除する|
|E set(int index, E element)	|index 番目の要素を置き換える|

Queue,Deque
|メソッド名	|説明|
|---|---|
|boolean add(E e)	|要素をキューへ挿入する|
|boolean offer(E e)	|要素をキューへ挿入する (失敗した場合は非チェック例外 NoSuchElementException をスロー)|
|E poll()	|要素を取得してキューから取り除く (要素がなければ null を返す)|
|E element()	|要素を取得してキューから取り除く (要素がなければ非チェック例外 NoSuchElementException をスロー)|
|E peek()	|要素を取得するがキューには残す (要素がなければ null を返す)|
|E remove()	|要素を取得するがキューには残す (要素がなければ非チェック例外 NoSuchElementException をスロー)|


Map
|メソッド名	|説明|
|---|---|
|void clear()	|すべてのエントリ (キーと値のペア) を削除する|
|boolean containsKey(Object key)	|キーが含まれている場合は true、そうでない場合は false を返す|
|Set<K> keySet()	|すべてのキーを返す (Set インタフェース)|
|V get(Object key)	|キーに対応する値を取得する (存在しない場合は null を返す)|
|V getOrDefault(Object key, V defaultValue)	|キーに対応する値を取得する (存在しない場合は defaultValue を返す)|
|V put(K key, V value)	|キーに対応する値を設定する (既に存在する場合は置き換える)|
|V putIfAbsent(K key, V value)	|キーに対応する値を設定する (既に存在する場合は置き換えない)|
|V remove(Object key)	|キーと対応する値を削除する (存在しない場合は何もしない)|
|int size()	|エントリ (キーと値のペア) の数を返す|
|Collection<V> values()	|すべての値を返す (Collection インタフェース)|


Arrays クラス
|メソッド名	|説明|
|---|---|
|asList	|指定された配列に連動する固定サイズのリストを返す|
|binarySearch	|配列から指定された値を検索する (二分検索)|
|copyOf	|指定された配列をコピーする (長さは必要に応じて切り詰めるかパディング)|
|copyOfRange	|指定された配列の指定された範囲を新しい配列にコピーする|
|equals	|2 つの配列が互いに同等である場合に true を返す|
|hashCode	|指定された配列の内容に基づくハッシュ・コードを返す|
|toString	|指定された配列の文字列表現を返す|
|deepEquals	|2つの配列が互いに等価な場合に true を返す (深層内容)|
|deepHashCode	|指定された配列のハッシュ・コードを返す (深層内容)|
|deepToString	|指定された配列の文字列表現を返す (深層内容)|
|fill	|配列の各要素を設定する (同一の値を使用する)|
|sort	|指定された配列をソートする|
|setAll	|配列のすべての要素を設定する (異なる値も設定可)|
|parallelSort	|指定された配列をソートする (マージソード・並列処理)|
|parallelSetAll	|配列のすべての要素を設定する (異なる値も設定可・並列処理)|


Collections クラス
|メソッド名	|対象	|説明|
|---|---|---|
|addAll	|Collection	|指定されたすべての要素を Collection に追加する (高速)|
|binarySearch	|List	|リストからインスタンスを検索する (二分探索)|
|copy	|List	|あるリストから別のリストにすべての要素をコピーする|
|disjoint	|Collection	|2 つの Collection に共通の要素がない場合に true を返す|
|emptyIterator	|N/A	|空のイテレータを返す|
|emptyList	|N/A	|空の List を返す|
|emptyMap	|N/A	|空の Map を返す|
|emptySet	|N/A	|空の Set を返す|
|fill	|List	|すべての要素を指定した要素で置き換える|
|frequency	|Collection	|指定されたオブジェクトと等価な要素の数を返す|
|indexOfSubList	|List	|サブ・リストが最初に出現した位置の開始位置を返す|
|lastIndexOfSubList	|List	|サブ・リストが最後に出現した位置の開始位置を返す|
|max	|Collection	|最大の要素を返す|
|min	|Collection	|最小の要素を返す|
|nCopies	|N/A	|指定されたインスタンスの n 個のコピーで構成される不変の List を返す|
|replaceAll	|List	|指定された値をすべて置き換える|
|reverse	|List	|リストの要素の順序を逆にする|
|rotate	|List	|リストの要素を指定された距離により回転する|
|shuffle	|List	|リストの順序をシャッフルする|
|singleton	|N/A	|指定されたインスタンスだけを格納する不変の Set を返す|
|singletonList	|N/A	|指定されたインスタンスだけを格納する不変の List を返す|
|singletonMap	|N/A	|指定されたインスタンスだけを格納する不変の Map を返す|
|sort	|N/A	|リストを昇順ソートする|
|swap	|List	|リストの指定された位置にある要素を入れ替える|
|synchronizedCollection	|Collection	|同期化 (10 章) された Collection を返す|
|synchronizedList	|List	|同期化 (10 章) された List を返す|
|synchronizedMap	|Map	|同期化 (10 章) された Map を返す|
|synchronizedSet	|Set	|同期化 (10 章) された Set を返す|
|unmodifiableCollection	|Collection	|不変な Collection を返す|
|unmodifiableList	|List	|不変な List を返す|
|unmodifiableMap	|Map	|不変な Map を返す|
|unmodifiableSet	|Set	|不変な Set を返す|

ファイル操作の基本

|操作	|InputStream	|OutputStream|
|---|---|---|
|ファイルのオープン	|new FileInputStream()	|new FileOutputStream()|
|1 バイトの読み取り/書き込み	|read()	|write(int)|
|配列の要素数だけ読み取り/書き込み	|read(byte[], int, int)	|write(byte[], int, int)|
|指定した長さだけ読み取り/書き込み	|read(byte[], int, int)	|write(byte[], int, int)|
|ファイルのフラッシュ	|N/A	|flush()|
|ファイルのクローズ	|close()	|close()|

- コンストラクタおよびメソッドは IOException (またはそのサブクラス) をスローする可能性があります。したがって、例外処理が必須となります。
^ 処理の最後に必ずファイルのクローズが必要です。原則として try-catch 文の finally 句でクローズします。


|操作	|Reader	|Writer|
|---|---|---|
|ファイルのオープン	|new FileReader()	|new FileWriter()|
|1 文字の読み取り/書き込み	|read()	|write(int)|
|配列の要素数だけ読み取り/書き込み	|read(char[], int, int)	|write(char[], int, int)|
|指定した長さだけ読み取り/書き込み	|read(char[], int, int)	|write(char[], int, int)|
|文字列の読み取り/書き込み	|N/A	|write(String)|
|ファイルのフラッシュ	|N/A	|flush()|
|ファイルのクローズ	|close( )	|close()|

入出力ストリームのバッファリング
 >一般に、入出力ストリームは read/write のメソッドが呼ばれる度に入出力を行います。そのため、入出力対象がメモリ等の高速なものでない限り、大きな負荷が掛かりやすくなります。そこで、こうした処理の効率化のためにバッファリングを行う入出力ストリームが用意されています。ファイル入出力ではバッファリングを行った方が良いでしょう。


FileSystem クラスと Path インタフェース

```java
FileSystem fileSystem = FileSystems.getDefault();

// デリミタを含む fileSystem.getPath("C:\\eclipse\\eclipse.ini") でも OK
Path path = fileSystem.getPath("C", "eclipse", "eclipse.ini");
```

|メソッド名	|説明|
|getFileName	|ファイルまたはディレクトリ名そのもの (ディレクトリ構造を含まない) を取得する|
|getParent	|親ディレクトリのパスを取得する (親を持たない場合は null)|
|resolve	|指定されたパスをこのパスに対して解決する (例: パスの子要素を取得する)|
|resolveSibling	|指定されたパスをこのパスの親パスに対して解決する (例: ファイル名を変更する)|
|toAbsolutePath	|絶対パスを取得する|
|toString	|文字列表現を取得する (書式は OS に依存する)|


### Files クラス

Files クラスが提供するメソッドのうちファイル・システム操作に関するものには、以下のようなものが用意されています。具体的なメソッド名については API ドキュメントを参照してください。

- ファイルのコピー (※NIO.2 以前、実はファイルのコピー機能も提供されていなかった)
- ファイルの移動 (リネーム)
- ディレクトリの作成
- 空ファイルの作成
- ハードリンク/シンボリックリンクの作成
- 一時ディレクトリの作成
- 空の一時ファイルの作成
- ファイル (ディレクトリ) の削除
- ファイル (ディレクトリ) の存在チェック
- ファイル (ディレクトリ) のサイズ・属性・更新日時・所有者等の取得
- ディレクトリ・ツリーのスキャン

|メソッド名	|説明|
|newBufferedReader	|ファイルを読み取り用に開き BufferedReader を返す|
|newBufferedWriter	|ファイルを書き込み用に開き BufferedWriter を返す|
|newInputStream	|ファイルを読み取り用に開き InputStream を返す|
|newOutputStream	|ファイルを書き込み用に開き OutputStream を返す|
|readAllBytes	|ファイルからすべてのバイトを読み取り byte[] で返す|
|readAllLines	|ファイルからすべての行を読み取り List<String> で返す|
|lines	|ファイルからすべての行を読み取り Stream<String> で返す (Java SE 8 以降)|
|write	|すべてのバイトまたは行をファイルに書き込む (一部の書式は Java SE 8 以降)|

※readAllBytes()、readAllLines()、lines() および write() は、完了後にファイルがクローズされる。


### try-catch 文を用いたファイルの読み込み
テキスト・ファイルを 1 行単位で読み込み、何らかの処理を行うメソッドの例を以下に示します。これが Java におけるファイル読み込みの基本形となりますが、現在では次節に示すように try-with-resources 文を使用したほうがよいでしょう。

```java
public void readFile(Path path) throws IOException {
    BufferedReader reader = Files.newBufferedReader(path, Charset.forName("UTF-8"));
    try {
        String line = reader.readLine();
        while (line != null) {
            // ここで何か処理を行う
            ...
            line = reader.readLine();
        }
    } catch (IOException e) {
        ...
        // IOException の再スロー、その他の処理がなければ catch 節ごと省略可能
        throw e;
    } finally {
        reader.close();
    }
}
```

### try-with-resources 文を用いたファイルの読み込み
```java
public void readFile(Path path) throws IOException {
    // リソース BufferedReader reader = Files.newBufferedReader(path) をオープンする
    // ここでオープンしたリソースは try-catch を抜ける際に自動的にクローズされる 
    try (BufferedReader reader = Files.newBufferdReader(path, Charset.forName("UTF-8"))) {
        String line = reader.readLine();
        while (line != null) {
            // ここで何か処理を行う
            ...
            line = reader.readLine();
        }
    } catch (IOException e) {
        ...
        // IOException の再スロー、その他の処理がなければ catch 節ごと省略可能
        throw e;
    } // close() が不要となるので、finally 節も省略できる
}
```

```java
public void readFile(Path path) throws IOException {
    try {
        for (String line : Files.readAllLines(path, Charset.forName("UTF-8"))) {
            // ここで何か処理を行う
            ...
        }
    } catch (IOException e) {
        ...
        // IOException の再スロー、その他の処理がなければ catch 節ごと省略可能
        throw e;
    }
}
```

```java
public void writeFile(Path path, List<String> lines) throws IOException {
    try {
        Files.write(path, lines, Charset.forName("UTF-8"));
    } catch (IOException e) {
        ...
        // IOException の再スロー、その他の処理がなければ catch 節ごと省略可能
        throw e;
    }
}
```

関数インターフェース
|関数インタフェース	|メソッド	|主な用途など|
|---|---|---|
|Runnable	|void run()	|スレッドの処理 (戻り値は返さない)|
|Callable<V>	|V call()	|スレッドの処理 (戻り値を返す)|
|Comparator<T>	|int compare(T o1, T o2)	|コレクション (比較結果を数値で返す)|
|Supplier<T>	|T get()	|Stream API (戻り値を返し、引数は受け取らない)|
|Consumer<T>	|void accept(T t)	|Stream API (引数を受け取り、戻り値は返さない)|
|Function<T, R>	|R accept(T t)	|Stream API (引数を受け取り、戻り値を返す)|
|Predicate<T>	|boolean test(T t)	|Stream API (引数を受け取り、条件により true or false を返す)|

メソッド参照
|参照する対象	|ラムダ式	|メソッド参照|
|インスタンスのメソッド	|e -> list.add(e)	|list::add|
|クラスの static メソッド	|s -> System.out.println(s)	|System.out::println|
|クラスのコンストラクタ	|() -> new ArrayList()	|ArrayList::new|

Streamの生成
|ソース	|クラス	|生成するメソッド	|生成されるストリーム|
|---|---|---|---|
|配列	|Arrays	<T>stream(T[])	|Stream<T>|
||Arrays	|stream(int[])	|IntStream|
|コレクション	|Collection<T>	|stream()	|Stream<T>|
|任意の要素	|Stream	<T>of(T...)	Stream<T>|
||IntStream	|of(int...)	|IntStream|
|n から m	|IntStream	|rangeClosed(int n, int m)	|IntStream|
|n から m - 1	|IntStream	|range(int n, int m)	|IntStream|
|文字列	|String	|chars()	|IntStream|
|ランダムな整数	|Random	|ints()	|IntStream|
|ファイルの各行	|BufferedReader	|lines()	|Stream<String>|
||Files	|lines(Path)	|Stream<String>|
|ディレクトリ内の要素	|Files	list(Path)	|Stream<Path>|
||Files	|walk(Path, int, FileVisitOption...)	|Stream<Path>|

中間操作
|メソッド名	|引数	|概要|
|filter()	|T -> boolean	|フィルタリングする|
|map()	|T -> U	|Stream<U> へ変換する|
|flatMap()	|T -> Stream<U>	|Stream<U> へ変換する|
|distinct()	|なし	|同一の要素を除外する|
|sorted()	|なし	|自然順序で並び替える|
|sorted()	|(T, T) -> int	|並び替える|
|peek()	|T -> void	|デバッグ用途の forEach()|
|limit()	|long	|引数の件数に要素数を制限する|
|skip()	|long	|引数の件数文を読み飛ばす|

終端操作
|メソッド名	|引数	|戻り値	|概要|
|forEach()	|T -> void	|void	|要素ごとに何らかの処理を行う|
|findAny()	|なし	|Optional<T>	|任意の要素を 1 つだけ取得する|
|findFirst()	|なし	|Optional<T>	|最初の要素を取得する|
|anyMatch()	|T -> boolean	|boolean	|いずれかの要素が条件に沿うか|
|allMatch()	|T -> boolean	|boolean	|すべての要素が条件に沿うか|
|noneMatch()	|T -> boolean	|boolean	|すべての要素が条件に沿わないか|
|reduce()	|(T, T) -> T	|Optional<T>	|各要素から 1 つの要素に畳み込む|
|collect()	|Collector<? super T, A, R>	|R	|各要素から 1 つのオブジェクトに集計する|
|toArray()	|int -> A[]	|A[]	|すべての要素を含む配列を生成する|
|count()	|なし	|int	|要素数を取得する|

>findFirst および findAny は該当する要素が見つからない場合があるため、戻り値が Optional になります。Optional については後ほど取り上げます。また、reduce は汎用の終端操作であり、他では実現できない特殊な操作を実装する際に使用します。

```java
Stream<String> stream = ... ;

// コレクション (List) への変換
List<String> list = stream.collect(Collectors.toList());

// 配列への変換
// String[]::new は i -> new String[i] と同じ
String[] array = stream.toArray(String[]::new);
```

Optional
|メソッド名	|引数	|説明|
|of	|T	|値をラップして Optional のインスタンスを生成する。値が null の場合は NullPointerException をスローする。|
|ofNullable	|T	|値をラップして Optional のインスタンスを生成する。値が null の場合は保持しない。|
|empty	|なし	|値を保持しない Optional のインスタンスを生成する。Optional.ofNullable(null) と同じ。|

```java
// 値 (null の可能性がある)
String original = ... ;

// original をラップして Optional のインスタンスを生成する → value
Optional<String> value = Optional.ofNullable(original);

// get : 
// 値を保持する場合は値を s1 に代入し、保持しない場合は NoSuchElementException をスローする
String s1 = value.get();

// orElse : 
// 値を保持する場合は値を s2 に代入し、保持しない場合は引数の値を s2 に代入する
String s2 = value.orElse("default value");

// orElseGet : 
// 値を保持する場合は値に s3 を代入し、保持しない場合は引数 (ラムダ式) で生成される値を s3 に代入する
String s3 = value.orElseGet(() -> "default value");

// orElseThrow : 
// 値を保持する場合は値を s4 に代入し、保持しない場合は引数 (ラムダ式) で生成される例外をスローする
String s4 = value.orElseThrow(() -> new IllegalArgumentException());
```

Date and Time API
https://zenn.dev/khasunuma/books/javase8-study/viewer/chapter13
