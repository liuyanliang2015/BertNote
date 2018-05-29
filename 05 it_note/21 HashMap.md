## HashMap简介
Hash散列，将一个任意长度通过某种算法(hash算法)转成为一个固定值。

Map地图，x，y存储

总结：通过hash算出来得值，然后通过值定位到这个map，然后value存储到这个map中。

## HashMap源码

    static final int DEFAULT_INITIAL_CAPACITY = 1 << 4;  //初始化容量16
	static final int MAXIMUM_CAPACITY = 1 << 30; //最大容量1073741824
    static final float DEFAULT_LOAD_FACTOR = 0.75f; //加载因子，put方法时，在容量达到3/4的时候扩容

 

    public V put(K key, V value) {
        if (table == EMPTY_TABLE) {
            inflateTable(threshold);
        }
        // HashMap的key是可以为空的
        if (key == null)
            return putForNullKey(value);
        int hash = hash(key);
        int i = indexFor(hash, table.length);
        for (Entry<K,V> e = table[i]; e != null; e = e.next) {
            Object k;
            //如果hash出来的key重复了，value会覆盖
            if (e.hash == hash && ((k = e.key) == key || key.equals(k))) {
                V oldValue = e.value;
                e.value = value;
                e.recordAccess(this);
                return oldValue;
            }
        }

        modCount++;
        addEntry(hash, key, value, i);
        return null;
    }


     final int hash(Object k) {
        int h = hashSeed;
        if (0 != h && k instanceof String) {
            return sun.misc.Hashing.stringHash32((String) k);
        }

        h ^= k.hashCode();

        // This function ensures that hashCodes that differ only by
        // constant multiples at each bit position have a bounded
        // number of collisions (approximately 8 at default load factor).
        //一种算法，得到一个相对比较分散的链表
        h ^= (h >>> 20) ^ (h >>> 12);
        return h ^ (h >>> 7) ^ (h >>> 4);
    }


     public V get(Object key) {
        if (key == null)
            return getForNullKey();
        Entry<K,V> entry = getEntry(key);

        return null == entry ? null : entry.getValue();
    }