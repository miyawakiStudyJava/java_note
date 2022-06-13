## 理解不足

第01章　Java開発の準備  
　1.1　セットアップ  
　1.2　実行  
  
第02章　Javaの基本  
　2.1　パッケージとインポート  
    011.staticメンバをインポートしたい  
    ```java
    // java.lang.Mathのすべてのstaticメンバをインポート
    import static java.lang.Math.*;
    ```
    
　2.2　変数とデータ型  
    015.ビット演算を行いたい  
    & a&b
    | a|b
    ^ a^b
    ~ ~a
    
    ```java
    <<  a<<b  aをbビット左にシフトする。右側は0で埋める
    >>  a>>b  aをbビット右にシフトする。左側はシフト前の最上位ビットで埋める。
    >>> a>>>b aをbビット右にシフトする。左側は0で埋める。
    ```
    
    023.Optionalってなに？
    
　2.3　コメント  
　2.4　制御構文  
　2.5　例外処理  
　2.6　ラムダ  
　2.7　文字列操作
 
    052.文字列に変数を埋め込みたい。
    
    書式  分類  説明
    b or B  真偽値
    h or H  １６進数
    s or S  文字列
    c or C  文字
    d       １０進数
    o       ８進数
    x or X  １６進数
    e or E  浮動小数点１０進数
    f       １０進数
    g or G  浮動小数点。四捨五入
    a or A
    t or T  日付の前
    B       ロケール固有の月の完全な名前
    A       ロケール固有の曜日の完全な名前
    Y       年
    m       月
    d       日
    k       24時間制の時
    l       12時間制の時
    M       分
    S       秒
    %       パーセントを表示する
    n       開業する

  055.文字列が正規表現に一致するか調べたい。
  056.文字列を正規表現で検索したい。
  057.文字列を正規表現で置換したい。
  
  059.数値を任意の形式にフォーマットしたい。
  ```Java
  /////////////////////////////////////////////////////////////////////////////
		// NumberFormatで整形する
		/////////////////////////////////////////////////////////////////////////////
		{
			// 整数値をフォーマット
			String result1 = NumberFormat.getIntegerInstance().format(1000000); // => "1,000,000"
			System.out.println(result1);
			// 通貨形式にフォーマット
			String result2 = NumberFormat.getCurrencyInstance().format(1000000); // => " ¥1,000,000"
			System.out.println(result2);
			// パーセント形式にフォーマット
			String result3 = NumberFormat.getPercentInstance().format(0.8); // => "80%"
			System.out.println(result3);
		}
		/////////////////////////////////////////////////////////////////////////////
		// ロケールを指定して整形する
		/////////////////////////////////////////////////////////////////////////////
		{
			// USロケールを指定
			Locale locale = Locale.US;
			String result = NumberFormat.getCurrencyInstance(locale).format(1000000); // => "$1,000,000"
			System.out.println(result);
		}
		/////////////////////////////////////////////////////////////////////////////
		// DecimalFormatで整形する
		/////////////////////////////////////////////////////////////////////////////
		{
			// 6桁のゼロで埋めるフォーマット
			DecimalFormat zeroDF = new DecimalFormat("000,000");
			String result1 = zeroDF.format(1234); // => "001,234"
			System.out.println(result1);
			// 負の数に▲をつけてフォーマット
			DecimalFormat negativeDF = new DecimalFormat("###,###; ▲###,###");
			String result2 = negativeDF.format(-1234); // => "▲1,234"
			System.out.println(result2);
		}
  ```

  2.8　正規表現  
　2.9　数値処理  
  
第03章　クラス・インターフェース  
　3.1　クラスとインターフェース  
　3.2　アクセス修飾子  
　3.3　列挙型  
    075.列挙型を使いたい  
	    暗黙メソッド  
	    name:enum定数の名前を取得する  
	    toString:name()メソッドと同じ値を取得する。オーバーライドすることで取得する値を変更できる。  
	    ordinal:enum定数の順序番号を取得する。順序番号は定義順に0から割り振られる。  
	    compareTo:enum定数の定義順を比較する。引数より前の場合は負の値、後の場合は正の値、同じ場合は0を返す。  
	    valueOf:引数がenum定数の名前に該当するenum定数オブジェクトを取得する。  
	    values:列挙型のすべてのenum定数オブジェクトを定義順に取得する。  
    
    076.enum定数ごとにメソッドをオーバーライドしたい。  
    077.列挙型に効率の良いコレクションを使いたい。（EnumSet,EnumMap）  

  3.4　ジェネリクス  
  	080.型パラメータに制限を付けたい。  
	081.ワイルドカードって何に使うの？  
	```java
	// MyClassもしくはそのサブクラスを代入できる。
	<? extends MyClass>
	
	//MyClassもしくはそのスーパークラスを代入できる。
	<? super MyClass>
	```
　3.5　アノテーション  
 	083.標準アノテーションを知りたい
 	```java
	//APIの使用が非推奨
	@Desprecated
	
	//オーバーライド
	@Override
	
	//コンパイラが出す警告を抑制する
	@SuppressWarnings
	
	//ジェネリクスを指定した可変長引数の警告を抑制する
	@SafeVarargs
	
	//関数型インターフェースであることを示す。
	@FunctionalInterface
	```
	
	084.独自アノテーションを作成したい。
	- マーカ・アノテーション
	- フル・アノテーション
	- 単位値アノテーション
	```java
	public @interface Check{
		String value();
		int id();
	}
	```
	
　3.6　リフレクション  
 	085-090.  
　3.7　シリアライズ  
 	091.  
	092.  	
  
第04章　コレクション  
　4.1　導入  
　4.2　配列 
 	097.配列をコピーしたい（浅いコピー、深いコピー）
	098.配列をソートしたい
	099.配列に特定の要素が含まれているか調べたい。
	
　4.3　List  
　4.4　Set  
　4.5　Map  
　4.6　Stream  
 	140.Streamをコレクションに変換したい。
	```java
	////////////////////////////////////////////////////////////////////////////
		// Streamをコレクションに変換する
		/////////////////////////////////////////////////////////////////////////////
		{
			List<String> list = Arrays.asList("Java", "Scala", "JavaScript", "Groovy");
			
			// Streamに対する処理を行ない、結果をListに変換
			List<String> result1 = list.stream().map(s -> s.toUpperCase())
					.collect(Collectors.toList());
			System.out.println(result1);
			
			// Streamに対する処理を行ない、結果をSetに変換
			Set<String> result2 = list.stream().map(s -> s.toUpperCase())
					.collect(Collectors.toSet());
			System.out.println(result2);
			
			// Collectors#toCollection()メソッドを使用すると変換後の実装クラスを指定することも可能
			List<String> result3 = list.stream().map(s -> s.toUpperCase())
					.collect(Collectors.toCollection(LinkedList::new));
			System.out.println(result3);
			
			// 文字列をキーに、その文字列の長さを格納したMapに変換
			Map<String, Integer> map = list.stream()
				.collect(Collectors.toMap(
					s -> s,         // Mapのキーを取得するラムダ式
					s -> s.length() // Mapの値を取得するラムダ式
				));
			System.out.println(map);
		}
		/////////////////////////////////////////////////////////////////////////////
		// Streamを配列に変換する
		/////////////////////////////////////////////////////////////////////////////
		{
			List<String> list = Arrays.asList("Java", "Scala", "JavaScript", "Groovy");
			
			// Streamに対する処理を行ない、結果を配列に変換
			Object[] result1 = list.stream().map(s -> s.toUpperCase())
					.toArray();
			System.out.println(Arrays.toString(result1));
			
			// toArray ( )メソッドに配列のコンストラクタを指定するとその型の配列に変換可能
			String[] result2 = list.stream().map(s -> s.toUpperCase())
					.toArray(String[]::new);
			System.out.println(Arrays.toString(result2));
		}
	```
	
	141.無限の長さを持つStreamを生成したい。
	```java
	// 10、20、40、80...と無限に値を返すStreamを生成
	Stream<Integer> stream = Stream.iterate(10, i -> i * 2);

	// 先頭の5件のみ表示
	stream.limit(5).forEach(System.out::println); // => 10、20、40、80、160の順に表示
	```
  
第05章　日付操作  
　5.1　導入
 	143.Javaでの日付操作について知りたい。
	```java
	//Calendarに1日を足すとそのインスタンス自身の日付が変わる。
	Calendar calendar = Calendar.getInstance();
	
	//LocalDateTimeに1日を足すとそのインスタンスの日付はそのままで
	//加算後の日付を表す新しいLocalDateTimeインスタンスが返却される
	LocalDateTime dateTime = LocalDateTime.now();
	LocalDateTime result = dateTime1.plusDays(1);
	```
　
 5.2　Date and Time API  
  	153.Date and Time APIで現在日時を取得したい。
	```Java
	/////////////////////////////////////////////////////////////////////////////
	// LocalDateTime/LocalDate/LocalTimeで現在日時を取得
	/////////////////////////////////////////////////////////////////////////////
	{
		// 現在日時を生成
		// => 2013-08-11T15:31:11.703
		LocalDateTime localDateTime = LocalDateTime.now();
		System.out.println(localDateTime);
		
		// 現在日を生成
		LocalDate localDate = LocalDate.now(); // => 2013-08-11
		System.out.println(localDate);
		
		// 現在時刻を生成
		LocalTime localTime = LocalTime.now(); // => 15:31:11.707
		System.out.println(localTime);
	}
	/////////////////////////////////////////////////////////////////////////////
	// OffsetDateTimeやZonedDateTimeを保持して現在日時を取得
	/////////////////////////////////////////////////////////////////////////////
	{
		// => 2014-03-29T13:20:11.607+09:00
		OffsetDateTime offsetDateTime = OffsetDateTime.now(); 
		System.out.println(offsetDateTime);
		
		// => 2014-03-29T13:20:11.607+09:00[Asia/Tokyo]
		ZonedDateTime zonedDateTime = ZonedDateTime.now(); 
		System.out.println(zonedDateTime);
	}
	/////////////////////////////////////////////////////////////////////////////
	// TimeZoneを指定して現在日時を取得
	/////////////////////////////////////////////////////////////////////////////
	{
		// => 2013-10-26T23:46:49.621-04:00[America/New_York]
		ZonedDateTime dateTime = ZonedDateTime.now(ZoneId.of("America/New_York")); 
		System.out.println(dateTime);
	}
	/////////////////////////////////////////////////////////////////////////////
	// Clockを指定して現在日時を取得
	/////////////////////////////////////////////////////////////////////////////
	{
		// 常にエポックタイムを返すシステムクロック
		Clock mock = Clock.fixed(Instant.EPOCH, ZoneId.systemDefault());
		MyBean bean = new MyBean(mock);
		LocalDateTime current = bean.current(); // => 1970-01-01T09:00
		System.out.println(current);

		// デフォルトのタイムゾーンを使用
		Clock jpClock = Clock.systemDefaultZone();
		MyBean bean2 = new MyBean(jpClock);
		LocalDateTime current2 = bean2.current(); // => 2014-04-06T10:45:07.294
		System.out.println(current2);

		// タイムゾーンを指定
		Clock usClock = Clock.system(ZoneId.of("America/New_York"));
		MyBean bean3 = new MyBean(usClock);
		LocalDateTime current3 = bean3.current(); // => 2014-04-06T10:45:07.294
		System.out.println(current3);
	}	
	```
	
	162.文字列をDate and Time APIのオブジェクトに変換したい
	```java
	/////////////////////////////////////////////////////////////////////////////
	// デフォルトのフォーマットでパースする
	/////////////////////////////////////////////////////////////////////////////
	{
		// "2007-12-03T10:15:30"のような文字列からLocalDateTimeを生成
		LocalDateTime dateTime = LocalDateTime.parse("2013-12-24T12:00");
		System.out.println(dateTime);
		// "2007-12-03"のような文字列からLocalDateを生成
		LocalDate date = LocalDate.parse("2013-12-25");
		System.out.println(date);
		// "2007-12-03T10:15:30+01:00"のような文字列からOffsetDateTimeを生成
		OffsetDateTime offsetDateTime = OffsetDateTime.parse("2014-01-01T00:00:00+01:00");
		System.out.println(offsetDateTime);
		// "2007-12-03T10:15:30+01:00[Europe/Paris]"のような文字列からZonedDateTimeを生成
		ZonedDateTime zonedDateTime =
				ZonedDateTime.parse("2014-01-01T00:11:10+09:00[Asia/Tokyo]");
		System.out.println(zonedDateTime);
	}
	/////////////////////////////////////////////////////////////////////////////
	// フォーマットを指定してパースする
	/////////////////////////////////////////////////////////////////////////////
	{
		// フォーマットを指定してDateTimeFormatterを生成
		DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy/MM/dd HH:mm");
		// "yyyy/MM/dd HH:mm"形式の文字列からLocalDateTimeを生成
		LocalDateTime dateTime = LocalDateTime.parse("2013/12/31 00:00", formatter);
		System.out.println(dateTime);
	}
	```
第06章　ファイル・入出力  
　6.1　導入  
　6.2　ファイル  
　6.3　パス  
　6.4　入出力  
  
第07章　並行プログラミング  
　7.1　導入  
　7.2　スレッド  
　7.3　タイマー  
　7.4　Concurrency Utilities  
　7.5　Fork/Join Framework  
  
第08章　JDBC  
　8.1　基本的なデータベース操作  
　8.2　高度なデータベース操作  
  
第09章　Junit  
　9.1　導入  
　9.2　テストケース  
　9.3　テストスイート  
  
第10章　ネットワーク、システム、ユーティリティ  
　10.1　ネットワーク  
　10.2　ユーティリティ  
　10.3　システム  
  
第11章　これからのJava  
　11.1　リリースポリシーの変更  
　11.2　モジュールシステム  
　11.3　新しい構文  
　11.4　APIの拡張  
　11.5　ツール  
　11.6　その他  
