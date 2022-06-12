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
