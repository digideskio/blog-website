title: java lambda
---

# �����ӿ�

## �����ӿڶ���
�����ӿ�ֻ��һ�����󷽷��Ľӿ�,����lambda���ʽ����.

ע��, �������������������Ҫע��ĵط�
1. �����ӿ���һ���ӿ�
2. �����ӿ�����ֻ��һ�����󷽷�
3. �����ӿ�����lambda���ʽ����

## �����ӿ�ʾ��:
```java
// ����һ���Ƿ���û�з���ֵû�в����ĺ����ӿ�
interface Run1 {
	public void runFast();
}
// ����һ���Ƿ���û�з���ֵ�в����ĺ����ӿ�
interface Run2 {
	public void runFast(int seconds);
}
// ����һ���Ƿ����з���ֵ�в����ĺ����ӿ�
interface Run3 {
	public int runFast(int seconds);
}
// ����һ�������з���ֵ�в����ĺ����ӿ�
interface Run4<T> {
	public int runFast(T t, int seconds);
}
```

# lambda���ʽ

## lambda���ʽ����
���������Ǹ������涨��ĺ����ӿ�������һ��lambda���ʽ
```java
// ���������İ汾
Run1 run1 = () -> {
	System.out.println("I am running");
};

// ����Ҫָ��
Run2 run2 = seconds -> {
	System.out.println("I am running " + seconds + " seconds");
};

// ��������汾�ͱ���Ҫ�и�����ֵ��
Run3 run3 = seconds -> {
	System.out.println("I am running");
	return 0;
};

// ����������İ汾��ָ�������ķ�����Ϣ
Run4<String> run4 = (name, seconds) -> {
	System.out.println(name + " is running");
	return 0;
};
```

## lambda���ʽʹ��
����������ʹ�����涨���lambda���ʽ
```java
run1.runFast();
-> I am running

run2.runFast(10);
-> I am running 10 seconds

int result = run3.runFast(10);
-> I am running

run4.runFast("С��", 10); С�� is running
-> 
```

### ע��
��������lambda���ʽ�ⲿ��һ������
```java
String name = "sam";
Run1 run1 = () -> {
	System.out.println(name + " am running");
};
```
��������ͨ��û������,����������ǽ�name��lambda���ʽ�ڲ����¸�ֵ�Ļ�
```java
String name = "sam";
Run1 run1 = () -> {
	name = "";
	System.out.println(name + " am running");
};
```		
����ʾ`variable used in lambda expression shouble be final`, ��˵��lambda��ʵ�ڲ����õ���ֵ�����Ǳ���.��,���������ǻ��ַ�ʽ�ٴ���֤һ�����ǵĽ����
```java
String name = "sam";
name = "Jams";
Run1 run1 = () -> {
	System.out.println(name + " am running");
};
```
ͬ���Ĳ����˱������.

### java����Ҫ�ĺ����ӿ�
* `Predicate<T>`
* `Consumer<T>`
* `Supplier<T>`
* `UnaryOperator<T>`
* `BinaryOperator<T>`