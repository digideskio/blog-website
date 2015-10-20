category: ����
date: 2013-09-13
title: CachesExplained
---

## Example
```java
LoadingCache<Key, Graph> graphs = CacheBuilder.newBuilder()  
       .maximumSize(1000)  
       .expireAfterWrite(10, TimeUnit.MINUTES)  
       .removalListener(MY_LISTENER)  
       .build(  
           new CacheLoader<Key, Graph>() {  
             public Graph load(Key key) throws AnyException {  
               return createExpensiveGraph(key);  
             }  
           });  
```

## ���÷�Χ
�����ʹ�÷�Χ��ʮ�ֹ㷺�ġ�ÿ���������ͨ��һЩ��ʽ����һ��ֵ��ʱ�򣬻������Դ�����˷ѵ�ʱ�����ǿ��Կ����û��漼�����洢��ֵ��

�����`CurrentMap`ʮ������(��ֵ����ʽ),��������֮�仹��������಻ͬ.����֮�����Ĳ�֮ͬ����`ConcurrentMap`���Ԫ���ڱ���ȷ��ɾ��֮ǰ��һֱ���洢��`Map`����Ƕ���cache��˵��Ϊ��ά��cache���ڴ�ռ�ã�cache����Ƴɻ��Զ�ɾ�����е����ݡ���һЩӦ�ó����У�ʹ��`LoadingCacheҲ`�Ƿǳ����õģ���ʹ�����������Զ�ɾ����entries(���������Զ��ڴ���ػ��ƣ�����������ô��)��
    
һ����˵��Guava�Ļ��漼��һ�����������³���
1. ��Ҫ���ĵ�һЩ�ڴ�����ȡ�ٶȵ�����
2. key(map��Ҳ��key)����һ��ʱ���ڱ�Ƶ���ķ��ʡ�
3. ��cache�洢�������������������RAM�д洢�ġ�

����԰��������е�����(CacheBuilder��builder pattern)������һ��Cache�����Ƕ��������Լ�Ӧ�ó����Cache����������ĵ��¡�

>ע��������Ӧ�ó����в����õ������ᵽ��Cache�����ԣ���ô����Կ���ConcurrentHashMap�������ڴ淽��Ҳ��������ơ�����ConcurrentHashMap�Ƿǳ����ѣ����������ܵ���ģ���Cache������ǿ���ܡ�
�������ѡ�񣬾�Ҫ�����Ӧ�ó���������,��ϸ���������ᵽ������----����Ԫ�صĴ���ڣ�Ԫ�صĴ�С�ȵȣ���Щ�ص㶼����ConcurrentMap���������ڵġ�

## ����
��Ӧ�������Լ���һ�����⣺���Ƿ����ض�����ȷ��ͨ��ĳЩkeys������������Value�ķ����������Ļش��ǿ϶��Ļ�����ô`CacheLoader`���ʺ���ġ�����㲻��Ҫͨ��ĳЩkey������value��������Ҫ����Ĭ�ϵķ���������Ҫʹ��`get-if-absent-compute`��ʽ,����Բο�[From A Callable]()��һ�����ǿ���ͨ��`Cache.put`ֱ�ӽ�Ԫ�ز���cache�У���������Ӧ�����ȿ��������Զ�������أ���Ϊ���ῼ�ǵ����л������ݵ�һ���ԡ�

> From A CacheLoader : LoadingCacheͨ��һ�����ŵ�CacheLoader������������һ��CacheLoaderҲ�Ƿǳ��򵥵ģ�ֻҪʵ��һ��V load(K key) throws exception�ķ����Ϳ�����.���������չʾ����δ���һ��LoadingCache
```java
LoadingCache<Key, Graph> graphs = CacheBuilder.newBuilder()  
       .maximumSize(1000)  
       .build(  
           new CacheLoader<Key, Graph>() {  
             public Graph load(Key key) throws AnyException {  
               return createExpensiveGraph(key);  
             }  
           });  
  
...  
try {  
  return graphs.get(key);  
} catch (ExecutionException e) {  
  throw new OtherException(e.getCause());  
}  
```
���������Ҳչʾ�������ǿ���ͨ��`get(K)`�ķ�ʽ��`LoadingCache`���в�ѯ��ȡֵ������������Դ�cache�в��ҵ���key����ô����ֱ�ӷ��ظ�key��Ӧ��value�������ͨ��cache��`CacheLoader`�Զ�����һ���µļ�ֵ�ԣ�Ȼ�󷵻ظ�ֵ����Ϊ`CacheLoader`���ܻ��׳��쳣������get(K)���ܻ��׳�`Execution`�������`CacheLoader`�ж�����һ�����쳣����`load`��������ô�ڲ�ѯȡֵʱ����ʹ��`getUnchecked(Key)`;���������������throws����һ����Ҫ����`getUnchecked(Key)`. ������һ�����ӣ�
```java
LoadingCache<Key, Graph> graphs = CacheBuilder.newBuilder()  
       .expireAfterAccess(10, TimeUnit.MINUTES)  
       .build(  
           new CacheLoader<Key, Graph>() {  
             public Graph load(Key key) { // no checked exception  
               return createExpensiveGraph(key);  
             }  
           });  
  
...  
return graphs.getUnchecked(key);  
```

��������Ҫ��ȡN��ֵ��ʱ���ڲ�ѯʱ����ʹ�÷���`getAll(Iterable<? extends K>)`.��getAll�У���ÿһ����������cache���key����ִ��һ�������Ķ�`CacheLoader.load`�ķ������������ظ�ֵ������guava�ṩ���������ķ���������һ��getAll�ȶ��get��������ʱ�����Ǿ�Ӧ������`CacheLoader.loadAll`��ʵ��������ܡ�

����ͨ��ʵ��`CacheLoader.loadAll`���������������Щ������������ʾ�����ֵ��

�����Ҫ�趨cache��һ���Ĵ�С����ͨ��`CacheBuilder.maximumSize(long)`���趨������趨��ʹ��cache�ڴﵽ�޶�ֵʱɾ����Щû�б�ʹ�ù����߲�����ʹ�õ�entries.

> From a Callable: ���е�Guava caches�������Ƿ���loadingģʽ�ģ���֧��get(K, Callable<V>)����������������cache�з������key�������value�����ߴ�Callable�м����ֵ�������Ž�cache�С��������ʹ����һ���ǳ��򵥵�ģʽ"if cached, return; otherwise create, cache and return"

```java
Cache<Key, Value> cache = CacheBuilder.newBuilder()  
    .maximumSize(1000)  
    .build(); // look Ma, no CacheLoader  
...  
try {  
  // If the key wasn't in the "easy to compute" group, we need to  
  // do things the hard way.  
  cache.get(key, new Callable<Value>() {  
    @Override  
    public Value call() throws AnyException {  
      return doThingsTheHardWay(key);  
    }  
  });  
} catch (ExecutionException e) {  
  throw new OtherException(e.getCause());  
}  
```
Inserted Directly : ValuesҲ����ͨ��cache.put(key,value)ֱ�ӽ�ֵ����cache�С��÷�������д��ǰ��keyƥ���entry��

## Eviction
һ�����ܱ�������⣺�����ڴ�ԭ�����ǲ��ܽ����еĶ��������ؽ�cache�С���ô������¾�����һ��cache entryӦ�ú�ʱ��������Guava�ṩ������entry�ͷŲ��ԣ�size-basd evicton��time-based eviction ��reference-based eviction

### Size-based Eviction
������cache����������,�����������趨�����ֵ����ôʹ��CacheBuilder.maxmuSize(long)���ɡ������������£�cache���Լ��ͷŵ���Щ���û�л��߲�����ʹ�õ�entries�ڴ档ע�⣺cache�������ڳ����޶�ʱ�Ż�ɾ������Щentries�������ڼ����ﵽ����޶�ֵʱ����ô���ҪС�Ŀ�����������ˣ���Ϊ�����Լ�ʹû�дﵽ����޶�ֵ��cache��Ȼ�����ɾ��������

����һ�������cache�ﲻͬ��entries���ܻ��в�ͬ��weight�����磺������cache values���Ž�Ȼ��ͬ���ڴ�ռ��----�����ʹ��CacheBuilder.weigher(Weigher)�趨weigh��ʹ��CacheBuilder.maximumWeight(long)�趨һ�����ֵ��
�������չʾ�˶�weight��ʹ��
```java
LoadingCache<Key, Graph> graphs = CacheBuilder.newBuilder()  
       .maximumWeight(100000)  
       .weigher(new Weigher<Key, Graph>() {  
          public int weigh(Key k, Graph g) {  
            return g.vertices().size();  
          }  
        })  
       .build(  
           new CacheLoader<Key, Graph>() {  
             public Graph load(Key key) { // no checked exception  
               return createExpensiveGraph(key);  
             }  
           });  
```

### Timed Eviction
CacheBuilder �ṩ�����ַ�ʽ��ʵ����һģʽ
expireAfterAccess(long, TimeUnit) 
�����һ�η���(������д)��ʼ��ʱ���������ָ����ʱ��ͻ��ͷŵ���entries��ע�⣺��Щ��ɾ����entries��˳��ʱ��size-based eviction��ʮ�����Ƶġ�
expireAfterWrite(long,TimeUnit)
���Ǵ�entries�������������һ�α��޸�ֵ�ĵ�����ʱ�ģ����������㿪ʼ�������Ƕ�ָ����ʱ�䣬entries�ͻᱻɾ�����������Ƶĺܾ�������Ϊ���ݻ�����ʱ����Խ��Խ�¾ɡ�
�����Ҫ����Timed Eviction��ʹ��Ticker interface��CacheBuilder.ticker(Ticker)���������cache�趨һ��ʱ�伴�ɣ���ô��Ͳ���Ҫȥ�ȴ�ϵͳʱ���ˡ�

### Reference-based Eviction
GuavaΪ��׼����entries������������������keys����values����ʹ��`weak reference` ������values����ʹ�� `soft reference`.

`CacheBuilder.weakKeys()`ͨ��weak reference�洢keys������������£����keysû�б�strong����soft���ã���ôentries�ᱻ�������ա����������µ������������ǽ����ڱ�ʶ��(����)֮�ϵģ���ô����������cache��ʹ��==���Ƚ�����key�ģ�������equals();

`CacheBuilder.weakValues()`  ͨ��weak referene �洢values.����������£����valvesû�б�strong����soft���ã���ôentries�ᱻ�������ա����������µ������������ǽ����ڱ�ʶ��(����)֮�ϵģ���ô����������cache��ʹ��==���Ƚ�����values�ģ�������equals();
CacheBuilder.softValues() 

### Explicit Removals
Ҳ����ĳ��ĳ��ĳ���㲻���ٵ�cache�ͷ�entries�������Լ����ֶ���ȥ�ͷŵ���Щentries���������������������
* �����ͷţ�Cache.invalidate(key)
* ����ͷţ�Cache.invalidateAll(keys)
* ȫ���ͷţ�Cache.invalidateAll()

### Removal Listeners
cache������ָ��һ��removal listener����entry���Ƴ�����(����`CacheBuilder.removalListener(RemovalListener)`).ͨ��R`emovaNotification`��õ�`RemovalListener`�ƶ���RemovalCause,key��value`��

>ע��RemovalListener�׳����κ��쳣���ᱻLogger��¼Ȼ�󱻶���

```java
CacheLoader<Key, DatabaseConnection> loader = new CacheLoader<Key, DatabaseConnection> () {  
  public DatabaseConnection load(Key key) throws Exception {  
    return openConnection(key);  
  }  
};  
RemovalListener<Key, DatabaseConnection> removalListener = new RemovalListener<Key, DatabaseConnection>() {  
  public void onRemoval(RemovalNotification<Key, DatabaseConnection> removal) {  
    DatabaseConnection conn = removal.getValue();  
    conn.close(); // tear down properly  
  }  
};  
  
return CacheBuilder.newBuilder()  
  .expireAfterWrite(2, TimeUnit.MINUTES)  
  .removalListener(removalListener)  
  .build(loader);  
```
���棺removal listeners�Ǳ�Ĭ��ͬ��ִ�еģ�����cache��ά����������ͨ������ά���ģ���ô������ġ�removal listener�ή��cache����(ĳЩ����)��Ч�ʡ��������ʹ��һ��"�����"removal listener�������ʹ��RemovalListener.asynchronous(RemovalListener,Executor),���䲼�ó��첽����.
