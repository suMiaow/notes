# Java Naming and Directory Interface

- [Architecture](#Architecture)
- [Packaging](#Packaging)
- [Naming Package](#Naming-Package)
  - [Context](#Context)
  - [The Initial Context](#The-Initial-Context)
  - [Exceptions](#Exceptions)
- [Directory Package](#Directory-Package)
  - [The Directory Context](#The-Directory-Context)
- [LDAP Package](#LDAP-Package)
  - [LDAP v3 Specific Features](LDAP-v3-Specific-Features)
  - [The LDAP Context](#The-LDAP-Context)

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

- Lookup

  The most commonly used operation is `lookup()`. You supply `lookup()` the name of the object you want to look up, and it returns the object bound to that name.

- Bindings

  `listBindings()` returns an enumeration of name-to-object bindings. A binding is a tuple containing the name of the bound object, the name of the object's class, and the object itself.

- List

  `list()` is similar to `listBindings()`, except that it returns an enumeration of names containing an object's name and the name of the object's class. `list()` is useful for applications such as browsers that want to discover information about the objects bound within a context but that don't need all of the actual objects. Although `listBindings()` provides all of the same information, it is potentially a much more expensive operation.

- Name

  `Name` is an interface that represents a generic name -- an ordered sequence of zero or more components. The Naming Systems use this interface to define the names that follow its conventions as described in the [Naming and Directory Concepts](Naming_and_Directory_Concepts).

- References

  Objects are stored in naming and directory services in different ways. A reference might be very compact representation of an object.

  The JNDI defines the `Reference` class to represent reference. A reference contains information on how to construct a copy of the object. The JNDI will attempt to turn references looked up from the directory into the Java objects that they represent so that JNDI clients have the illusion that what is stored in the directory are Java objects.

### The Initial Context

In the JNDI, all naming and directory operations are performed relative to a context. There are no absolute roots. Therefore the JNDI defines an `InitialContext`, which provides a starting point for naming and directory operations. Once you have an initial context, you can use it to look up other contexts and objects.

### Exceptions

The JNDI defines a class hierarchy for exceptions that can be thrown in the course of performing naming and directory operations. The root of this class hierarchy is `NamingException`. Programs interested in dealing with a particular exception can catch the corresponding subclass of exception. Otherwise, they should catch `NamingException`

## Directory Package

The `javax.naming.directory` package extends the `javax.naming` package to provide functionality for accessing directory services in addition to naming services. This package allows applications to retrieve associated with objects stored in the directory and to search for objects using specified attributes.

### The Directory Context

The `DirContext` interface represents a *directory context*. `DirContext` also behaves as a naming context by extending the `Context` interface. This means that any directory object can also provide a naming context. It defines methods for examining and updating attributes associated with a directory entry.

- Attributes

  You use `getAttributes()` method to retrieve the attributes associated with a directory entry (for which you supply the name). Attributes are modified using `modifyAttributes()` method. You can add, replace, or remove attributes and/or attribute values using this operation.

- Searches

  `DirContext` contains methods for performing content based searching of the directory. In the simplest and most common form of usage, the application specifies a set of attributes possibly with specific values to match and submits this attribute set to the `search()` method. Other overloaded forms of `search()` support more sophisticated search filters.

## LDAP Package

The `javax.naming.ldap` package contains classes and interfaces for using features that are specific to the [LDAP v3](https://www.ietf.org/rfc/rfc2251.txt) that are not already covered by the more generic `javax.naming.directory` package. In fact, most JNDI applications that use the LDAP will find the `javax.naming.directory` package sufficient and will not need to use the `javax.naming.ldap` package at all. This package is primarily for those applications that need to use "extended" operations, controls, or unsolicited notifications.

- "Extended" Operation

  In addition to specifying well defined operations such as search and modify, the LDAP v3(RFC 2251) specifies a way to transmit yet-to-be defined operations between the LDAP client and the server. These operations are called "extended" operations. An "extended" operation may be defined by a standards organization such as the Internet Engineering Task Force (IETF) or by a vendor.

- Controls

  The LDAP v3 allows any request or response to be augmented by yet-to-be defined modifiers, called *controls*. A control sent with a request is a *request control* and a control sent with a response is a *response control*. A control may be defined by a standards organization such as the IETF or by a vendor. Request controls and response controls are not necessarily paired, that is, there need not be a response control for each request control sent, and vice versa.

- Unsolicited Notifications

  In addition to the normal request/response style of interaction between the client and server, the LDAP v3 also specifies *unsolicited notifications* -- messages that are sent from the server to the client asynchronously and not in response to any client request.

### The LDAP Context

The `LdapContext` interface represents a `context` for performing "extended" operations, sending request controls, and receiving response controls.

## Event Package

The `javax.naming.event` package contains classes and interfaces for supporting event notification in naming and directory services.

- Events

  A `NamingEvent` represents an events that is generated by a naming/directory service. The event contains a *type* that identifies the type of event. For example, event types are categorized in to those that affect the namespace, such as "object added", and those that do not, such as "object changed".

- Listeners

  A `NamingListener` is an object that listens for `NamingEvent`s. Each category of event type has a corresponding type of `NamingListener`. For example, a `NamespaceChangeListener` represents a listener interested in namespace change events and an `ObjectChangeListener` represents a listener interested in object change events.

To receive event notifications, a listener must be registered with either an `EventContext` or an `EventDirContext`. Once registered, the listener will receive event notifications when the corresponding changes occur in the naming/directory service.

## Service Provider Package

The `javax.naming.spi` package provides the means by which developers of different naming/directory service providers can develop and hook up their implementations so that the corresponding services are accessible from applications that use the JNDI.

- Plug-In Architecture

The `javax.naming.spi` package allows different implementations to be plugged in dynamically. These implementations include those for the initial context and for contexts that can be reached from the initial context.

- Java Object Support

  The `javax.naming.spi` package supports implementors of lookup and related methods to return Java objects that are natural and intuitive for the Java programmer. For example, if you look up a printer name from the directory, then you likely would expect to get back a printer object on which to operate. This support is provided in the form of object factories.

  This package also provides support for doing the reverse. That is, implementors of `Context.bind()` and related methods can accept Java objects and store the objects in a format acceptable to the underlying naming/directory service. This support is provided in the form of state factories.

- Multiple Naming Systems (Federation)

  JNDI operations allow applications to supply names that span multiple naming systems. In the process of completing an operation, one service might need to interact with another service provider, for example to pass on the operation to be continued in the next naming system. This package provides support for different providers to cooperate to complete JNDI operations.