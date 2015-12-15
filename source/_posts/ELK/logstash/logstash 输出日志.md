category: ELK
tag: logstash
date: 2015-12-15
title: logstash�����־
---

## ��׼���
```
output {
    stdout {
        codec => rubydebug
        workers => 2
    }
}
```
ʹ��`stdout`���ǽ��ܽ����ܵ�����־���������̨��

## ������ļ�
```
output {
    file {
        path => "D:\log\generate\%{+yyyy-MM-dd}.log"
    }
}
```
������������־�ͻ������`D:\log\generate`Ŀ¼�µ�2015-12-15.log�ļ���. 

��file�����,����path�ֶ�֮��,���ǻ��������ֶ�`message_format`��`gzip`
* `message_format` ��ѡ��Ҫ���������,Ĭ�����������event��JSON��ʽ�ַ���, ���ǿ��Խ�������Ϊ`%{message}`, �������Ǿ�ֻ�������־������
* `gzip` : ������������ǲ���gzipѹ��.

## �����elasticsearch
�����ܵ�����־����� elasticsearch��
```
output {
    elasticsearch {
        host => "192.168.0.2"
        protocol => "http"
        index => "logstash-%{type}-%{+YYYY.MM.dd}"
        index_type => "%{type}"
        workers => 5
        template_overwrite => true
    }
}
```