category: groovy
date: 2014-04-08
title: groovy JSON
---
> �����Ƕ�Groovy���ֹٷ��ĵ������˷���

Groovy ԭ��֧��Groovy�����JSON֮���ת��. `groovy.json`���ڵ�������JSON�����л��ͽ�������

## JsonSlurper

`JsonSlurper`���ڽ�JSON�ı����������������ݽ�����Groovy������ݽṹ,����`maps</code>, `lists</code>, ��������ԭ���������� `Integer</code>, `Double</code>, `Boolean</code>, `String`��

����������˺ܶ෽��, ���һ������һЩ����ķ���, ����`parseText</code>, `parseFile` ��.�����������������ʹ���� `parseText` ����, �������һ��JSON�ַ���, Ȼ��ݹ�ؽ���ת����`list</code>, `map`�ṹ. һЩ������`parse*</code> �������������������, ��������JSON�ַ���, ֻ���������ķ������ܵĲ�����һ��.

```groovy
def jsonSlurper = new JsonSlurper()
def object = jsonSlurper.parseText('{ "name": "John Doe" } /* some comment */')

assert object instanceof Map
assert object.name == 'John Doe'
```

��Ҫע�����, �����Ľ����һ����map, ������һ����ͨ��Groovy����ʵ��������. `JsonSlurper`����[ECMA-404 JSON Interchange Standard](http://www.ecma-international.org/publications/files/ECMA-ST/ECMA-404.pdf)����������JSON, ͬʱ֧��JavaScript��ע�ͺ�ʱ������.

����֧��maps֮��, `JsonSlurper` ��֧�ֽ�JSON���������list�Ĺ���
```groovy
def jsonSlurper = new JsonSlurper()
def object = jsonSlurper.parseText('{ "myList": [4, 8, 15, 16, 23, 42] }')

assert object instanceof Map
assert object.myList instanceof List
assert object.myList == [4, 8, 15, 16, 23, 42]
```

JSON��׼��ֻ֧��������Щԭ���������ͣ�`string</code>, `number</code>, `object</code>, `true</code>, `false</code>, `null</code>. `JsonSlurper` ����ЩJSON����ת����Groovy����.
```groovy
def jsonSlurper = new JsonSlurper()
def object = jsonSlurper.parseText '''
    { "simple": 123,
      "fraction": 123.66,
      "exponential": 123e12
    }'''

assert object instanceof Map
assert object.simple.class == Integer
assert object.fraction.class == BigDecimal
assert object.exponential.class == BigDecimal
```

`JsonSlurper` ���ɵĽ�����Ǵ�Groovy����ʵ��, �����ڲ���������κε�JSON��ص������, �����÷����൱͸����. ��ʵ��`JsonSlurper`�Ľ����ѭ`GPath`���ʽ. `GPath`��һ���ǳ�ǿ��ı��ʽ����, ��֧�ֶ��ֲ�ͬ�����ݸ�ʽ(����`XmlSlurper`֧��`XML` ��������һ������)

�����Ҫ�˽���������, �����ֱ��ȥ[GPath expressions](http://docs.groovy-lang.org/latest/html/documentation/core-semantics.html#gpath_expressions)��һ��.
���������JSON������Groovy��������֮��Ķ�Ӧ��ϵ.
```groovy
JSON			Groovy
string			java.lang.String
number			java.lang.BigDecimal or java.lang.Integer
object			java.util.LinkedHashMap
array			java.util.ArrayList
true			true
false			false
null			null
date			java.util.Date based on the yyyy-MM-dd��T��HH:mm:ssZ date format
```

���JSON�е�һ��ֵ��`null</code>, `JsonSlurper`֧����ת����Groovy�е�`null</code>.���������JSON�������γ��˶Ա�, ����һ����ֵ����ṩ�ĵ�һ����

### Parser Variants

Groovy �ж��`JsonSlurper` ������ʵ��. ÿһ������������Ӧ�Ų�ͬ������, ÿһ���ض��Ľ������ܺܺõĴ����ض�����, ����Ĭ�ϵĽ�������������Ӧ�����е����. ����ͶԸ����������������:

`JsonParserCharArray` ����������һ��JSON�ַ���, Ȼ�����ڲ�ʹ��һ���ֽ�������н���. During value conversion it copies character sub-arrays (a mechanism known as "chopping") and operates on them.


* `JsonFastParser`��������`JsonParserCharArray`�������ı���, �������Ľ�����. ������������,���ǻ���ĳЩԭ��,��������Ĭ�ϵĽ�����. `JsonFastParser`������Ҳ����Ϊ��������(index-overlay)������. ����������JSON�ַ�����ʱ��,�ý������Ἣ�����ⴴ���µ��ֽ���������ַ���ʵ��. ��һֱָ��ԭ�����ֽ����顣 ����, ���ᾡ���ܵ��Ƴٶ���Ĵ���. If parsed maps are put into long-term caches care must be taken as the map objects might not be created and still consist of pointer to the original char buffer only. `JsonFastParser`��ȡ��һ��������и�ģ��, ���ᾡ��طָ�char buffer, �Ա���ά��һ�ݶ�ԭ��buffer�Ƚ�С�Ŀ���. �������ʹ��`JsonFastParser</code>, ��ô����Ľ����Ǳ���`JsonFastParser`��JSON buffer��2MB����, ����ʱ��Ҫ���ֳ��ڻ�������.

* `JsonParserLax` ��`JsonFastParser`��һ������ʵ��. ����`JsonFastParser` ��һЩ���Ƶ������ص�, ���ǲ�ͬ���������ǽ�������`ECMA-404 JSON grammar</code>. ����,��������������֧�ֲ������ŵ��ַ���ע��.

`JsonParserUsingCharacterSource` ���ڽ����ǳ�����ļ�. ��ʹ��һ�ֳ�Ϊ<code>"character windowing"</code>�ļ���ȥ�����ǳ���(����2MB)��JSON�ļ�,����������Ҳ�ǳ��ȶ�

`JsonSlurper`��Ĭ��ʵ���� `JsonParserCharArray</code>.  `JsonParserType`�����˽����������ö������:

```groovy
Implementation					Constant
JsonParserCharArray				JsonParserType#CHAR_BUFFER
JsonFastParser					JsonParserType#INDEX_OVERLAY
JsonParserLax					JsonParserType#LAX
JsonParserUsingCharacterSource	JsonParserType#CHARACTER_SOURCE
```

�����Ҫ�ı��������ʵ��Ҳ�ǳ���, ֻ��Ҫͨ������`JsonSlurper#setType()</code>������`JsonParserType`�����ϲ�ͬ��ֵ�Ϳ�����

```groovy
def jsonSlurper = new JsonSlurper(type: JsonParserType.INDEX_OVERLAY)
def object = jsonSlurper.parseText('{ "myList": [4, 8, 15, 16, 23, 42] }')

assert object instanceof Map
assert object.myList instanceof List
assert object.myList == [4, 8, 15, 16, 23, 42]
```

### JsonOutput

`JsonOutput`���ڽ�Groovy�������л���JSON�ַ���. 

`JsonOutput` ������`toJson`��̬����. ÿ����ͬ��`toJson`�����������һ����ͬ�Ĳ�������. 

`toJson`�������ص���һ������JSOn��ʽ���ַ���
```groovy
def json = JsonOutput.toJson([name: 'John Doe', age: 42])

assert json == '{"name":"John Doe","age":42}'
```

`JsonOutput`����֧��ԭ������, map, list���������л���JSON, ������֧�����л�`POGOs</code>(һ�ֱȽ��ϵ�Groovy����)
```groovy
class Person { String name }

def json = JsonOutput.toJson([ new Person(name: 'John'), new Person(name: 'Max') ])

assert json == '[{"name":"John"},{"name":"Max"}]'
```

�ղ��Ǹ�������, JSON���Ĭ��û�н���pretty���. ���`JsonSlurper`���ṩ��`prettyPrint`����
```groovy
def json = JsonOutput.toJson([name: 'John Doe', age: 42])

assert json == '{"name":"John Doe","age":42}'

assert JsonOutput.prettyPrint(json) == '''\
{
    "name": "John Doe",
    "age": 42
}'''.stripIndent()
```

`prettyPrint`����ֻ����һ��String���͵��ַ���, �����ܺ�`JsonOutput`�������ķ�ʽ�������ʹ��, it can be applied on arbitrary JSON String instances.

��Groovy�л�����ʹ��`JsonBuilder</code>, `StreamingJsonBuilder`��ʽ����JSON. �������������ṩ��һ��`DSL</code>, ������������һ��JSON��ʱ��,�����ƶ�һ������ͼ.


```groovy
// an inclusive range
def range = 'a'..'d'
assert range.size() == 4
assert range.get(2) == 'c'
assert range[2] == 'c'
assert range instanceof java.util.List
assert range.contains('a')
assert range.contains('d')
assert !range.contains('e')
```

You can iterate on a range using a classic for loop:

```groovy
for (i in 1..10) {
    println "Hello ${i}"
}
```

but alternatively you can achieve the same effect in a more Groovy idiomatic style, by iterating a range with each method:

```groovy
(1..10).each { i ->
    println "Hello ${i}"
}
```

Ranges can be also used in the switch statement:

```
switch (years) {
    case 1..10: interestRate = 0.076; break;
    case 11..25: interestRate = 0.052; break;
    default: interestRate = 0.037;
}
```