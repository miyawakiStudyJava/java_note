Date And API

```java
// Calendar,Date,LocalDate

//Calendar型の設定
Calendar calendar = Calendar.getInstance();
calendar.set(Calendar.YEAR, 2018);
calendar.set(Calendar.MONTH, 11);
calendar.set(Calendar.DAY_OF_MONTH, 20);
System.out.println("カレンダー型："+calendar.getTime());

// Calendar型 → Date型の変換
Date date = calendar.getTime();

// SimpleDateFormatクラスでフォーマットパターンを設定する
SimpleDateFormat sdf = new SimpleDateFormat("yyyy/MM/dd");
System.out.println("フォーマッター："+sdf.format(calendar.getTime()));

// String型 → Date型
Date birthday = null;
try {
	birthday = sdf.parse("1983/02/25");
} catch (ParseException e) {
	e.printStackTrace();
}

// LocalDateの設定
LocalDate localBirthday = LocalDate.of(1983, 2, 25);
LocalDate localToday = LocalDate.now();
System.out.println("LocalDate型 誕生日："+localBirthday);
System.out.println("LocalDate型 本日："+localToday);
System.out.println();

// String → LocalDate
String strDate = "1983/02/25";
LocalDate parseBirthday = LocalDate.parse(strDate, DateTimeFormatter.ofPattern("yyyy/MM/dd"));


// 各種フォーマッターパターン
DateTimeFormatter dtf = DateTimeFormatter.ofPattern("MM月dd日");
System.out.println("formatter:"+dtf.format(parseBirthday));


// LocalDate間の差分
long diff = ChronoUnit.YEARS.between(localBirthday, localToday);
System.out.println("年齢："+diff);
```
