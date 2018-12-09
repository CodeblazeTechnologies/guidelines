# Protocol Oriented Programming


### What's a Protocol?
> As per Apple's The Swift Programming Language (Swift 4.0.3)

A protocol defines a blueprint of methods, properties, and other requirements that suit a particular task or piece of functionality. The protocol can then be adopted by a class, structure, or enumeration to provide an actual implementation of those requirements. Any type that satisfies the requirements of a protocol is said to conform to that protocol.
  In my words, **Protocols are rules and roles that are to be accepted and played respectively by the type that conforms to it.**


### What are the caveats in Object Oriented Programming?
- Abstraction, Encapsulation, Inheritance & Polymorphism are the base of OOPs but are **only supported by Object types (ie. class)**, value types like structure and enums cannot utilize these features and hence are just used as data store.

- **Value types are thread-safe** but due to the above mentioned short coming of OOPs in using value types efficiently we have to use Object types and then tackle thread safety with objects(references).


### How Protocol Oriented Programming makes the difference?
**Problem:**

Your boss comes in and demands creating an application were there are two kinda actors Painter and Singer, he further adds that during the course of time more actors will be added to the system. (deliberately I have used the word *actors* and NOT *objects* as that may be misinterpreted with reference type)

**Solution in OOPs:**

- Considering the factor that *during the course of time more actors will be added to the system*, we consider creating an Artist class and then inherit Painter and Singer from it.

Here a Painter and a Sculptor inherits from their parent, Artist, they both get the common features implemented by the Artist implicitly.
![Hello](https://github.com/CodeblazeTechnologies/guidelines/blob/master/Swift/support_images/pop_solution.png)
