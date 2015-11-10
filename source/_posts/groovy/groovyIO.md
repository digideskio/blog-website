category: groovy
date: 2014-04-08
title: groovy IO
---
> �����Ƕ�Groovy���ֹٷ��ĵ������˷���

### ���ļ�
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

### д�ļ�

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

### �����ļ�

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

### ���л�

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

### ִ������

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

������ʾһ��,���������������е����(����error stream).
```groovy
def p = "rm -f foo.tmp".execute([], tmpDir)
p.consumeProcessOutput()
p.waitFor()
```

`consumeProcessOutput`��Ȼ�кܶ��`StringBuffer`, `InputStream`, `OutputStream`�ȷ�װ�ı���, �����Ҫ��ȡһ�������ķ�װ�б��,�ǿ��Բο� [GDK API for java.lang.Process](http://docs.groovy-lang.org/latest/html/groovy-jdk/java/lang/Process.html)

����, `pipeTo`���� ������һ�����̵���������ӵ�һ�����̵���������. ������:

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

