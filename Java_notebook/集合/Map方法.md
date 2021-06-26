### map方法的介绍

```java
public class HashMapMeathod {
    public static void main(String[] args) {
        HashMap map = new HashMap();

        map.put("hello ","haer");
        map.put("erha","dasha");
        map.put(123,"aa");

        HashMap map1 = new HashMap();

        map1.put("hello ","haer");
        map1.put("erha","dasha");
        map1.put(123,"aa");

        // Object get(Object key)：获取指定key对应的value
        Object o = map.get(0);

        //boolean containsKey(Object key)：是否包含指定的key
        //boolean containsValue(Object value)：是否包含指定的value
        boolean b = map.containsKey(123);

        // int size()：返回map中key-value对的个数
        // boolean isEmpty()：判断当前map是否为空
        int size = map.size();
        System.out.println(size);

        // boolean equals(Object obj)：判断当前map和参数对象obj是否相等
        System.out.println(map.equals(map1));

        //Set keySet()：返回所有key构成的Set集合
        Set set = map.keySet();

        Iterator iterator = set.iterator();
        while (iterator.hasNext()){
            //这一步的主要目的是为了给这个对象赋予能够调getKey（）和getValue的能力（）----Entry类中没有提供此方法
			Map.Entry next = (Map.Entry)iterator;
            next.getKey();
            next.getValue;
        }

        //Collection values()：返回所有value构成的Collection集合
        Collection values = map.values();
        Iterator iterator1 = values.iterator();
        while (iterator1.hasNext()){
            
			Map.Entry next = (Map.Entry)iterator;
            next.getKey();
            next.getValue;
        }

        Set set1 = map.entrySet();

        for(Object obj : set1){
            System.out.println(obj);
        }
        
        //遍历keySet(set)：keyset（）和valueset（）
        Set set = map.keySet();

        Iterator iterator = set.iterator();
        while (iterator.hasNext()){
            System.out.println(iterator.next());
        }

        //entrySet():遍历所有的key- value

        Set set1 = map.entrySet();

        Iterator iterator1 = set1.iterator();
        while (iterator.hasNext()){
            System.out.println((Map.Entry)iterator.next());
        }


    }
}
```

总结:

增:  put（key , value )

删：remove（object key）

改： put（key , value )

查：get（object key）获取指定key对应的value

长度：size（）

遍历：

Set keySet（）；返回所有key构成的Set集合

Collection values()：返回所有value构成的Collection集合

Set entrySet()：返回所有key--value对构成的Set集合