---
layout: post
title: ThreadLocal
subtitle: ThreadLocal
author: oliver
header-img: img/post-bg-rwd.jpg
catalog: true
tags:
    - 并发
---
# ThreadLocal如何为每个线程创建变量的副本
- 首先，在每个线程Thread内部有一个ThreadLocal.ThreadLocalMap类型的成员变量threadLocals，这个threadLocals就是用来存储实际的变量副本的，键值为当前ThreadLocal变量，value为变量副本（即T类型的变量）。

- 初始时，在Thread里面，threadLocals为空，当通过ThreadLocal变量调用get()方法或者set()方法，就会对Thread类中的threadLocals进行初始化，并且==以当前ThreadLocal变量为键值==，以ThreadLocal要保存的副本变量为value，存到threadLocals。

- 然后在当前线程里面，如果要使用副本变量，就可以通过get方法在threadLocals里面查找。

# ThreadLocal的应用场景
最常见的ThreadLocal使用场景为 用来解决 数据库连接、Session管理等。

如数据库连接：

```
private static ThreadLocal<Connection> connectionHolder = new ThreadLocal<Connection>() {
    public Connection initialValue() {
        return DriverManager.getConnection(DB_URL);
    }
};
 
public static Connection getConnection() {
    return connectionHolder.get();
}
```

Session管理：
```
private static final ThreadLocal threadSession = new ThreadLocal();
 
public static Session getSession() throws InfrastructureException {
    Session s = (Session) threadSession.get();
    try {
        if (s == null) {
            s = getSessionFactory().openSession();
            threadSession.set(s);
        }
    } catch (HibernateException ex) {
        throw new InfrastructureException(ex);
    }
    return s;
}
```


# 总结

1. 实际的通过ThreadLocal创建的副本是存储在每个线程自己的threadLocals中的；

1. 为何threadLocals的类型ThreadLocalMap的键值为ThreadLocal对象，因为每个线程中可有多个threadLocal变量，就像上面代码中的longLocal和stringLocal；

1. 在进行get之前，必须先set，否则会报空指针异常；

1. ==如果想在get之前不需要调用set就能正常访问的话，必须重写initialValue()方法==；
> 因为在上面的代码分析过程中，我们发现如果没有先set的话，即在map中查找不到对应的存储，则会通过调用setInitialValue方法返回i，而在setInitialValue方法中，有一个语句是T value = initialValue()， 而默认情况下，initialValue方法返回的是null。
