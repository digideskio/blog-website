category: groovy
date: 2014-04-08
title: groovy����
---
> �����Ƕ�Groovy���ֹٷ��ĵ������˷���

Groovy ���Բ����Ͼ�֧�ֶ��ּ�������,����list, map, range. ��������ͼ��϶��ǻ���java�ļ��Ͽ��,����Groovy development kit����Щ�������úܶ��ݷ���.

### Lists

Groovyʹ����һ�ֱ�`[]`������,ֵͨ��`,`�ָ���﷨ ����list. Groovy list ���õ��� JDK��`java.util.List`��ʵ��, ��Ϊ������û�ж����Լ��ļ�����.
Groovy list ��Ĭ��ʵ����`java.util.ArrayList`, �ں������ǿ��Կ���������ʽ��list

```groovy
def numbers = [1, 2, 3]         (1)

assert numbers instanceof List  (2)
assert numbers.size() == 3      (3)
```

1. ���Ƕ�����һ��Number���͵�List,Ȼ�����list�����һ������
2. �ж�list�� Java��s `java.util.List` interface ��ʵ��
3. list�Ĵ�С����ͨ��size()�����в�ѯ, ������Ҳ������չʾ�����listȷʵ����3��Ԫ��

�������list��,����ʹ�õ���ͬ��Ԫ�ص�list, ����ʵGroovy list�е��������ͻ����Բ�һ����
```groovy
def heterogeneous = [1, "a", true]  (1)
```
1. ���Ƕ�����һ��������number,string,boolean �������͵�list

�����������ᵽ��, listʵ������`java.util.ArrayList`ʵ��, ����ʵlist��������������ͬ���͵�ʵ��, ��������ͨ��������������ʽ����������ǿ��ָ�� listʹ�ò�ͬ��Listʵ��
```groovy
def arrayList = [1, 2, 3]
assert arrayList instanceof java.util.ArrayList

def linkedList = [2, 3, 4] as LinkedList    (1)
assert linkedList instanceof java.util.LinkedList

LinkedList otherLinked = [3, 4, 5]          (2)
assert otherLinked instanceof java.util.LinkedList
```
1. ����ʹ�ò�����ǿ�ƽ�������ʽ������Ϊ`java.util.LinkedList`
2. ����ʹ����ʽ������ʽ, ��list����Ϊ`java.util.LinkedList`

���ǿ���ͨ��`[]`�±������������list�е�Ԫ��(��д������). �±������������Ļ�,�Ǿʹ����ҷ���Ԫ��, ����±��Ǹ����Ǿʹ��ҵ������Ԫ��. ���Ǻÿ���ʹ��`<<`��������list��׷��Ԫ��
```groovy
def letters = ['a', 'b', 'c', 'd']

assert letters[0] == 'a'     (1)
assert letters[1] == 'b'

assert letters[-1] == 'd'    (2)
assert letters[-2] == 'c'

letters[2] = 'C'             (3)
assert letters[2] == 'C'

letters << 'e'               (4)
assert letters[ 4] == 'e'
assert letters[-1] == 'e'

assert letters[1, 3] == ['b', 'd']         (5)
assert letters[2..4] == ['C', 'd', 'e']    (6)
```

1. ���ʵ�һ��Ԫ��(������Կ���,list���±��Ǵ�0��ʼ��)
2. ͨ��-1 �±����list�е����һ��Ԫ��.
3. ʹ���±��list�е�����Ԫ�����¸�ֵ
4. ʹ��`<<`��listβ�����һ��Ԫ��
5. һ���Է���list������Ԫ��,��������Ľ���Ƿ���һ����������Ԫ�ص��µ�list
6. ʹ��ֵ���������list��һ����Χ�ڵ�ֵ.

����list֧�ֶ��ֲ�ͬ���͵�Ԫ��, ��ôlist��Ҳ���԰���list,�����Ϳ����������άlist
```groovy
def multi = [[0, 1], [2, 3]]     (1)
assert multi[1][0] == 2          (2)
```

1. ������һ������Number����list��list
2. �������ĵڶ���Ԫ��(�ڶ���list), Ȼ������ڲ�list�ĵ�һ��Ԫ��(�ڶ���list�ĵ�һ��Ԫ��)

#### List literals

�����������������������, ע��`[]`�ǿռ��ϱ��ʽ.
```groovy
def list = [5, 6, 7, 8]
assert list.get(2) == 7
assert list[2] == 7
assert list instanceof java.util.List

def emptyList = []
assert emptyList.size() == 0
emptyList.add(5)
assert emptyList.size() == 1
```groovy

ÿһ��list���ʽ����ʵ����`java.util.List`

��ȻlistҲ����ָ��������ʵ������
```groovy
def list1 = ['a', 'b', 'c']
//construct a new list, seeded with the same items as in list1
def list2 = new ArrayList<String>(list1)

assert list2 == list1 // == checks that each corresponding element is the same

// clone() can also be called
def list3 = list1.clone()
assert list3 == list1
```

list��������һ������Ķ��󼯺�.
```groovy
def list = [5, 6, 7, 8]
assert list.size() == 4
assert list.getClass() == ArrayList     // the specific kind of list being used

assert list[2] == 7                     // indexing starts at 0
assert list.getAt(2) == 7               // equivalent method to subscript operator []
assert list.get(2) == 7                 // alternative method

list[2] = 9
assert list == [5, 6, 9, 8,]           // trailing comma OK

list.putAt(2, 10)                       // equivalent method to [] when value being changed
assert list == [5, 6, 10, 8]
assert list.set(2, 11) == 10            // alternative method that returns old value
assert list == [5, 6, 11, 8]

assert ['a', 1, 'a', 'a', 2.5, 2.5f, 2.5d, 'hello', 7g, null, 9 as byte]
//objects can be of different types; duplicates allowed

assert [1, 2, 3, 4, 5][-1] == 5             // use negative indices to count from the end
assert [1, 2, 3, 4, 5][-2] == 4
assert [1, 2, 3, 4, 5].getAt(-2) == 4       // getAt() available with negative index...
try {
    [1, 2, 3, 4, 5].get(-2)                 // but negative index not allowed with get()
    assert false
} catch (e) {
    assert e instanceof ArrayIndexOutOfBoundsException
}
```

#### List as a boolean expression

list�����Լ����boolean���ʽ.
```groovy
assert ![]             // an empty list evaluates as false

//all other lists, irrespective of contents, evaluate as true
assert [1] && ['a'] && [0] && [0.0] && [false] && [null]
```

#### Iterating on a list

����ͨ��`each`, `eachWithIndex`������������.
```groovy
[1, 2, 3].each {
    println "Item: $it" // `it` is an implicit parameter corresponding to the current element
}
['a', 'b', 'c'].eachWithIndex { it, i -> // `it` is the current element, while `i` is the index
    println "$i: $it"
}
```

�ڱ�����ʱ��,���Ǿ�����Ҫ������������ֵ����ĳЩ����,Ȼ�������·Ž�һ���µ�list��. ���ֲ���������Ϊӳ��(mapping), ���ֲ���ͨ��`collect`����ʵ��.
```groovy
assert [1, 2, 3].collect { it * 2 } == [2, 4, 6]

// shortcut syntax instead of collect
assert [1, 2, 3]*.multiply(2) == [1, 2, 3].collect { it.multiply(2) }

def list = [0]
// it is possible to give `collect` the list which collects the elements
assert [1, 2, 3].collect(list) { it * 2 } == [0, 2, 4, 6]
assert list == [0, 2, 4, 6]
```

#### Manipulating lists

##### Filtering and searching

[Groovy development kit](http://www.groovy-lang.org/gdk.html)�ṩ�����ǿ����Ȥ�ķ�������ǿ����׼����:

```groovy
assert [1, 2, 3].find { it > 1 } == 2           // find 1st element matching criteria
assert [1, 2, 3].findAll { it > 1 } == [2, 3]   // find all elements matching critieria
assert ['a', 'b', 'c', 'd', 'e'].findIndexOf {      // find index of 1st element matching criteria
    it in ['c', 'e', 'g']
} == 2

assert ['a', 'b', 'c', 'd', 'c'].indexOf('c') == 2  // index returned
assert ['a', 'b', 'c', 'd', 'c'].indexOf('z') == -1 // index -1 means value not in list
assert ['a', 'b', 'c', 'd', 'c'].lastIndexOf('c') == 4

assert [1, 2, 3].every { it < 5 }               // returns true if all elements match the predicate
assert ![1, 2, 3].every { it < 3 }
assert [1, 2, 3].any { it > 2 }                 // returns true if any element matches the predicate
assert ![1, 2, 3].any { it > 3 }

assert [1, 2, 3, 4, 5, 6].sum() == 21                // sum anything with a plus() method
assert ['a', 'b', 'c', 'd', 'e'].sum {
    it == 'a' ? 1 : it == 'b' ? 2 : it == 'c' ? 3 : it == 'd' ? 4 : it == 'e' ? 5 : 0
    // custom value to use in sum
} == 15
assert ['a', 'b', 'c', 'd', 'e'].sum { ((char) it) - ((char) 'a') } == 10
assert ['a', 'b', 'c', 'd', 'e'].sum() == 'abcde'
assert [['a', 'b'], ['c', 'd']].sum() == ['a', 'b', 'c', 'd']

// an initial value can be provided
assert [].sum(1000) == 1000
assert [1, 2, 3].sum(1000) == 1006

assert [1, 2, 3].join('-') == '1-2-3'           // String joining
assert [1, 2, 3].inject('counting: ') {
    str, item -> str + item                     // reduce operation
} == 'counting: 123'
assert [1, 2, 3].inject(0) { count, item ->
    count + item
} == 6
```

������δ�������Groovy����֧�ŵ��ڼ������ҵ�������С��������:
```groovy
def list = [9, 4, 2, 10, 5]
assert list.max() == 10
assert list.min() == 2

// we can also compare single characters, as anything comparable
assert ['x', 'y', 'a', 'z'].min() == 'a'

// we can use a closure to specify the sorting behaviour
def list2 = ['abc', 'z', 'xyzuvw', 'Hello', '321']
assert list2.max { it.size() } == 'xyzuvw'
assert list2.min { it.size() } == 'z'
```

�ڱհ���,�㻹�����Զ���һ���ȽϹ���.
```groovy
Comparator mc = { a, b -> a == b ? 0 : (a < b ? -1 : 1) }

def list = [7, 4, 9, -6, -1, 11, 2, 3, -9, 5, -13]
assert list.max(mc) == 11
assert list.min(mc) == -13

Comparator mc2 = { a, b -> a == b ? 0 : (Math.abs(a) < Math.abs(b)) ? -1 : 1 }


assert list.max(mc2) == -13
assert list.min(mc2) == -1

assert list.max { a, b -> a.equals(b) ? 0 : Math.abs(a) < Math.abs(b) ? -1 : 1 } == -13
assert list.min { a, b -> a.equals(b) ? 0 : Math.abs(a) < Math.abs(b) ? -1 : 1 } == -1
```

##### Adding or removing elements

���ǿ���ʹ��`[]`ȥ����һ���µĿ�list, Ȼ��ʹ��`<<`��list׷��Ԫ��
```groovy
def list = []
assert list.empty

list << 5
assert list.size() == 1

list << 7 << 'i' << 11
assert list == [5, 7, 'i', 11]

list << ['m', 'o']
assert list == [5, 7, 'i', 11, ['m', 'o']]

//first item in chain of << is target list
assert ([1, 2] << 3 << [4, 5] << 6) == [1, 2, 3, [4, 5], 6]

//using leftShift is equivalent to using <<
assert ([1, 2, 3] << 4) == ([1, 2, 3].leftShift(4))
```groovy
We can add to a list in many ways:
```groovy
assert [1, 2] + 3 + [4, 5] + 6 == [1, 2, 3, 4, 5, 6]
// equivalent to calling the `plus` method
assert [1, 2].plus(3).plus([4, 5]).plus(6) == [1, 2, 3, 4, 5, 6]

def a = [1, 2, 3]
a += 4      // creates a new list and assigns it to `a`
a += [5, 6]
assert a == [1, 2, 3, 4, 5, 6]

assert [1, *[222, 333], 456] == [1, 222, 333, 456]
assert [*[1, 2, 3]] == [1, 2, 3]
assert [1, [2, 3, [4, 5], 6], 7, [8, 9]].flatten() == [1, 2, 3, 4, 5, 6, 7, 8, 9]

def list = [1, 2]
list.add(3)
list.addAll([5, 4])
assert list == [1, 2, 3, 5, 4]

list = [1, 2]
list.add(1, 3) // add 3 just before index 1
assert list == [1, 3, 2]

list.addAll(2, [5, 4]) //add [5,4] just before index 2
assert list == [1, 3, 5, 4, 2]

list = ['a', 'b', 'z', 'e', 'u', 'v', 'g']
list[8] = 'x' // the [] operator is growing the list as needed
// nulls inserted if required
assert list == ['a', 'b', 'z', 'e', 'u', 'v', 'g', null, 'x']
```

��list��`+`�����岢û�з����仯,���Ǻεȵ���Ҫ��~~~ ��`<<`���, `+`�ᴴ��һ���µ�list,  �������������list�ܿ��ܲ�������Ԥ�ڵ�, �������ַ�ʽҲ���ܻᵼ��һЩ��������.

`Groovy development kit`ͬ���ṩ�˺ܶ��ݵķ�ʽ��list��ɾ��Ԫ��:
```groovy
assert ['a','b','c','b','b'] - 'c' == ['a','b','b','b']
assert ['a','b','c','b','b'] - 'b' == ['a','c']
assert ['a','b','c','b','b'] - ['b','c'] == ['a']

def list = [1,2,3,4,3,2,1]
list -= 3           // creates a new list by removing `3` from the original one
assert list == [1,2,4,2,1]
assert ( list -= [2,4] ) == [1,1]
```
ͬ��,��Ҳ��ͨ�������ķ�ʽ��list��ɾ��Ԫ��.
```groovy
def list = [1,2,3,4,5,6,2,2,1]
assert list.remove(2) == 3          // remove the third element, and return it
assert list == [1,2,4,5,6,2,2,1]
```
����,�������list��ɾ�������ͬԪ���еĵ�һ��, ������Ե���`remove`����.
```groovy
def list= ['a','b','c','b','b']
assert list.remove('c')             // remove 'c', and return true because element removed
assert list.remove('b')             // remove first 'b', and return true because element removed

assert ! list.remove('z')           // return false because no elements removed
assert list == ['a','b','b']
```
�������Ҫ��list��յĻ�,ֻ��Ҫ����`clear`��������
```groovy
def list= ['a',2,'c',4]
list.clear()
assert list == []
```

##### Set operations

`Groovy development kit`�������ܶ��߼�����ķ���
```groovy
assert 'a' in ['a','b','c']             // returns true if an element belongs to the list
assert ['a','b','c'].contains('a')      // equivalent to the `contains` method in Java
assert [1,3,4].containsAll([1,4])       // `containsAll` will check that all elements are found

assert [1,2,3,3,3,3,4,5].count(3) == 4  // count the number of elements which have some value
assert [1,2,3,3,3,3,4,5].count {
    it%2==0                             // count the number of elements which match the predicate
} == 2

assert [1,2,4,6,8,10,12].intersect([1,3,6,9,12]) == [1,6,12]

assert [1,2,3].disjoint( [4,6,9] )
assert ![1,2,3].disjoint( [2,4,6] )
```

##### Sorting

Groovy���ṩ�˺ܶ�ʹ�ñհ��Ƚ������������
```groovy
assert [6, 3, 9, 2, 7, 1, 5].sort() == [1, 2, 3, 5, 6, 7, 9]

def list = ['abc', 'z', 'xyzuvw', 'Hello', '321']
assert list.sort {
    it.size()
} == ['z', 'abc', '321', 'Hello', 'xyzuvw']

def list2 = [7, 4, -6, -1, 11, 2, 3, -9, 5, -13]
assert list2.sort { a, b -> a == b ? 0 : Math.abs(a) < Math.abs(b) ? -1 : 1 } ==
        [-1, 2, 3, 4, 5, -6, 7, -9, 11, -13]

Comparator mc = { a, b -> a == b ? 0 : Math.abs(a) < Math.abs(b) ? -1 : 1 }

// JDK 8+ only
// list2.sort(mc)
// assert list2 == [-1, 2, 3, 4, 5, -6, 7, -9, 11, -13]

def list3 = [6, -3, 9, 2, -7, 1, 5]

Collections.sort(list3)
assert list3 == [-7, -3, 1, 2, 5, 6, 9]

Collections.sort(list3, mc)
assert list3 == [1, 2, -3, 5, 6, -7, 9]
```

##### Duplicating elements

`roovy development kit`��ͨ�����ز������ķ�ʽ, �ڲ��ṩ��һЩ��������listԪ�ظ���.
```groovy
assert [1, 2, 3] * 3 == [1, 2, 3, 1, 2, 3, 1, 2, 3]
assert [1, 2, 3].multiply(2) == [1, 2, 3, 1, 2, 3]
assert Collections.nCopies(3, 'b') == ['b', 'b', 'b']

// nCopies from the JDK has different semantics than multiply for lists
assert Collections.nCopies(2, [1, 2]) == [[1, 2], [1, 2]] //not [1,2,1,2]
```

### Arrays

Groovy ����������list����, ���������Ҫ��������, ��ô�ͱ���ǿ�Ƶ���ʽ������������
```groovy
String[] arrStr = ['Ananas', 'Banana', 'Kiwi']  (1)

assert arrStr instanceof String[]    (2)
assert !(arrStr instanceof List)

def numArr = [1, 2, 3] as int[]      (3)

assert numArr instanceof int[]       (4)
assert numArr.size() == 3
```

1. ʹ����ʽ�������Ͷ�����һ���ַ�������
2. ���ԸղŴ����������Ƿ���string����
3. ʹ�ò���������һ��int����
4. ���ԸղŴ����������Ƿ���int����

����Ҳ���Դ�����һ����ά����
```groovy
def matrix3 = new Integer[3][3]         (1)
assert matrix3.size() == 3

Integer[][] matrix2                     (2)
matrix2 = [[1, 2], [3, 4]]
assert matrix2 instanceof Integer[][]
```
1. ����ָ����������ı߽�
2. ��Ȼ����Ҳ���Բ�ָ�����ı߽�

��������Ԫ�غͷ���listԪ�صķ�ʽ��ͬ
```groovy
String[] names = ['C��dric', 'Guillaume', 'Jochen', 'Paul']
assert names[0] == 'C��dric'     (1)

names[2] = 'Blackdrag'          (2)
assert names[2] == 'Blackdrag'
```
1	Retrieve the first element of the array
2	Set the value of the third element of the array to a new value
1. ���������е�һ��Ԫ��
2. �������е�����Ԫ�����¸�ֵ

Groovy��֧��Java�����ʼ���﷨, ��ΪJava�����еĻ����ſ��ܱ���Groovy�޽�ɱհ�

### Maps
��ʱ�����������������г�mapΪ �ֵ���߹�������. Map��key��value��������, ��Groovy��map��`[]`������, ͨ��`,`�ָ��ֵ��, ��ֵͨ��`:`�ָ�
```groovy
def colors = [red: '#FF0000', green: '#00FF00', blue: '#0000FF']   (1)

assert colors['red'] == '#FF0000'    (2)
assert colors.green  == '#00FF00'    (3)

colors['pink'] = '#FF00FF'           (4)
colors.yellow  = '#FFFF00'           (5)

assert colors.pink == '#FF00FF'
assert colors['yellow'] == '#FFFF00'

assert colors instanceof java.util.LinkedHashMap
```

1. ���Ƕ�����һ��string���͵Ĵ�����ɫ���ֵ�����,
2. Ȼ��ʹ���±�������map���Ƿ����red���key
3. ���ǻ�����ֱ��ʹ��`.`��������ĳ��key
4. ���ǿ���ʹ���±���map�����һ���µļ�ֵ��
5. ����Ҳ����ʹ��`.`���һ���µļ�ֵ��

Groovy������map����Ĭ�ϵ���`java.util.LinkedHashMap`

������Ҫ����һ�������ڵ�keyʱ��
```groovy
assert colors.unknown == null
```
�㽫������һ��null�Ľ��

�����������������ʹ�õ�����string��Ϊkey, �����㻹����ʹ������������Ϊmap��key��

```groovy
def numbers = [1: 'one', 2: 'two']

assert numbers[1] == 'one'
```

����ʹ����number��Ϊ��map�µ�key����, number���;ͻ�ֱ�ӱ�����Ϊnumber����, ���Groovy��������ǰ��������һ��string���͵�key. ���Ǽ�������Ҫ����һ��������Ϊkey,�Ǳ�����ֵ��Ϊkey��

```groovy
def key = 'name'
def person = [key: 'Guillaume']      (1)

assert !person.containsKey('name')   (2)
assert person.containsKey('key')     (3)
```
1. ��`\'Guillaume'` ������keyʵ������`"key"`����ַ���, ���������key������ֵ`'name'`
2. map�в�����`'name'`key
3. ȡ����֮����map�а���һ��`"key"`���ַ���

�������map�д���һ�������ַ�����Ϊkey,����`["name": "Guillaume"]`.

```groovy
person = [(key): 'Guillaume']        (1)

assert person.containsKey('name')    (2)
assert !person.containsKey('key')    (3)
```
1	This time, we surround the key variable with parentheses, to instruct the parser we are passing a variable rather than defining a string key
2	The map does contain the name key
3	But the map doesn��t contain the key key as before
1.
2.
3.

#### Map literals

��Groovy�п���ʹ��`[:]` ����һ��map.
```groovy
def map = [name: 'Gromit', likes: 'cheese', id: 1234]
assert map.get('name') == 'Gromit'
assert map.get('id') == 1234
assert map['name'] == 'Gromit'
assert map['id'] == 1234
assert map instanceof java.util.Map

def emptyMap = [:]
assert emptyMap.size() == 0
emptyMap.put("foo", 5)
assert emptyMap.size() == 1
assert emptyMap.get("foo") == 5
```

Map��keyĬ����`string`, ����`[a:1]`��ͬ��`['a':1]`. �Ƚ���������ɻ�ľ���,����㴴����һ������a(ֵΪb), �����㽫����a`put`��map��, map��key����a,������b. ������������������Ļ�,��ô������ʹ��`()`key����ת����.
```groovy
def a = 'Bob'
def ages = [a: 43]
assert ages['Bob'] == null // `Bob` is not found
assert ages['a'] == 43     // because `a` is a literal!

ages = [(a): 43]            // now we escape `a` by using parenthesis
assert ages['Bob'] == 43   // and the value is found!
```

ͨ������ķ�ʽ��������ɿ�¡һ��map
```groovy
def map = [
        simple : 123,
        complex: [a: 1, b: 2]
]
def map2 = map.clone()
assert map2.get('simple') == map.get('simple')
assert map2.get('complex') == map.get('complex')
map2.get('complex').put('c', 3)
assert map.get('complex').get('c') == 3
```

#### Map property notation

Maps��beansҲ�Ƿǳ������, ��������Զ�mapʹ��`get/set`����Ԫ��,��Ȼ��Ҳ�и�ǰ��,�Ǿ���map�е�key�����Ƿ���Groovy��ʶ����key.

```groovy
def map = [name: 'Gromit', likes: 'cheese', id: 1234]
assert map.name == 'Gromit'     // can be used instead of map.get('Gromit')
assert map.id == 1234

def emptyMap = [:]
assert emptyMap.size() == 0
emptyMap.foo = 5
assert emptyMap.size() == 1
assert emptyMap.foo == 5
```

ע��:`map.foo`���ǻ���map�в���key`foo`. ����ζ��,
```groovy
def map = [name: 'Gromit', likes: 'cheese', id: 1234]
assert map.class == null
assert map.get('class') == null
assert map.getClass() == LinkedHashMap // this is probably what you want

map = [1      : 'a',
       (true) : 'p',
       (false): 'q',
       (null) : 'x',
       'null' : 'z']
assert map.containsKey(1) // 1 is not an identifier so used as is
assert map.true == null
assert map.false == null
assert map.get(true) == 'p'
assert map.get(false) == 'q'
assert map.null == 'z'
assert map.get(null) == 'x'
```

#### Iterating on maps

`Groovy development kit`���ṩ��`eachWithIndex`��������map.ֵ��ע�����,map�ᱣ��putԪ�ص�˳��,Ҳ����˵,�������һ��map��ʱ��,���۽��ж��ٴ�,���õ�Ԫ�ص�˳����һ����.
```groovy
def map = [
        Bob  : 42,
        Alice: 54,
        Max  : 33
]

// `entry` is a map entry
map.each { entry ->
    println "Name: $entry.key Age: $entry.value"
}

// `entry` is a map entry, `i` the index in the map
map.eachWithIndex { entry, i ->
    println "$i - Name: $entry.key Age: $entry.value"
}

// Alternatively you can use key and value directly
map.each { key, value ->
    println "Name: $key Age: $value"
}

// Key, value and i as the index in the map
map.eachWithIndex { key, value, i ->
    println "$i - Name: $key Age: $value"
}
```

#### Manipulating maps

##### Adding or removing elements

��map�����Ԫ�������ʹ��`put`����, `�±�`, `putAll`����.
```groovy
def defaults = [1: 'a', 2: 'b', 3: 'c', 4: 'd']
def overrides = [2: 'z', 5: 'x', 13: 'x']

def result = new LinkedHashMap(defaults)
result.put(15, 't')
result[17] = 'u'
result.putAll(overrides)
assert result == [1: 'a', 2: 'z', 3: 'c', 4: 'd', 5: 'x', 13: 'x', 15: 't', 17: 'u']
```

�����Ҫɾ��map��ȫ����Ԫ��,����ʹ��`clear`����.
```groovy
def m = [1:'a', 2:'b']
assert m.get(1) == 'a'
m.clear()
assert m == [:]
```

ͨ��map��������Ǵ�����map��ʹ��`object`��`equals`������`hashcode`����.

��Ҫע�����,��Ҫʹ��GString��Ϊmap��key, ��ΪGString��hashcode������String��hashcode������һ��.
```groovy
def key = 'some key'
def map = [:]
def gstringKey = "${key.toUpperCase()}"
map.put(gstringKey,'value')
assert map.get('SOME KEY') == null
```

##### Keys, values and entries

���ǿ�������ͼ��inspect`keys, values, and entries`
```groovy
def map = [1:'a', 2:'b', 3:'c']

def entries = map.entrySet()
entries.each { entry ->
  assert entry.key in [1,2,3]
  assert entry.value in ['a','b','c']
}

def keys = map.keySet()
assert keys == [1,2,3] as Set
```

Mutating values returned by the view (be it a map entry, a key or a value) is highly discouraged because success of the operation directly depends on the type of the map being manipulated. In particular, Groovy relies on collections from the JDK that in general make no guarantee that a collection can safely be manipulated through keySet, entrySet, or values.


##### Filtering and searching

The Groovy development kit contains filtering, searching and collecting methods similar to those found for lists:

```groovy
def people = [
    1: [name:'Bob', age: 32, gender: 'M'],
    2: [name:'Johnny', age: 36, gender: 'M'],
    3: [name:'Claire', age: 21, gender: 'F'],
    4: [name:'Amy', age: 54, gender:'F']
]

def bob = people.find { it.value.name == 'Bob' } // find a single entry
def females = people.findAll { it.value.gender == 'F' }

// both return entries, but you can use collect to retrieve the ages for example
def ageOfBob = bob.value.age
def agesOfFemales = females.collect {
    it.value.age
}

assert ageOfBob == 32
assert agesOfFemales == [21,54]

// but you could also use a key/pair value as the parameters of the closures
def agesOfMales = people.findAll { id, person ->
    person.gender == 'M'
}.collect { id, person ->
    person.age
}
assert agesOfMales == [32, 36]

// `every` returns true if all entries match the predicate
assert people.every { id, person ->
    person.age > 18
}

// `any` returns true if any entry matches the predicate

assert people.any { id, person ->
    person.age == 54
}
```

##### Grouping

We can group a list into a map using some criteria:

```groovy
assert ['a', 7, 'b', [2, 3]].groupBy {
    it.class
} == [(String)   : ['a', 'b'],
      (Integer)  : [7],
      (ArrayList): [[2, 3]]
]

assert [
        [name: 'Clark', city: 'London'], [name: 'Sharma', city: 'London'],
        [name: 'Maradona', city: 'LA'], [name: 'Zhang', city: 'HK'],
        [name: 'Ali', city: 'HK'], [name: 'Liu', city: 'HK'],
].groupBy { it.city } == [
        London: [[name: 'Clark', city: 'London'],
                 [name: 'Sharma', city: 'London']],
        LA    : [[name: 'Maradona', city: 'LA']],
        HK    : [[name: 'Zhang', city: 'HK'],
                 [name: 'Ali', city: 'HK'],
                 [name: 'Liu', city: 'HK']],
]
```

### Ranges

Ranges allow you to create a list of sequential values. These can be used as List since Range extends java.util.List.

Ranges defined with the .. notation are inclusive (that is the list contains the from and to value).

Ranges defined with the ..< notation are half-open, they include the first value but not the last value.

```groovy
// an inclusive range
def range = 5..8
assert range.size() == 4
assert range.get(2) == 7
assert range[2] == 7
assert range instanceof java.util.List
assert range.contains(5)
assert range.contains(8)

// lets use a half-open range
range = 5..<8
assert range.size() == 3
assert range.get(2) == 7
assert range[2] == 7
assert range instanceof java.util.List
assert range.contains(5)
assert !range.contains(8)

//get the end points of the range without using indexes
range = 1..10
assert range.from == 1
assert range.to == 10
```

Note that int ranges are implemented efficiently, creating a lightweight Java object containing a from and to value.

Ranges can be used for any Java object which implements java.lang.Comparable for comparison and also have methods next() and previous() to return the next / previous item in the range. For example, you can create a range of String elements:

