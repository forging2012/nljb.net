---
title: Java之Map的各种锁的使用
date: '2015-06-25'
description:
categories:

tags:java

---

>

### Java之Map的各种锁的使用

>

---

>

1.使用 synchronized 关键字，这也是最原始的方法

>

	synchronized(anObject)  {   
		value = map.get(key);
	}  

>

---

>

2.使用 JDK1.5 提供的锁（java.util.concurrent.locks.Lock）

>

	lock.lock();   
	value = map.get(key);   
	lock.unlock();  

>

---

>

这样处理时对于hashmap的读写都加锁了，但是如果涉及到少量插入及频繁的查找

那么上面两种的效率不是很高，这时候最好的方式是加读写锁

以下为构造读写锁包装map的方法：

	import java.util.Map;
	import java.util.concurrent.locks.Lock;
	import java.util.concurrent.locks.ReadWriteLock;
	import java.util.concurrent.locks.ReentrantReadWriteLock;

	public class ReadWriteMap<K, V> {

		private final Map<K, V> map;
		private final ReadWriteLock lock = new ReentrantReadWriteLock();
		private final Lock r = lock.readLock();
		private final Lock w = lock.writeLock();

		public ReadWriteMap(Map<K, V> map) {
			this.map = map;
		}

		public V put(K key, V value) {
			w.lock();
			try {
				return map.put(key, value);
			} finally {
				w.unlock();
			}
		}

		public V get(Object key) {
			r.lock();
			try {
				return map.get(key);
			} finally {
				r.unlock();
			}
		}
	}

>

---

>

使用 JDK1.5 提供的 java.util.concurrent.ConcurrentHashMap 类。

该类将 Map 的存储空间分为若干块，每块拥有自己的锁，大大减少了多个线程争夺同一个锁的情况。

	value = map.get(key); //同步机制内置在 get 方法中

>

---

>

1、如果 ConcurrentHashMap 够用，则使用 ConcurrentHashMap。 

2、如果需自己实现同步，则使用 JDK1.5 提供的锁机制，避免使用 synchronized 关键字。

