# POCO
- Is a collection of C++ class libraries
- It focuses on network-centric applications
- Is written in ANSI/ISO Standard C++ and based on the C++ Standard Library/STL
- Portable and available on many platforms
- Open source, licensed under the Boost Software License

## Types and Byte Order

### The Any Type
- An instance of ```Poco::Any``` can hold a value of any built-in or user-defined type
- Supports value semantic
- The value can be extracted in a type-safe manner: if the instance of ```Poco::Any``` *A* is instantiated with an ```int``` type, extracting *A* to any type that **not** ```int```  will throw a ```BadCastException```

### The DynamicAny Type
- An instance of ```Poco::DynamicAny``` can hold a value of any type for which a **DynamicAnyHolder** specialization is available, which includes all C++ built-in types with addition of ```std::string```, ```DateTime```, ```LocalDateTime```, ```TimeStamp```, and ```std::vector<DynamicAny>```
- Same as the ```Any``` type, the value can be extracted in a type-safe manner

## Memory Management

### Reference Counting
- Is a technique of storing the number of references, pointers, or handlers to a resource such as an object or a block of memory. Is used to avoid the memory leak.
- Given an object, each time a reference to the object is created or copied, the reference count is incremented. And if a reference to the object is destroyed or overwritten, the count is decremented.
- The initial count is 1.
- When the reference count reaches zero, the object is deleted.
- In a multithreaded scenario, incrementing and decrementing/comparing reference counters must be atomic operations.

### Object Ownership
- The "owner", who takes "ownership" of an object, is responsible for deleting the object when it's no longer needed. Other can point to the object, but they will not delete it.
- Memory leak happens when the owner failed to delete an unused object.
- The ownership is transferable. But at any time, only one owner is allowed. Taking the ownership does not change the reference count.
- Usually, the first pointer to the object takes ownership.

### The AutoPtr Class Template
- ```Poco::AutoPtr``` can be instantiated with any class that supports reference counting which must:
  + maintain a reference count (intialized to 1 at creation)
  + implement a method ```void duplicate()``` that increments the count
  + implement a method ```void release()``` that decrements the count and deletes the object when the count reaches 0
- ```Poco::AutoPtr``` supports a cast operation. The cast is always typesafe. The ```dynamic_cast``` is used internally, so an invalid cast will result in a null pointer.
- Be careful when assging an ```AutoPtr``` to a plain pointer, then assgin that plain pointer to another ```AutoPtr```. Both ```AutoPtr```'s will claim the ownership of the object. Instead, explicitly tell ```AutoPtr``` that it has to share the ownership of the object or not:
```cpp
AutoPtr::AutoPtr(C* Object, bool shared);
AutoPtr& AutoPtr::assign(C* object, bool shared);
```
```shared``` must be true to share the ownership. The difference is that the assignment of pointers will make the second ```AutoPtr``` assumes its sole ownership of the object, whereas the ```shared``` variable tells the second ```AutoPtr``` to share the ownership to another one.

### The AutoRealsePool
The ```Poco::AutoReleasePool``` takes care of (reference counted) objects that nobody else wants. It takes ownership of every object added to it. When the ```Poco::AutoReleasePool``` is destroyed (or its ```release()``` is called), it releases the references to all objects it holds.
