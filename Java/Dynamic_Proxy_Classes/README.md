# Dynamic Proxy Classes

## Contents

* [**Introduction**](#introduction)
* [**Dynamic Proxy Class API**](#dynamic-proxy-class-api)
* [**Serialization**](#serialization)
* [**Example**](#example)

## Introduction

A *dynamic proxy class* is a class that implements a list of interfaces specified at runtime such that a method invocation through one of the interfaces on an instance of the class will be encoded and dispatched to another object through a uniform interface. Thus, a dynamic proxy class can be used to create a type-safe proxy object for a list of interfaces without requiring pre-generation of the proxy class, such as with compile-time tools. Method invocations on an instance of a dynamic proxy class are dispatched to a single method in the instance's *invocation handler*, and they are encoded with a `java.lang.reflect.Method` object identifying the method that was invoked and an array of type `Object` containing the arguments.

Dynamic proxy classes are useful to an application or library that needs to provide type-safe reflective dispatch of invocations on objects that present interface APIs. For example, an application can use a dynamic proxy class to create an object that implements multiple arbitrary event listener interfaces -- interfaces that extend `java.util.EventListener` -- to process a variety of events of different types in a uniform fashion, such as by logging all such events to a file.

## Dynamic Proxy Class API

A *dynamic proxy class* (simply referred to as a *proxy class* below) is a class that implements a list of interfaces specified at runtime when the class is created.

A *proxy interface* is such an interface that is implemented by a *proxy class*.

A *proxy instance* is an instance of a *proxy class*.

### **Creating a Proxy Class**

Proxy classes, as well as instances of them, are created using the static methods of the class [`java.lang.reflect.Proxy`](https://docs.oracle.com/javase/8/docs/api/java/lang/reflect/Proxy.html).

The `Proxy.getProxyClass` method returns the `java.lang.Class` object for a proxy class loader and an array of interfaces. The proxy class will be defined in the specified class loader and will implement all of the supplied interfaces. If a proxy class for the same permutation of interfaces has already been defined in the class loader, then te existing proxy class will be returned, otherwise, a proxy class for those interfaces will be generated dynamically and defined in the class loader.

There are several restrictions on the parameters that may be passed to `Proxy.getProxyClass`.

* All of the `class` in the `interfaces` array must represent interfaces, not classes or primitive types.
* No two elements in the `interfaces` array may refer to identical `Class` objects.
* All of the interface types must be visible by name through the specified class loader. In other words, for class loader `cl` and every interface `i`, the following expression must be true:

  ```java
  Class.forName(i.getName(), false, cl) == i
  ```

* All non-public interfaces must be in the same package; otherwise, it would no be possible for the proxy class to implement all of the interfaces, regardless of what package it is defined in.
* For any set of member methods of the specified interfaces that have the same signature:
  * If the return type of any of the methods is a primitive type or void, then all of the methods must have that same return type.
  * Otherwise, one of the methods must have a return type that is assignable to all of the return types of the rest of the methods.
* The resulting proxy class must not exceed any limits imposed on classes by the virtual machine. For example, the VM may limit the number of interfaces that a class may implement to 65535; in that case, the size of the `interfaces` array must not exceed 65535.

If any of these restrictions are violated, `Proxy.getProxyClass` will throw an `IllegalArgumentException`. If the `interfaces` array argument or amy of its elements are null, a `NullPointerException` will be thrown.

Note that the order of the specified proxy interfaces is significant: two requests for a proxy class with the same combination of interfaces but in a different order will result in two distinct proxy classes. Proxy classes are distinguished by the order of their proxy interfaces in order to provide deterministic method invocation encoding in cases where two or more of the proxy interfaces share a method with the same name and parameter signature; this reasoning is described in more detail in the section below titled [**Methods Duplicated in Multiple Proxy Interfaces**](#methods-duplicated-in-multiple-proxy-interfaces).

So that a new proxy class does not need to be generated each time `Proxy.getProxyClass` is invoked with the same class loader and list of interfaces, the implementation of the dynamic proxy class API should keep a cache of generated proxy classes, keyed by their corresponding loaders and interface list. The implementation should be careful not to refer to the class loaders, interfaces, and proxy classes in such a way as to prevent class loaders, and all of their classes, from being garbage collected when appropriate.

<div align="right">
    <b><a href="#contents">TOP</a></b>
</div>

### **Proxy Class Properties**

A proxy class has the following properties:

* Proxy classes are public, final, and not abstract.
* The unqualified name of a proxy class is unspecified. The space of class names that begin with the string `"Proxy"` is, however, to be reserved for proxy classes.
* A proxy class implements exactly the interfaces specified at its creation, in the same order.
* If a proxy class implements a non-public interface, then it will be defined in the same package as that interface. Otherwise, the package of a proxy class is also unspecified. Note that package sealing will not prevent a proxy class from being successfully defined in a particular package at runtime, and neither will classes already defined in the same class loader and the same package with particular signers.
* Since a proxy class implements all of the interfaces specified at its creation, invoking `gerInterfaces` on its `Class` object will return an array containing the same list off interfaces (in the order specified at its creation), invoking `getMethods` on its `Class` object will return an array of `Method` objects that include all of the methods in those interfaces, and invoking `getMethod` will find methods in the proxy interfaces as would be expected.
* The `Proxy.isProxyClass` method will return true if it is passed a proxy class-- a class returned by `ProxyClass` or the class of an object returned by `Proxy.newProxyInstance`-- and false otherwise. The reliability of this method is important for the ability to use it to make security decisions, so its implementation should not just test if the class in question extends `java.lang.reflect.Proxy`.
* The `java.security.ProtectionDomain` of a proxy class is the same as that of system classes loaded by the bootstrap class loader, such as `java.lang.Object`, because the code for a proxy class is generated by trusted system code. This protection domain will typically be granted `java.security.AllPermission`.

### **Creating a Proxy Instance**

Each proxy class has one public constructor that takes one argument, an implementation of the interface `InvocationHandler`.

### **Proxy Instance Properties**

### **Methods Duplicated in Multiple Proxy Interfaces**

## Serialization

## Example