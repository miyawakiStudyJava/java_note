#java_note

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
