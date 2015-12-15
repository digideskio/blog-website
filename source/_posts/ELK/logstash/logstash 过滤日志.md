category: ELK
tag: logstash
date: 2015-12-15
title: logstash������־
---
### filter���
```json
filter {
	#����������ʽ�����Ժ�(grok������������������ʽ��)������	
    grok {
        match => ["message",  "%{COMBINEDAPACHELOG}"]
    }
	date {
        match => ["logdate", "dd/MMM/yyyy:HH:mm:ss Z"]
    }
	json {
        source => "message"
        target => "jsoncontent"
    }
	mutate {
		#����ת��
        convert => ["request_time", "float"]
		#�ַ�������
		gsub 	=> ["urlparams", "[\\?#]", "_"]
		split 	=> ["message", "|"]
		join 	=> ["message", ","]
		merge 	=> ["message", "message"]
		#�ֶδ���
		rename => ["syslog_host", "host"]
		update => ["syslog_host", "host"]
		replace => ["syslog_host", "host"]
    }
	ruby {
        
    }
	split {
        field => "message"
        terminator => "#"
    }
	
}
```