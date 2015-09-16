title: groovy
---
# IO
## 1.1. Reading files
��Ϊ��һ������,�����ǿ�һ��,������һ���ı��ļ����������
```groovy
new File(baseDir, 'haiku.txt').eachLine { line ->
    println line
}
```

`eachLine`������Groovy�Զ���ӵ�File Class��,ͬʱ��,Groovy������˺ܶ����,����,�������Ҫ֪��ÿһ�е��к�,�����ʹ���������:
```groovy
new File(baseDir, 'haiku.txt').eachLine { line, nb ->
    println "Line $nb: $line"
}
```
��������ʲôԭ��, ��`eachLine`���׳����쳣,�����������ȷ��,��Դ�Ѿ�����ȷ�Ĺرյ���. �������Groovy�Զ���ӵĹ���I/O��Դ�ķ�������Ч.

����, ĳ�������ʹ����`Reader`, �����㻹����Groovy�Լ�������Դ. �����������, ��ʹ�׳���exception, reader��Ȼ�ᱻ�Զ��ر�.
```groovy
def count = 0, MAXSIZE = 3
new File(baseDir,"haiku.txt").withReader { reader ->
    while (reader.readLine()) {
        if (++count > MAXSIZE) {
            throw new RuntimeException('Haiku should only have 3 verses')
        }
    }
}
```

�������Ҫ���ı��ļ���ÿһ�ж��Ž�һ��list��, �������ô��:
```groovy
def list = new File(baseDir, 'haiku.txt').collect {it}
```

�����������ò��������ļ���ÿһ�ж���ӵ�һ��������:
```groovy
def array = new File(baseDir, 'haiku.txt') as String[]
```

�������ʾ��,�ǳ��򵥵�ʵ����,��һ���ļ����һ���ֽ�������:
```groovy
byte[] contents = file.bytes
```

������,�������ɵػ����һ��������.
```groovy
def is = new File(baseDir,'haiku.txt').newInputStream()
// do something ...
is.close()
```

�ϸ����������ǻ����һ��������,����������ǲ��ò��ֶ��ر���, Groovy�ṩ��һ������`withInputStream`, ����������԰������Զ��Ĺر�������.
```groovy
new File(baseDir,'haiku.txt').withInputStream { stream ->
    // do something ...
}
```

## 1.2. Writing files

��ʱ��,����Ҫ��Ҳ��ֻ��д�ļ�,����չʾ��,�����Groovy��д�ļ�
```groovy
new File(baseDir,'haiku.txt').withWriter('utf-8') { writer ->
    writer.writeLine 'Into the ancient pond'
    writer.writeLine 'A frog jumps'
    writer.writeLine 'Water��s sound!'
}
```

������һ��Ҫ��ܼ򵥵�������˵,���ǿ���ʹ��`<<`���ļ���д
```groovy
new File(baseDir,'haiku.txt') << '''Into the ancient pond
A frog jumps
Water��s sound!'''
```

��Ȼ����ÿһ�����Ƕ������ļ�������ı�,�����������ʾ��,���������һ���ļ���д���ֽ�:
```groovy
file.bytes = [66,22,11]
```

��Ȼ,��Ҳ����ֱ�Ӵ�һ�������,�����������ʾ����ο���һ�������.
```groovy
def os = new File(baseDir,'data.bin').newOutputStream()
// do something ...
os.close()
```

ͬ`newInputStream`һ��,`newOutputStream`ͬ����Ҫ�ֶ��ر�, ok,�����뵽��`withOutputStream`:
```groovy
new File(baseDir,'data.bin').withOutputStream { stream ->
    // do something ...
}
```

## 1.3. Traversing file trees

�ڽű���, �и��ܳ��õ��������,����һ��Ŀ¼,Ȼ���ҵ�һ���ļ�,����ĳЩ����. Groovy�ṩ�˺ܶ෽��,���ﵽ���Ч��. �����������ʾ�˽�һ��Ŀ¼�µ������ļ���ִ��ĳ������:
```groovy
dir.eachFile { file ->                      (1)
    println file.name
}
dir.eachFileMatch(~/.*\.txt/) { file ->     (2)
    println file.name
}
```

1. ��Ŀ¼�µ�ÿ���ļ���ִ�бհ�����.
2. ����������ʽ��Ŀ¼���ҵ������������ļ�,Ȼ��ִ�бհ�����.

Ҳ������Ҫ����ĳ��Ŀ¼��Ŀ¼���������Ŀ¼, ��ô�����ʹ��`eachFileRecurse`
```groovy
dir.eachFileRecurse { file ->                      (1)
    println file.name
}

dir.eachFileRecurse(FileType.FILES) { file ->      (2)
    println file.name
}
```
1. ��Ŀ¼���������Ŀ¼���еݹ�, Ȼ����ҵ����ļ���Ŀ¼���бհ�����
2. ��Ŀ¼����еݹ����,����ֻ�����ļ�.

```groovy
dir.traverse { file ->
    if (file.directory && file.name=='bin') {
        FileVisitResult.TERMINATE                   (1)
    } else {
        println file.name
        FileVisitResult.CONTINUE                    (2)
    }

}
```
1. ����ҵ����ļ���Ŀ¼,������������"dir", ��ֹͣ����
2.  ��ӡ���ļ�������,���ű���

## 1.4. Data and objects

��java�л�ʹ��`java.io.DataOutputStream` ���л�����Ҳ������. Groovy���������Ҳ���˷ǳ��򵥵�ʵ��, �����������ʾ��������л��ͷ����л�:
```groovy
boolean b = true
String message = 'Hello from Groovy'
// Serialize data into a file
file.withDataOutputStream { out ->
    out.writeBoolean(b)
    out.writeUTF(message)
}
// ...
// Then read it back
file.withDataInputStream { input ->
    assert input.readBoolean() == b
    assert input.readUTF() == message
}
```

ͬ��,����������ʵ�������л��ӿ�`Serializable`, �����ʹ�� object output stream�������������л����ļ�:
```groovy
Person p = new Person(name:'Bob', age:76)
// Serialize data into a file
file.withObjectOutputStream { out ->
    out.writeObject(p)
}
// ...
// Then read it back
file.withObjectInputStream { input ->
    def p2 = input.readObject()
    assert p2.name == p.name
    assert p2.age == p.age
}
```

## 1.5. Executing External Processes

ǰ����½ڽ�������Groovy�в���files, readers or streams�ǳ���. Ȼ��, ��ϵͳ����Ա���߿�����,���ܸ������ִ��һ��ϵͳ����.

Groovyͬ���ṩ�˷ǳ��򵥵ķ�ʽִ������������. ֻ��Ҫ����һ��������ַ���,Ȼ��ִ������ַ�����`execute()`. ����Unixϵͳ��(�����windows��Ҳ��װ����Unix�����й���Ҳ��),���������ִ������.
```groovy
def process = "ls -l".execute()             (1)
println "Found text ${process.text}"        (2)
```
1. ���ⲿ����(external process)ִ��ls����
2. �����������,�����

`execute()`��������һ��`java.lang.Process`ʵ��, ���ѡ��һ�������`in/out/err`, ͬʱ���`exit`ֵ,�鿴�Ƿ�����ִ�����.

���������ʹ���˺͸ղ��Ǹ�����һ��������,������������ÿ�ζ���Ի�õĽ�����������.
```groovy
            def process = "ls -l".execute()             (1)
            process.in.eachLine { line ->               (2)
                println line                            (3)
            }
            assert process instanceof Process
        }
    }

    void testProcessConsumeOutput() {
        if (unixlike) {
            doInTmpDir { b ->
                File file = null
                def tmpDir = b.tmp {
                    file = 'foo.tmp'('foo')
                }
                assert file.exists()
                def p = "rm -f foo.tmp".execute([], tmpDir)
                p.consumeProcessOutput()
                p.waitFor()
                assert !file.exists()
            }

        }
    }

    void testProcessPipe() {
        if (unixlike) {
            doInTmpDir { b ->
                def proc1, proc2, proc3, proc4
                proc1 = 'ls'.execute()
                proc2 = 'tr -d o'.execute()
                proc3 = 'tr -d e'.execute()
                proc4 = 'tr -d i'.execute()
                proc1 | proc2 | proc3 | proc4
                proc4.waitFor()
                if (proc4.exitValue()) {
                    println proc4.err.text
                } else {
                    println proc4.text
                }

                def sout = new StringBuilder()
                def serr = new StringBuilder()
                proc2 = 'tr -d o'.execute()
                proc3 = 'tr -d e'.execute()
                proc4 = 'tr -d i'.execute()
                proc4.consumeProcessOutput(sout, serr)
                proc2 | proc3 | proc4
                [proc2, proc3].each { it.consumeProcessErrorStream(serr) }
                proc2.withWriter { writer ->
                    writer << 'testfile.groovy'
                }
                proc4.waitForOrKill(1000)
                println "Standard output: $sout"
                println "Standard error: $serr"
            }
        }
    }

    public static class Person implements Serializable {
        String name
        int age
    }
}
```

1	executes the ls command in an external process
2	for each line of the input stream of the process
3	print the line
1. ���ⲿ������ִ��ls����
2.

It is worth noting that in corresponds to an input stream to the standard output of the command. out will refer to a stream where you can send data to the process (its standard input).


Remember that many commands are shell built-ins and need special handling. So if you want a listing of files in a directory on a Windows machine and write:

```groovy
def process = "dir".execute()
println "${process.text}"
```

��������յ�һ���쳣`IOException`,�쳣��ϢΪ`Cannot run program "dir": CreateProcess error=2`,ϵͳ�Ҳ���ָ�����ļ�.

������Ϊ`dir`���ڽ���`windows shell(cmd.ext)`, ��Ҫʹ���Ǹ�����,��Ҫ���������������:
```groovy
def process = "cmd /c dir".execute()
println "${process.text}"
```

����,��Ϊ�����Ĺ��������ڲ�ʹ�õ�`java.lang.Process`, ������һЩ����ĵط�,����ҲҪ��ֿ���. ��javadoc��,����������������:

> Because some native platforms only provide limited buffer size for standard input and output streams, failure to promptly write the input stream or read the output stream of the subprocess may cause the subprocess to block, and even deadlock
Because of this, Groovy provides some additional helper methods which make stream handling for processes easier.

Here is how to gobble all of the output (including the error stream output) from your process:

������ʾһ��,���������������е����(����error stream).
```groovy
def p = "rm -f foo.tmp".execute([], tmpDir)
p.consumeProcessOutput()
p.waitFor()
```

`consumeProcessOutput`��Ȼ�кܶ��`StringBuffer`, `InputStream`, `OutputStream`�ȷ�װ�ı���, �����Ҫ��ȡһ�������ķ�װ�б��,�ǿ��Բο� [GDK API for java.lang.Process](http://docs.groovy-lang.org/latest/html/groovy-jdk/java/lang/Process.html)

����, `pipeTo`���� ������һ�����̵���������ӵ�һ�����̵���������. ������:

Pipes in action
```groovy
proc1 = 'ls'.execute()
proc2 = 'tr -d o'.execute()
proc3 = 'tr -d e'.execute()
proc4 = 'tr -d i'.execute()
proc1 | proc2 | proc3 | proc4
proc4.waitFor()
if (proc4.exitValue()) {
    println proc4.err.text
} else {
    println proc4.text
}
```
Consuming errors
```groovy
def sout = new StringBuilder()
def serr = new StringBuilder()
proc2 = 'tr -d o'.execute()
proc3 = 'tr -d e'.execute()
proc4 = 'tr -d i'.execute()
proc4.consumeProcessOutput(sout, serr)
proc2 | proc3 | proc4
[proc2, proc3].each { it.consumeProcessErrorStream(serr) }
proc2.withWriter { writer ->
    writer << 'testfile.groovy'
}
proc4.waitForOrKill(1000)
println "Standard output: $sout"
println "Standard error: $serr"
```

# 2. Working with collections

Groovy ���Բ����Ͼ�֧�ֶ��ּ�������,����list, map, range. ��������ͼ��϶��ǻ���java�ļ��Ͽ��,����Groovy development kit����Щ�������úܶ��ݷ���.
## 2.1. Lists

### 2.1.1. List literals

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

### 2.1.2. List as a boolean expression

list�����Լ����boolean���ʽ.
```groovy
assert ![]             // an empty list evaluates as false

//all other lists, irrespective of contents, evaluate as true
assert [1] && ['a'] && [0] && [0.0] && [false] && [null]
```

### 2.1.3. Iterating on a list

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

### 2.1.4. Manipulating lists

###### Filtering and searching

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

###### Adding or removing elements

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

###### Set operations

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

###### Sorting

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

###### Duplicating elements

`roovy development kit`��ͨ�����ز������ķ�ʽ, �ڲ��ṩ��һЩ��������listԪ�ظ���.
```groovy
assert [1, 2, 3] * 3 == [1, 2, 3, 1, 2, 3, 1, 2, 3]
assert [1, 2, 3].multiply(2) == [1, 2, 3, 1, 2, 3]
assert Collections.nCopies(3, 'b') == ['b', 'b', 'b']

// nCopies from the JDK has different semantics than multiply for lists
assert Collections.nCopies(2, [1, 2]) == [[1, 2], [1, 2]] //not [1,2,1,2]
```

## 2.2. Maps

### 2.2.1. Map literals

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

### 2.2.2. Map property notation

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

### 2.2.3. Iterating on maps

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

### 2.2.4. Manipulating maps

###### Adding or removing elements

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

###### Keys, values and entries

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


###### Filtering and searching

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

###### Grouping

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

## 2.3. Ranges

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

# Parsing and producing JSON

Groovy ԭ��֧��Groovy�����JSON֮���ת��. `groovy.json`���ڵ�������JSON�����л��ͽ�������

## 1. JsonSlurper

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

## 1.1. Parser Variants

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

# 2. JsonOutput

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

## 2.4. Syntax enhancements for collections

### 2.4.1. GPath support

Thanks to the support of property notation for both lists and maps, Groovy provides syntactic sugar making it really easy to deal with nested collections, as illustrated in the following examples:

```groovy
def listOfMaps = [['a': 11, 'b': 12], ['a': 21, 'b': 22]]
assert listOfMaps.a == [11, 21] //GPath notation
assert listOfMaps*.a == [11, 21] //spread dot notation

listOfMaps = [['a': 11, 'b': 12], ['a': 21, 'b': 22], null]
assert listOfMaps*.a == [11, 21, null] // caters for null values
assert listOfMaps*.a == listOfMaps.collect { it?.a } //equivalent notation
// But this will only collect non-null values
assert listOfMaps.a == [11,21]
```

### 2.4.2. Spread operator

The spread operator can be used to "inline" a collection into another. It is syntactic sugar which often avoids calls to putAll and facilitates the realization of one-liners:

```groovy
assert [ 'z': 900,
         *: ['a': 100, 'b': 200], 'a': 300] == ['a': 300, 'b': 200, 'z': 900]
//spread map notation in map definition
assert [*: [3: 3, *: [5: 5]], 7: 7] == [3: 3, 5: 5, 7: 7]

def f = { [1: 'u', 2: 'v', 3: 'w'] }
assert [*: f(), 10: 'zz'] == [1: 'u', 10: 'zz', 2: 'v', 3: 'w']
//spread map notation in function arguments
f = { map -> map.c }
assert f(*: ['a': 10, 'b': 20, 'c': 30], 'e': 50) == 30

f = { m, i, j, k -> [m, i, j, k] }
//using spread map notation with mixed unnamed and named arguments
assert f('e': 100, *[4, 5], *: ['a': 10, 'b': 20, 'c': 30], 6) ==
        [["e": 100, "b": 20, "c": 30, "a": 10], 4, 5, 6]
```

### 2.4.3. The star-dot `*.' operator

The "star-dot" operator is a shortcut operator allowing you to call a method or a property on all elements of a collection:

```groovy
assert [1, 3, 5] == ['a', 'few', 'words']*.size()

class Person {
    String name
    int age
}
def persons = [new Person(name:'Hugo', age:17), new Person(name:'Sandra',age:19)]
assert [17, 19] == persons*.age
```

### 2.4.4. Slicing with the subscript operator

You can index into lists, arrays, maps using the subscript expression. It is interesting that strings are considered as special kinds of collections in that context:

```groovy
def text = 'nice cheese gromit!'
def x = text[2]

assert x == 'c'
assert x.class == String

def sub = text[5..10]
assert sub == 'cheese'

def list = [10, 11, 12, 13]
def answer = list[2,3]
assert answer == [12,13]
```

Notice that you can use ranges to extract part of a collection:

```groovy
list = 100..200
sub = list[1, 3, 20..25, 33]
assert sub == [101, 103, 120, 121, 122, 123, 124, 125, 133]
```

The subscript operator can be used to update an existing collection (for collection type which are not immutable):

```groovy
list = ['a','x','x','d']
list[1..2] = ['b','c']
assert list == ['a','b','c','d']
```

It is worth noting that negative indices are allowed, to extract more easily from the end of a collection:

You can use negative indices to count from the end of the List, array, String etc.

```groovy
text = "nice cheese gromit!"
x = text[-1]
assert x == "!"

def name = text[-7..-2]
assert name == "gromit"
```

Eventually, if you use a backwards range (the starting index is greater than the end index), then the answer is reversed.

```groovy
text = "nice cheese gromit!"
name = text[3..1]
assert name == "eci"
```

## 2.5. Enhanced Collection Methods

In addition to lists, maps or ranges, Groovy offers a lot of additional methods for filtering, collecting, grouping, counting, ��? which are directly available on either collections or more easily iterables.

In particular, we invite you to read the Groovy development kit API docs and specifically:

* methods added to Iterable can be found here
* methods added to Iterator can be found here
* methods added to Collection can be found here
* methods added to List can be found here
* methods added to Map can be found here

# Scripting Ant tasks

��Ȼ`Ant`ֻ��һ����������, �����ṩ�������ܹ������ļ�(����zip�ļ�), ����, ��Դ��������ʵ�ù���. Ȼ������㲻ϲ��ʹ��`build.xml`�ļ�����`Jelly`�ű�, ������Ҫһ���������Ĺ�����ʽ, ��ô��Ϳ�������ʹ��Groovy��д��������.

Groovy�ṩ��һ��������`AntBuilder`��æ��дAnt��������. ������������һ�����������ŵ�Ant��s XML�ļ��汾. ���������ڽű��л�Ϻ�ƥ����. Ant������һ��Jar�ļ��ļ���. ������jar�ļ���ӵ����classpath��, ��Ϳ�����Groovy���������ɵ�ʹ������.

`AntBuilder`ͨ����ݵĹ������﷨ֱ�ӱ�¶��Ant task. ������һ���򵥵�ʾ��, ���Ĺ������ڱ�׼��������һ����Ϣ.
```groovy
def ant = new AntBuilder()          
ant.echo('hello from Ant!')        
```

1. ����һ��`AntBuilder`ʵ��
2. ִ��`AntBuilder`ʵ����echo task

����,��������Ҫ����һ��ZIP�ļ���
```groovy
def ant = new AntBuilder()
ant.zip(destfile: 'sources.zip', basedir: 'src')
```

�������������, ���ǽ�չʾ��Groovy��ʹ�ô�ͳ��Ant ģʽͨ��`AntBuilder`����һ���ļ�.
```groovy
// lets just call one task
ant.echo("hello")

// here is an example of a block of Ant inside GroovyMarkup
ant.sequential {
    echo("inside sequential")
    def myDir = "target/AntTest/"
    mkdir(dir: myDir)
    copy(todir: myDir) {
        fileset(dir: "src/test") {
            include(name: "**/*.groovy")
        }
    }
    echo("done")
}

// now lets do some normal Groovy again
def file = new File(ant.project.baseDir,"target/AntTest/groovy/util/AntTest.groovy")
assert file.exists()
```

����������Ǳ���һ���ļ�, Ȼ��ÿ���ļ���������ģʽ����ƥ��.
```groovy
// lets create a scanner of filesets
def scanner = ant.fileScanner {
    fileset(dir:"src/test") {
        include(name:"**/Ant*.groovy")
    }
}

// now lets iterate over
def found = false
for (f in scanner) {
    println("Found file $f")
    found = true
    assert f instanceof File
    assert f.name.endsWith(".groovy")
}
assert found
```

Or execute a JUnit test:

��������ִ��JUnit
```groovy
// lets create a scanner of filesets
ant.junit {
    test(name:'groovy.util.SomethingThatDoesNotExist')
}
```

����, �����ǵĲ������ظ���һ�㣺��Groovy�б���Ȼ��ִ��һ��Java�ļ�.
```groovy
ant.echo(file:'Temp.java', '''
    class Temp {
        public static void main(String[] args) {
            System.out.println("Hello");
        }
    }
''')
ant.javac(srcdir:'.', includes:'Temp.java', fork:'true')
ant.java(classpath:'.', classname:'Temp', fork:'true')
ant.echo('Done')
```

��Ҫ�ἰ����, `AntBuilder`����Ƕ��`Gradle`�е�. ���������Groovy������, ��`Gradle`ʹ��`AntBuilder`