# Unified Modeling Language

## Diagrams

![Hierarchy of UML 2.2 Diagrams, shown as a class diagram](UML_diagrams_overview.svg)

---

* Structural UML diagrams
  * [Class diagram](#class-diagram)
    * [Members](#members)
      * [Visibility](#visibility)
      * [Scope](#scope)

---

## Class diagram

In the diagram, classes are represented with boxes that contain three compartments:

* The top compartment contains the name of the class. It is printed in bold and centered, and the first letter is capitalized.
* The middle compartment contains the attributes of the attributes of the class. They are left-aligned and the first letter is lowercase.
* The bottom compartment contains the operations the class can execute. They are also left-aligned and  the first letter is lowercase.

![A class with three compartments](BankAccount1.svg)

### Members

UML provides mechanisms to represent class members, such as attributes and methods, and additional information about them.

#### Visibility

To specify the visibility of a class member (i.e. any attribute or method), these notations must be placed placed before the member's name:

* `+` Public
* `-` Private
* `#` Protected
* `/` Derived (can be combined with one of the others)
* `~` Package
* `*` Random

#### Scope

The UML specifies two types of scope for members: *instance* and *classifier*, and the latter is represented by <u>underlined names</u>.

* **Classifier members** are commonly recognized as "static" in may programming languages. The scope is the class itself.
  * Attribute values are equal for all instances
  * Method invocation does not affect the instance's state
* **Instance members** are scoped to a specific instance.
  * Attribute values may vary between instances
  * Method invocation may affect the instance's state (i.e. change instance's attributes)

To indicate a classifier scope for a member, its name must be underlined. Otherwise, instance scope is assumed by default.

