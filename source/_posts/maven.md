title: maven
---
# maven�汾����
## �Զ����汾����
�Զ����汾����������ȷ�İ汾��. һ�����ǵİ汾�Ź���Ϊ`��Ҫ�汾��.��Ҫ�汾��.�����汾��-��̱��汾��`

### ���
```xml
<plugin>
	<groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-release-plugin</artifactId>
    <version>2.5.2</version>
    <configuration>
		<!-- �����Ŀ�ṹ���ǲ��ñ�׼��SVN����(ƽ�е�trunk/tags/branches),����Ҫ������������ -->
		<tagBase>https://svn.mycompany.com/repos/myapplication/tags</tagBase>
		<branchBase>https://svn.mycompany.com/repos/myapplication/branchs</branchBase>
    </configuration>
</plugin>
```
### ִ������
#### mvn release:clean

#### mvn release:prepare`
׼���汾����,ִ�����в���
* �����Ŀ�Ƿ���δ�ύ����
* �����Ŀ�Ƿ��п��հ汾����
* �����û������뽫���հ汾����Ϊ������
* ��POM�е�SCM��Ϣ����ΪTAG��ַ
* �����޸ĺ��POMִ��MAVEN����
* �ύPOM���
* �����û������뽫�����TAG
* ������ӷ���������Ϊ�µĿ��հ�
* �ύPOM���
��ǰ������ok֮��,�������ʾ�û������Ҫ�����İ汾��,TAG���ƺ��µĿ��հ汾��

#### mvn release:rollback
�ع�`release:prepare`��ִ�еĲ���. ������Ҫע�������`release:prepare`�����д����TAG�����ᱻɾ��,��Ҫ�ֶ�ɾ��.

#### mvn release:perform
ִ�а汾����. ���`release:prepare`���ɵ�TAGԴ��,���ڴ˻�����ִ��`mvn deploy`,��������𵽲ֿ�.

#### mvn release:stage

#### mvn release:branch
ͨ��maven���֧,ִ�����в���
* �����Ŀ�Ƿ���δ�ύ����
* Ϊ��֧����POM�İ汾,�����`1.1.00SNAPSHOT`�ı�Ϊ`1.1.1-SNAPSHOT`
* ��POM�е�SCM��Ϣ����Ϊ��֧��ַ
* �ύ���ϸ���
* �����ɴ������Ϊ��֧����
* �޸ı��ش���ʹ֮���˵�֮ǰ�İ汾(�û�����ָ���µİ汾)
* �ύ���ظ���

#### mvn release:update-versions


















