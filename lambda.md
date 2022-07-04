# lambda

1から10までの配列を返す
```java
public static int[] generateIntArray(int num) {
	return IntStream.rangeClosed(1, num).map(n->n).toArray();
}
```

1から10までのリストを返す
```java
public static List<Integer> generateIntegerList(int num) {
	return IntStream.rangeClosed(1, num)
		.mapToObj(n->n)
		.collect(Collectors.toList());
}
```

リストを2倍して返す
```java
public List<Integer> doubling(List<Integer> nums) {
    return nums.stream()
        .map(n->n*2)
        .collect(Collectors.toList());
}
```

（順列・ランダム）リストと配列
```java
Random rnd = new Random();
//順列数字リストを作成
List<Integer> intList = IntStream.rangeClosed(1, 10).boxed().collect(Collectors.toList());
//ランダム数字リストを作成
List<Integer> rndList = rnd.ints(10, 1, 10).boxed().collect(Collectors.toList());
//数字配列を作成
int[] intAry = IntStream.rangeClosed(1, 10).toArray();
//ランダム数字配列を作成
int[] rndAry = rnd.ints(10,1,10).toArray();
//順列リストをforEach
intList.forEach(n -> System.out.printf("%d,", n));
System.out.println();
//ランダムリストをforEach
rndList.forEach(n -> System.out.printf("%d,", n));
System.out.println();
//順列配列をリスト化してforEach
intList.stream().forEach(n -> System.out.printf("%d,", n));
System.out.println();
//ランダム配列をリスト化してforEach
rndList.stream().forEach(n -> System.out.printf("%d,", n));
```
