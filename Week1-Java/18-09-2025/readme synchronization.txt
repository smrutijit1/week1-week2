Regarding synchronization

1. Only methods (or blocks) can be synchronized, not variables or classes.
2. Each object has just one lock.
3. Not all methods in a class need to be synchronized. A class can have both
synchronized and non-synchronized methods.
4.  If two threads are about to execute a synchronized method in a class, and both threads are using the same instance of the class to invoke the method,only one thread at a time will be able to execute the method. The other thread will need to wait until the first one finishes its method call. In otherwords, once a thread acquires the lock on an object, no other thread can enter ANY of the synchronized methods in that class (for that object).


5. If a class has both synchronized and non-synchronized methods, multiple
threads can still access the class's non-synchronized methods. If you have methods that don't access the data you're trying to protect, then you don't
need to synchronize them. Synchronization can cause a hit in some cases (or
even deadlock if used incorrectly), so you should be careful not to overuse it.
6. If a thread goes to sleep(or invokes join,yield,notify) or encounters context switching , it holds any locks it has—it doesn't release them.
7.  A thread can acquire more than one lock. For example, a thread can enter a
synchronized method, thus acquiring a lock, and then immediately invoke
a synchronized method on a different object, thus acquiring that lock as
well. As the stack unwinds, locks are released again. Also, if a thread acquires
a lock and then attempts to call a synchronized method on that same
object, no problem. The JVM knows that this thread already has the lock for
this object, so the thread is free to call other synchronized methods on the
same object, using the lock the thread already has.
eg : 
class A {
private B b1;
 synched void test()
 {
   ...
   b1.testMe();
 }
}

class B 
{

 synched void testMe()
 {
  //some B.L
 }
}





8. You can synchronize a block of code rather than a method.
When to use synched blocks?
Because synchronization does hurt concurrency, you don't want to synchronize
any more code than is necessary to protect your data. So if the scope of a method is
more than needed, you can reduce the scope of the synchronized part to something
less than a full method—to just a block. OR when u are using Thread un-safe(un-sunchronized eg -- StringBuilder or HashMap or HashSet) classes in your appln.
-----------------------------
Regarding static & non -static synchronized
1. Threads calling non-static synchronized methods in the same class will
only block each other if they're invoked using the same instance. That's
because they each lock on "this" instance, and if they're called using two different
instances, they get two locks, which do not interfere with each other.
2. Threads calling static synchronized methods in the same class will always
block each other—they all lock on the same Class instance.

3.  A static synchronized method and a non-static synchronized method
will not block each other, ever. The static method locks on a Class
instance(java.lang.Class<?>)  while the non-static method locks on the "this" instance—these actions do not interfere with each other at all.







