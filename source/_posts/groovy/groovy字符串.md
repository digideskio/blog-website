category: groovy
date: 2014-04-08
title: groovy�ַ���
---
> �����Ƕ�Groovy���ֹٷ��ĵ������˷���

��Groovy�ı�����������ΪString,�������ַ�������ʽ���ֵ�. Groovy������ʵ����`java.lang.String`,��  GStrings (`groovy.lang.GString`)����, (GString������Ϊ��ֵ�ַ���)

### �������ַ�
�������ַ�����ͨ����������������һ���ַ�
```groovy
'a single quoted string'
```

�������ַ���`java.lang.String`��ͬһ������, ͬʱ��Ҳ�������ֵ�ĳ���
### �ַ�������

Groovy�����е��ַ���������ͨ�� `+` ��������
```groovy
assert 'ab' == 'a' + 'b'
```

### ���ص������ַ���

���ص������ַ��� ��ͨ������������ ��Χ�������ַ�����.
```groovy
'''a triple single quoted string'''
```
���ص������ַ������Ǵ�`java.lang.String` ���Ҳ������ֵ.
���ص������ַ������Զ��и�ֵ.
```groovy
def aMultilineString = '''line one
line two
line three'''
```

�����Ĵ������������, �������еķ�����, �ǿ��е����ص������ַ���Ҳ���������. �������Ե���`String#stripIndent()` ȥ��������. `String#stripMargin()`������ͨ���ָ�����ַ����Ŀ�ͷ
```groovy
def startingAndEndingWithANewline = '''
line one
line two
line three
'''
```

��Ҳ���ע�⵽���յõ����ַ��������һ�����з�.It is possible to strip that character by escaping the newline with a backslash:
```groovy
def strippedFirstNewline = '''\
line one
line two
line three
'''

assert !strippedFirstNewline.startsWith('\n')
```

#### ���������ַ�

����ͨ��`\`�ַ���`''`��������`'`
```groovy
'an escaped single quote: \' needs a backslash'
```

��ȻҲ����ͨ��`\`������������
```groovy
'an escaped escape character: \\ needs a double backslash'
```

����һЩ�����������ַ���Ҫ`\`������
```groovy
Escape sequence	Character
'\t'	tabulation
'\b'	backspace
'\n'	newline
'\r'	carriage return
'\f'	formfeed
'\\'	backslash
'\''	single quote (for single quoted and triple single quoted strings)
'\"'	double quote (for double quoted and triple double quoted strings)
```
#### Unicode ת������

��һЩ�ַ�������ͨ���������, ��ô��ʱ�Ϳ���ͨ��Unicode ת��������ʵ��. ����`backslash`, ��u���4��16�������ּ���.

```groovy
'The Euro currency symbol: \u20AC'
```
### ˫���Ű����� string

ͨ��˫���Ű����������ַ���
```groovy
"a double quoted string"
```
��˫�����ַ�����û�в�ֵ(`${}`)��ʱ��, �����͵�ͬ��`java.lang.String`, ���в�ֵ��ʱ����ô˫�����ַ�������`groovy.lang.GString`��ʵ��

#### String ��ֵ

�κα��ʽ������Ƕ�뵽���˵����ź������ŵ������ַ���������. �����ַ�����ֵ��ʱ��, ��ֵ��ʹ������ֵ���滻���ַ������ռλ��. ռλ�����ʽͨ��`${}` ���� `$`��ʵ��. ռλ����ı��ʽֵ�ᱻת�������ַ�����ʾ��ʽ, ת����ͨ�����ñ��ʽ`toString()`����,ͨ������һ��String����.

���������չʾ�����ַ������ռλ����λ���ر���
```groovy
def name = 'Guillaume' // a plain string
def greeting = "Hello ${name}"

assert greeting.toString() == 'Hello Guillaume'
```

���ǲ������еı��ʽ���ǺϷ���, �����������оٵ�����������ʽ

```groovy
def sum = "The sum of 2 and 3 equals ${2 + 3}"
assert sum.toString() == 'The sum of 2 and 3 equals 5'
```

��ʵ������ֻ�б��ʽ���������`${}`���ʽ��. Statements ͬ��������`${}` ռλ�������, ����statement��ֵ����null. �����N��statements������`${}`��,��ô���һ��statementӦ�÷���һ����Чֵ,�Ա㱻���뵽�ַ�����. ����`"The sum of 1 and 2 is equal to ${def a = 1; def b = 2; a + b}"` �������,����Ҳ�����﷨Ԥ�ڵ�����ִ��, ����ϰ����,GString ռλ����Ӧ�ø������ʹ�ü򵥱��ʽ.
����` ${}`ռλ��֮��, ����Ҳ����ʹ��`$`���ǰ׺��׺���ʽ��

```groovy
def person = [name: 'Guillaume', age: 36]
assert "$person.name is $person.age years old" == 'Guillaume is 36 years old'
```
���ǽ���һ����ʽ�ĵ�׺���ʽ�ǺϷ��ģ�a.b, a.b.c,etc.������Щ�������ŵı��ʽ(���緽������,������Ϊ�հ�,���������)����Ч��.
���������һ�������������ʽ�ı���.
```groovy
def number = 3.14
```

����� statement �����׳�һ��`groovy.lang.MissingPropertyException` �쳣,��ΪGroovy��Ϊ�����ڳ��Է����Ǹ����ֵĲ����ڵ�toString����.
```groovy
shouldFail(MissingPropertyException) {
    println "$number.toString()"
}
```
��������ɽ������Ὣ`"$number.toString()"` ���ͳ� `"${number.toString}()"`.�������Ҫ��GString�б���`$`����`${}` ��Ϊ��ֵ�Ļ�,ֻ��Ҫ������ǰ�����`\`����.

```groovy
assert '${name}' == "\${name}"
```
#### �����ֵ��ʽ-�հ����ʽ

��ĿǰΪֹ,���ǿ���������${}ռλ��������κεı��ʽ, ������һ������ı��ʽ-�հ����ʽ. ��ռλ���ںú�һ����ͷʱ`${��}`,������ʽʵ���Ͼ���һ���հ����ʽ.

```groovy
def sParameterLessClosure = "1 + 2 == ${-> 3}" (1)
assert sParameterLessClosure == '1 + 2 == 3'

def sOneParamClosure = "1 + 2 == ${ w -> w << 3}" (2)
assert sOneParamClosure == '1 + 2 == 3'
```
1. ���ڱհ�������������, ������ʹ�ñհ�ʱ,���ǲ��ض��䴫��
2. ������,�հ���ʹ����һ��`java.io.StringWriter argument`����, ���ǿ���ʹ��`<<`�������������.�����κ����, ռλ������Ƕ���˱հ�.

����ı��ʽ������������ʹ����һ�����µķ�ʽȥ�����ֵ���ʽ, ���Ǳհ��и���Ȥ�ָ߼������ԣ����Լ���:

```groovy
def number = 1 (1)
def eagerGString = "value == ${number}"
def lazyGString = "value == ${ -> number }"

assert eagerGString == "value == 1" (2)
assert lazyGString ==  "value == 1" (3)

number = 2 (4)
assert eagerGString == "value == 1" (5)
assert lazyGString ==  "value == 2" (6)
```

1. ���Ƕ�������ֵΪ1��number���ͱ���, ���Ժ����Ϊ��ֵ����������GString��,
2. ����ϣ��eagerGString �������ַ�����������ͬ��ֵ 1
3. ͬ������Ҳϣ��lazyGString �������ַ�����������ͬ��ֵ 1
4. Ȼ�����ǽ�number�ı�һ��ֵ.
5. With a plain interpolated expression, the value was actually bound at the time of creation of the GString.
6. But with a closure expression, the closure is called upon each coercion of the GString into String, resulting in an updated string containing the new number value.

An embedded closure expression taking more than one parameter will generate an exception at runtime. Only closures with zero or one parameters are allowed.

#### Inteoperability with Java
��һ������(��������Java������Groovy�ж����)����һ��`java.lang.String`����, �����Ǵ���һ��`groovy.lang.GString instance`ʵ��, GString���Զ�����toString()����.

```groovy
String takeString(String message) {         (4)
    assert message instanceof String        (5)
    return message
}

def message = "The message is ${'hello'}"   (1)
assert message instanceof GString           (2)

def result = takeString(message)            (3)
assert result instanceof String
assert result == 'The message is hello'
```
1. �������Ǵ���һ��GString����
2. Ȼ�����Ǽ��һ�������ı����Ƿ���GString��ʵ��
3. ����������һ������(����ΪString����)����GString���ͱ���
4. takeString()��ʽ��ָ������Ψһ�Ĳ���ΪString
5. �����ٴ���֤����Ĳ�����String ������GString


#### GString and String hashCodes

���ܲ�ֵ�ַ����ܱ���������`Java strings`, ����������ĳЩ�ط���������ȫһ���ġ��� ���ǵ�hashCodes�ǲ�ͬ��. Java Strig��`immutable`, Ȼ��, GStringͨ�������ڲ�ֵ ���ɵ��ַ����ǿ��Ըı��. ��ʹ������ȫһ�����ַ���, GStrings �� Strings�� hashCode ��Ȼ�ǲ�һ����.

```groovy
assert "one: ${1}".hashCode() != "one: 1".hashCode()
```

GString �� Strings ӵ�в�ͬ��hashCodeֵ, ��Map��Ӧ�ñ���ʹ��GString��Ϊkey, �ر��,��������Ҫ����ֵ��֮��Ӧ��ʹ��String,������GString.
```groovy
def key = "a"
def m = ["${key}": "letter ${key}"]     (1)

assert m["a"] == null                   (2)
```
1. mapʹ��һ�Լ�ֵ�������˳���,��key��GString����
2. ������ͨ��һ��String���͵�key���м���ֵ��ʱ��,���ǻ�õ�һ��null�Ľ��, ����������������������String��GStringӵ�в�ͬ��hashCode

### Triple double quoted string

����˫�����ַ�����ʹ�ú�˫�����ַ�����������, ����˫�����ַ�����ͬ��һ���ǣ������ǿ��Ի��е�(�����ص������ַ�������)
```groovy
def name = 'Groovy'
def template = """
    Dear Mr ${name},

    You're the winner of the lottery!

    Yours sincerly,

    Dave
"""

assert template.toString().contains('Groovy')
```

������˫�����ַ�����,������˫���Ż��ǵ����Ŷ�����Ҫescaped

### Slashy string
���������ַ���, Groovy���ṩ��slashy�ַ���(ʹ��/��Ϊ�ָ���). Slashy�ַ����Զ���������ʽ������ģʽ�Ƿǳ����õ�.

```groovy
def fooPattern = /.*foo.*/
assert fooPattern == '.*foo.*'
```

ֻ����`/ slashes`����Ҫʹ��\ ��escaped
```groovy
def escapeSlash = /The character \/ is a forward slash/
assert escapeSlash == 'The character / is a forward slash'
```

Slashy�ַ���Ҳ�����Ƕ��е�
```groovy
def multilineSlashy = /one
    two
    three/

assert multilineSlashy.contains('\n')
```

Slashy�ַ���Ҳ���Բ�ֵ��ʽ����(��GStringһ��)
```groovy
def color = 'blue'
def interpolatedSlashy = /a ${color} car/

assert interpolatedSlashy == 'a blue car'
```

������һЩ��ʶ����Ķ�����Ҫ��֪����
`//`���ᱻ����Ϊ��Slashy�ַ���,���������ע��.

```groovy
assert '' == //
```

### Dollar slashy string

Dollar slashy�ַ��� ͨ��`$/``/$` ��ʵ�ֶ���GString. ��Ԫ����Ϊת���ַ�, ����������ת����һ����Ԫ����, ����һ�� forward slash. ����Ҫʵ����GStringռλ���ͱհ���Ԫ��slashy�Ŀ�ͷ��Ԫ��֮��, ��Ԫ����forward slashes������Ҫת��
```groovy
def name = "Guillaume"
def date = "April, 1st"

def dollarSlashy = $/
    Hello $name,
    today we're ${date}.

    $ dollar sign
    $$ escaped dollar sign
    \ backslash
    / forward slash
    $/ escaped forward slash
    $/$ escaped dollar slashy string delimiter
/$

assert [
    'Guillaume',
    'April, 1st',
    '$ dollar sign',
    '$ escaped dollar sign',
    '\\ backslash',
    '/ forward slash',
        '$/ escaped forward slash',
        '/$ escaped dollar slashy string delimiter'

        ].each { dollarSlashy.contains(it) }
```

### Characters

����java, Groovy��û����ʽ���ַ�������. ����ͨ���������ַ�ʽ,��ʽ������Groovy �ַ�����
```groovy
char c1 = 'A' (1)
assert c1 instanceof Character

def c2 = 'B' as char (2)
assert c2 instanceof Character

def c3 = (char)'C' (3)
assert c3 instanceof Character
```
1. ͨ��ָ��char��������ʽ������һ��character����
2. ͨ��������ǿ��ת������
3. ͨ��ǿ��ת����ָ������

