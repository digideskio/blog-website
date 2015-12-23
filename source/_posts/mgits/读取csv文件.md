category: mgits
tag: csv
date: 2015-06-08
title: java��ȡcsv�ļ�
---
����Apache Commons CSV��ȡcsv�ļ�, �������������������
```xml
<dependency>
	<groupId>org.apache.commons</groupId>
	<artifactId>commons-csv</artifactId>
	<version>1.2</version>
</dependency>
```
��ȡcsv�ļ�
```java
Reader in = new FileReader("path/to/file.csv");
Iterable<CSVRecord> records = CSVFormat.EXCEL.parse(in);
for (CSVRecord record : records) {
    String lastName = record.get("Last Name");
    String firstName = record.get("First Name");
}
```


