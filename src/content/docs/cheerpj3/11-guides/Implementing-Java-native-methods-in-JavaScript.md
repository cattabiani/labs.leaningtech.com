---
title: Implementing native methods
description: Java Native Interface (JNI) with CheerpJ
---

With CheerpJ, it is possible to implement Java 'native' methods (that would normally be implemented in C/C++ or other AOT-compiled language) in JavaScript, similarly to what would be done in regular Java using the Java Native Interface (JNI).

As an example, consider the following Java class:

```java title="TestClass.java"
package com.example;

public class TestClass {
  public native void alert(String str);
}
```

To provide an implementation of `alert`, pass it to the `cheerpjInit` function as a property of the `natives` object:

```js
await cheerpjInit({
	natives: {
		async Java_com_example_TestClass_alert(lib, str) {
			window.alert(str);
		},
	},
});
```

The name of methods in the `natives` object must be in the form `Java_<fully-qualified-class-name>_<method-name>`.

The `lib` parameter is a [CJ3Library]. It can be used to access other classes and methods of the library.

Parameters and return values of JNI calls are automatically converted between JavaScript and Java types.

[CJ3Library]: /cheerpj3/reference/cheerpjRunLibrary#cj3library
