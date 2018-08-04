# Reflection

- [Overview](#overview)
- [Classes](#classes)
- [Members](#members)

## Overview

### Uses of Reflection

Reflection is Commonly used by programs which require the ability to examine or modify the runtime behavior of applications running in the Java virtual machine.

- Extensibility features

  An application may make use of external, user-defined classes by creating instances of extensibility objects using their fully-qualified names.

- Class Browsers and visual Development Environments

  A class browser needs to be able to enumerate the members of classes. Visual development environments can benefit from making use of type information available in reflection to aid the developer in writing correct code.

- Debuggers and Test Tools

  Debuggers need to be able to examine private members on classes. Test harnesses can make use of reflection to systematically call a discoverable set APIs defined on a class, to insure a high level of code coverage in a test suite.

### Drawbacks of Reflection

Reflection is powerful, but should not be used indiscriminately. If it is possible to perform an operation without using reflection, then it is preferable to avoid using it. The following concerns should be kept in mind when accessing code via reflection.

- Performance Overhead

  Because reflection involves types that are dynamically resolved, certain Java virtual machine optimizations can not be performed. Consequently, reflective operations have slower performance than their non-reflective counterparts, and should be avoided in sections of code which are called frequently in performance-sensitive applications.

- Security Restrictions

  Reflection requires a runtime permission which may not be present when running under a security manager. This is in an important consideration for code which has to run in a restricted security context, such as in an Applet.

- Exposure of Internals

  Since reflection allows code to perform operations that would be illegal in non-reflective code, such as accessing `private` fields and methods, the use of reflection can result in unexpected side-effects, which may render code dysfunctional and may destroy portability. Reflective code breaks abstractions and therefore may change behavior with upgrades of the platform.

## Classes

For every type of object, the Java virtual machine instantiates an immutable instance of `java.lang.Class` which provides methods to examine the runtime properties of the object including its members and type information. `Class` also provides the ability to create new classes and objects. Most importantly, it is the entry point for all of the Reflection APIs.

### Retrieving Class Objects

The entry point for all reflection operations is `java.lang.Class`. With the exception of `java.lang.reflect.ReflectPermission`, none of the classes in `java.lang.reflect` have public constructors. To get to these classes, it is necessary to invoke appropriate methods on `Class`. There are several ways to get a `Class` depending on whether the code has access to an object, the name of class, a type, or an existing `Class`.

#### `Object.getClass()`

If an instance of object is available, then the simplest way o get its `Class` is to invoke `Object.getClass()`. Of course, this only works for reference types which all inherit from `Object`.

```java
// the Class for String
Class c1 = "foo".getClass();

// the Class corresponding to java.io.Console
Class c2 = System.console().getClass();

enum E { A, B }
// the Class corresponding to the enumeration type E
Class c3 = A.getClass();

byte[] bytes = new byte[1024];
// the Class corresponding to an array with component type byte
Class c4 = bytes.getClass();

import java.util.HashSet;
import java.util.Set;

Set<String> s = new HashSet<String>();
// the Class corresponding to java.util.HashSet
Class c5 = s.getClass();
```

#### The .class Syntax

If the type is available but there is no instance then it is possible to obtain a `Class` by appending ".class" to the name of the type. This is also the easiest way to obtain the `Class` for a primitive type.

```java
boolean b;
Class c1 = b.getClass(); // compile-time error

// the Class corresponding to the type boolean
Class c2 = boolean.class; // correct

// the Class corresponding to the type java.io.PrintStream
Class c3 = java.io.PrintStream.class;

// ths Class corresponding to a multidimensional array of int
Class c4 = int[][][].class;
```

#### Class.forName()

If the fully-qualified name of a class is available, it is possible to get the corresponding `Class` using the static method `Class.forName()`. This cannot be used for primitive types. The syntax for names of array classes is described by `Class.getName()`. This syntax is applicable to references and primitive types.

```java
Class c1 = Class.forName("com.test.MyClass");
Class cDoubleArray = Class.forName("[D");
Class cStringArray = Class.forName("[[Ljava.lang.String;");
```

#### TYPE Field for Primitive Type Wrappers

Each of the primitive types and `void` has a wrapper class in `java.lang` that is used for boxing of primitive types to reference types. Each wrapper class contains a field named `TYPE` which is equal to the `Class` for the primitive type being wrapped.

```java
Class c1 = Double.TYPE; // double.class
Class c2 = Void.TYPE; // void.class
```

#### Methods that Return Classes

- **`Class.getSuperClass()`**

  Returns the super class for the given class.

  ```java
  Class c = javax.swing.JButton.class.getSuperclass();
  ```

  The super class of `javax.swing.JButton` is `javax.swing.AbstractButton`.

- **`Class.getClasses()`**

  Returns all the public classes, interfaces and enums that are members of the class including inherited members.

  ```java
  Class<?>[] c = Character.class.getClasses();
  ```

  `Character` contains two member classes `Character.Subset` and `Character.unicodeBlock`.

- **`Class.getDeclaredClasses()`**

  Returns all of the classes interfaces, and enums that are explicitly declared in this class.

  ```java
  Class<?>[] c = Character.class.getDeclaredClasses();
  ```

  `Character` contains two public member classes `Character.Subset` and `Character.UnicodeBlock` and one private class `Character.CharacterCache`.

- **`Class.getDeclaringClass()`**
- **`java.lang.reflect.Field.getDeclaringClass()`**
- **`java.lang.reflect.Method.getDeclaringClass()`**
- **`java.lang.reflect.Constructor.getDeclaringClass()`**

  Returns the `Class` in which these members were declared. Anonymous Class Declarations will not have a declaring class but will have an enclosing class.

  ```java
  import java.lang.reflect.Field;

  Field f = System.class.getField("out");
  Class c = f.getDeclaringClass();
  ```

- **`Class.getEnclosingClass()`**

  ```java
  public class MyClass {
      static Object o = new Object() {
        public void m() {}
      };
      static Class<c> = o.getClass().getEnclosingClass();
  }
  ```

### Examining Class Modifiers and Types

A class may be declared with one or more modifiers which affect its runtime behavior:

- Access modifier: `public`, `protected`, and `private`
- Modifier requiring override: `abstract`
- Modifier restricting to one instance: `static`
- Modifier prohibiting value modification: `final`
- Modifier forcing strict floating point behavior: `strictfp`
- Annotations

### Discovering Class Members

#### Class Methods for Locating Fields

`Class` API | List of members? | Inherited members? | Private member?
--- | :---: | :---: | :---:
`getDeclaredField()`  | x | x | o
`getField()`          | x | o | x
`getDeclaredFields()` | o | x | o
`getFields()`         | o | o | x

#### Class Methods for Locating Methods

`Class` API | List of members? | Inherited members? | Private members?
--- | :---: | :---: | :---:
`getDeclaredMethod()`  | x | x | o
`getMethod()`          | x | o | x
`getDeclaredMethods()` | o | x | o
`getMethods()`         | o | o | x

#### Class Methods for Locating Constructors

`Class` API | List of members? | Private members?
--- | :---: | :---:
`getDeclaredConstructor()`  | x | o
`getConstructor()`          | x | x
`getDeclaredConstructors()` | o | o
`getConstructors()`         | o | x

> Constructors are not inherited

## Members

`java.lang.reflect`

- *`Member`*
  - *`Field`*
  - *`Method`*
  - *`Constructor`*

### Fields

A *field* is a class, interface, or enum with an associated value.