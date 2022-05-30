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
Esc	意味	Unicode
\b	backspace (BS)	\u0008
\t	horizontal tab (HT)	\u0009
\n	linefeed (LF)	\u000a
\f	form feed (FF)	\u000c
\r	carriage return (CR)	\u000d
\"	double quote (")	\u0022
\'	single quote (')	\u0027
\\	backslash ()	\u005c

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
