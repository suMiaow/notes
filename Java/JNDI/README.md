# Java Naming and Directory Interface

- [Architecture](#Architecture)
- [Packaging](#Packaging)
- [Naming Package](#Naming-Package)
  - [Context](#Context)
  - [Lookup](#Lookup)
  - [Binding](#Binding)
  - [List](#List)
  - [Name](#Name)

The Java Naming and Directory Interface™ (JNDI) is an application programming interface (API) that provides naming and directory functionality to applications written using the Java™ programming language. It is defined to be independent of any specific directory service implementation. Thus a variety of directories - new, emerging, and already deployed can be accessed in a common way.

## Architecture

The JNDI architecture consists of an API and a service provider interface (SPI). Java applications use the JNDI API to access a variety of naming and directory services. The SPI enables a variety of naming and directory services to be plugged in transparently, thereby allowing the Java application using the JNDI API to their services. See the following figure:

![JNDI arch](jndiarch.gif)

## Packaging

JNDI is included in the Java SE Platform. To use the JNDI, you must have the JNDI classes and one or more service providers. The JDK includes service providers for the following naming/directory services:

- Lightweight Directory Access Protocol (LDAP)
- Common Object Request Broker Architecture (CORBA) Common Object Services (COS) name service
- Java Remote Method Invocation (RMI) Registry
- Domain Name Service (DNS)

Other service providers can be down loaded from the JNDI page or obtained from other vendors.

The JNDI is divided into five packages:

- `javax.naming`
- `javax.naming.directory`
- `javax.naming.ldap`
- `javax.naming.event`
- `javax.naming.spi`

## Naming Package

The `javax.naming` package contains classes and interfaces for accessing naming services.

### Context

The `javax.naming` package defines a `Context` interface, which is the core interface for looking up, binding/unbinding, renaming objects and creating and creating and destroying subcontexts.

#### Lookup

The most commonly used operation is `lookup()`. You supply `lookup()` the name of the object you want to look up, and it returns the object bound to that name.

#### Bindings

`listBindings()` returns an enumeration of name-to-object bindings. A binding is a tuple containing the name of the bound object, the name of the object's class, and the object itself.

#### List

`list()` is similar to `listBindings()`, except that it returns an enumeration of names containing an object's name and the name of the object's class. `list()` is useful for applications such as browsers that want to discover information about the objects bound within a context but that don't need all of the actual objects. Although `listBindings()` provides all of the same information, it is potentially a much more expensive operation.

#### Name

`Name` is an interface that represents a generic name -- an ordered sequence of zero or more components. The Naming Systems use this interface to define the names that follow its conventions as described in the [Naming and Directory Concepts](Naming_and_Directory_Concepts).

