category: �������
date: 2015-10-08
title: SQL
---

# Mysql����

һ�������ڴ���������ʱ��Ҫָ��������������. ��Ȼ������Ǳ����.

## ��������

### ��ͨ����
```sql
CREATE INDEX idxName ON db1.idtable(id);

CREATE TABLE t1 (
  id int NOT NULL,
  INDEX (id)
)
```

### Ψһ����
����ͨ�������ֵΨһ, �����п�ֵ
```sql
CREATE UNIQUE INDEX id ON db1.idtable(id);

CREATE TABLE t1 (
  id int NOT NULL,
  UNIQUE (id)
)
```

### ��������
����ͨ�������ֵΨһ, �������п�ֵ
```sql
CREATE TABLE t1 (
  id int NOT NULL,
  PRIMARY KEY (id)
)
```

### �������
�������Ǵ�����һ��Ψһ�������.
```sql
CREATE TABLE `t1` (
  `id` int(11) NOT NULL,
  `name` varchar(16) NOT NULL,
  UNIQUE KEY `idx` (`id`,`name`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8
```

## ����ע������
* ��������ϳ���`NULL`ֵ(�����������),��ô��һ�оͲ��ᱻ����
* ����Ҫ�����ܵ�ʹ�ö�����(ָ��һ��ǰ׺����),�����varchar���ͽ�������,���ĳ�����16���ַ�,����ǰ6���ַ��ǹ̶���,��ô����ָ��ǰ׺����Ϊ6�ͺ���
* ������Ҫ����������`like`����
* ��Ҫ����������ʹ��`NOT IN`��`<>`����