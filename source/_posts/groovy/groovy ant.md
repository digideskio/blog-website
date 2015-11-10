category: groovy
date: 2014-04-08
title: groovy ANT
---
> �����Ƕ�Groovy���ֹٷ��ĵ������˷���

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