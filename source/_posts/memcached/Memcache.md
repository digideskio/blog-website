category: memcached
date: 2015-11-18
title: memcached
---
## ԭ��
memcached��һ���������ڴ���󻺴�ϵͳ. ������libevent,�ɷ������չΪ�����С, ���ҶԷ�ֹ�ڴ�swap��ʹ�÷�����IO���˴����Ż�����.

memcached�ڴ���䣺

## ���ó���
* ���ʺϴ洢С���ݣ����Ҵ洢�������Ǵ�Сһ�µ�
* Memcached�ںܶ�ʱ������Ϊ���ݿ�ǰ��cacheʹ��
* ����ϲ��ʺϲ���Memcached
* ȷ��Memcached���ڴ治�ᱻSwap��ȥ
* ���ܱ����������ݣ��⽫����������������
* Local Cache+ Memcached���ֲַ�Cache���Ǻ��м�ֵ��
* Memcached����Ԥ����һ���ð취

	   
## ��װ
Memcached����libevent,��������������Ҫ��װlibevent
```
wget http://jaist.dl.sourceforge.net/project/levent/libevent/libevent-2.0/libevent-2.0.22-stable.tar.gz
tar -zxvf libevent-2.0.22-stable.tar.gz
cd libevent-2.0.22-stable
./configure --prefix=/usr && make && make install
```
��������װMemcached
```
wget http://memcached.org/latest
tar -zxvf memcached-1.x.x.tar.gz
cd memcached-1.x.x
./configure --with-libevent=/usr && make && make test && sudo make install
```

## ����
`memcached`����ѡ�� 
* `-s <file>` : Unix socket path to listen on (disables network support).
* `-a <perms>` : ��ͨ��sѡ���socket��ʱ��,���ǿ���ͨ��-aѡ��ָ������socketʹ�õ�Ȩ��(Ȩ��Ϊ�˽���).
* `-l <ip_addr>` : ������������ַ. Ĭ���Ǳ����κο��õĵ�ַ. 
* `-d` : �Ժ�̨���̷�ʽ����memcached
* `-u <username>` : ��memcached����root�û�����ʱ��������Ҫͨ���ò���ָ���û�(��ָ�����û��������)
* `-c <num>` : �������ͬʱ������.(Ĭ����1024).
* `-C` : �ر�CAS. (ÿ�����󶼻����8bytes��С).
* `-k` : �������еķ�ҳ�ڴ�. �ھ޴�Ļ���ϵͳ��,ʹ�����ѡ���Ƿǳ�Σ�յ�,ʹ�õ�ʹ��Ҫ�ο�README�ļ���memcached homepage��������.
* `-p <num>` : ���ü���TCP�˿ں�, Ĭ����11211.
* `-P` : ����pid�洢�ļ�.
* `-U <num>` : ���ü���UDP�˿ں�, Ĭ����11211, 0 ��ʾ�ر�UDP����.
* `-m <num>` : ���ö���洢��ʹ�õ�����ڴ�(��λ��MB,Ĭ����64M)
* `-M` : �رն���洢�����ڴ泬������ڴ�ʱ,�Զ�ɾ���������Ĺ���. ���memcached�������ڴ�ﵽ���ֵ�Ͳ����ٴ洢�µĶ���.
* `-r` : �����ĺ����ļ���С������������������ֵ.
* `-v` : ����Ϊverbose ͬʱ�����������errors ��warnings.
* `-R <num>` : This  option  seeks  to prevent client starvation by setting a limit to the number of sequential requests the server will process from an individual client connection. Once a connection has exceeded this value, the server will attempt to process I/O on other connections before handling any further request from this connection. The default value for this option is 20.
* `-f <factor>` : Use <factor>  as the multiplier for computing the sizes of memory chunks that items are stored in. A lower value may result in less wasted memory depending on the total amount of memory available and the distribution of item sizes.  The default is 1.25.
* `-n <size>` : Allocate  a  minimum  of <size>  bytes for the item key, value, and flags. The default is 48. If you have a lot of small keys and values, you can get a significant memory efficiency gain with a lower value. If you use a high chunk growth factor (-f option), on the other hand, you may want to increase the size to allow a bigger percentage of your items to fit in the most densely packed (smallest) chunks.
* `-i`     Print memcached and libevent licenses.
* `-P <filename>` : Print pidfile to <filename>, only used under -d option.
* `-t <threads>` : Number  of  threads  to use to process incoming requests. This option is only meaningful if memcached was compiled with thread support enabled. It is typically not useful to set this higher than the number of CPU cores on the memcached server. The default is 4.
* `-D <char>` : Use <char> as the delimiter between key prefixes and IDs. This is used for per-prefix stats reporting. The default is ":" (colon). If this option is specified, stats collection is turned on automat- ically; if not, then it may be turned on by sending the "stats detail on" command to the server.
* `-L` : Try  to  use  large memory pages (if available). Increasing the memory page size could reduce the number of TLB misses and improve the performance. In order to get large pages from the OS, memcached will allocate the total item-cache in one large chunk. Only available if supported on your OS.
* `-B <proto>` : Specify the binding protocol to use.  By default, the server will autonegotiate client connections.  By using this option, you can specify the protocol clients  must  speak.   Possible  options  are "auto" (the default, autonegotiation behavior), "ascii" and "binary".
* `-I <size>` : Override  the default size of each slab page. Default is 1mb. Default is 1m, minimum is 1k, max is 128m. Adjusting this value changes the item size limit.  Beware that this also increases the number of slabs (use -v to view), and the overal memory usage of memcached.
* `-F` : Disables the "flush_all" command. The cmd_flush counter will increment, but clients will receive an error message and the flush will not occur.
* `-o <options>` : Comma separated list of extended or experimental options. See -h or wiki for up to date list.

```
memcached  -d -p 10021 -l 10.234.10.12 -u root -c 1024  -P ./memcached1.pid
```

## javaʹ��
����ʹ��spymemcached��Ϊjava�ͻ�������memcached. ��Maven��Ŀ�������������
```xml
<groupId>net.spy</groupId>
	<artifactId>spymemcached</artifactId>
<version>2.12.0</version>
```
Ȼ������memcached
```java
MemcachedClient client = new MemcachedClient(new InetSocketAddress("10.234.10.12", 10021));
```
ͨ����һ�����Ǿͳɹ�����������memcached.Ȼ�����ǾͿ���ʹ��spymemcached�ṩ�Ĵ���api������memcached

## memcached��Ϣͳ��
���ǿ���ʹ��telnet����ֱ������memcached`telnet 127.0.0.1 10021`,Ȼ��������������鿴�����Ϣ

### stats
ͳ��memcached�ĸ�����Ϣ 
* `STAT pid 20401` memcache�������Ľ���ID
* `STAT uptime 47`  �������Ѿ����е����� 
* `STAT time 1447835371` ��������ǰ��unixʱ��� 
* `STAT version 1.4.24`  memcache�汾 
* `STAT libevent 2.0.22-stable`
* `STAT pointer_size 64` ��ǰ����ϵͳ��ָ���С��32λϵͳһ����32bit��
* `STAT rusage_user 0.002999`
* `STAT rusage_system 0.001999`
* `STAT curr_connections 10` ��ǰ���ŵ������� 
* `STAT total_connections 11` �ӷ����������Ժ������򿪹��������� 
* `STAT connection_structures 11` ��������������ӹ�����
* `STAT reserved_fds 20`
* `STAT cmd_get 0`  get�����ȡ�����������
* `STAT cmd_set 0`  set������棩��������� 
* `STAT cmd_flush 0`
* `STAT cmd_touch 0`
* `STAT get_hits 0`  �����д��� 
* `STAT get_misses 0` ��δ���д��� 
* `STAT delete_misses 0`
* `STAT delete_hits 0`
* `STAT incr_misses 0`
* `STAT incr_hits 0`
* `STAT decr_misses 0`
* `STAT decr_hits 0`
* `STAT cas_misses 0`
* `STAT cas_hits 0`
* `STAT cas_badval 0`
* `STAT touch_hits 0`
* `STAT touch_misses 0`
* `STAT auth_cmds 0`
* `STAT auth_errors 0`
* `STAT bytes_read 7` �ܶ�ȡ�ֽ����������ֽ����� 
* `STAT bytes_written 0` �ܷ����ֽ���������ֽ����� 
* `STAT limit_maxbytes 67108864`   �����memcache���ڴ��С���ֽڣ�
* `STAT accepting_conns 1`
* `STAT listen_disabled_num 0`
* `STAT threads 4`     ��ǰ�߳��� 
* `STAT conn_yields 0`
* `STAT hash_power_level 16`
* `STAT hash_bytes 524288`
* `STAT hash_is_expanding 0`
* `STAT malloc_fails 0`
* `STAT bytes 0`   ��ǰ�������洢itemsռ�õ��ֽ��� 
* `STAT curr_items 0` ��������ǰ�洢��items���� 
* `STAT total_items 0` �ӷ����������Ժ�洢��items������ 
* `STAT expired_unfetched 0`
* `STAT evicted_unfetched 0`
* `STAT evictions 0` Ϊ��ȡ�����ڴ��ɾ����items���������memcache�Ŀռ��������� 
* `STAT reclaimed 0`
* `STAT crawler_reclaimed 0`
* `STAT crawler_items_checked 0`
* `STAT lrutail_reflocked 0`

����Ҳ����ʹ��java��ȡ��Щ��Ϣ
```java
MemcachedClient client = new MemcachedClient(new InetSocketAddress("10.234.10.12", 10021));
client.getStats().entrySet().stream().forEach(entry -> {
	System.out.println("Node : " + entry.getKey());
	entry.getValue().entrySet().stream().forEach(value -> {
		System.out.println("    " + value.getKey() + " : " + value.getValue());
	});
});
```

### stats reset
����ͳ������ 

### stats slabs
��ʾslabs��Ϣ��������ϸ�������ݵķֶδ洢��� 
* `STAT active_slabs 0`
* `STAT total_malloced 0`

### stats items
��ʾslab�е�item��Ŀ 

### stats cachedump 1 0
�г�slabs��һ������KEYֵ 


### STAT evictions 0
��ʾҪ�ڳ��¿ռ���µ�item���ƶ��ĺϷ�item��Ŀ 