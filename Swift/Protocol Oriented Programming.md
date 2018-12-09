# Protocol Oriented Programming


### What's a Protocol?
> As per Apple's The Swift Programming Language (Swift 4.0.3)

A protocol defines a blueprint of methods, properties, and other requirements that suit a particular task or piece of functionality. The protocol can then be adopted by a class, structure, or enumeration to provide an actual implementation of those requirements. Any type that satisfies the requirements of a protocol is said to conform to that protocol.
  In my words, **Protocols are rules and roles that are to be accepted and played respectively by the type that conforms to it.**


### What are the caveats in Object Oriented Programming?
- Abstraction, Encapsulation, Inheritance & Polymorphism are the base of OOPs but are **only supported by Object types (ie. class)**, value types like structure and enums cannot utilize these features and hence are just used as data store.

- **Value types are thread-safe** but due to the above mentioned short coming of OOPs in using value types inefficiently we have to use Object types and then tackle thread safety with objects(references).


### How Protocol Oriented Programming makes the difference?
**Problem:**

Your boss comes in and demands creating an application were there are two kinda actors Painter and Singer, he further adds that during the course of time more actors will be added to the system. (deliberately I have used the word *actors* and NOT *objects* as that may be misinterpreted with reference type)

**Solution in OOPs:**

- Considering the factor that *during the course of time more actors will be added to the system*, we consider creating an Artist class and then inherit Painter and Singer from it.

**Here a Painter and a Sculptor inherits from their parent, Artist, they both get the common features implemented by the Artist implicitly.**

![OOPs design](https://github.com/CodeblazeTechnologies/guidelines/blob/master/Swift/support_images/pop_solution.png)



**Now if the requirement demands that some painter could be an sculptor too then the inheritance tree would look something like this**

![OOPs issues](https://github.com/CodeblazeTechnologies/guidelines/blob/master/Swift/support_images/issue_in_oops.png)

**But we are making every Painter a Sculptor, which forces irrelevant features of Sculptor in a Painter.**




**Solution in POPs:**

- The philosophy of POPs, *Start with a Protocol instead of a class*

**We declared three protocols here Artist, Painter and Sculptor. Painter and Sculptor inherits from Artist protocol. Each protocol has declared their requirements (ie. methods, properties)**

![POPs design](https://github.com/CodeblazeTechnologies/guidelines/blob/master/Swift/support_images/pop_solution.png)


**Here we define a struct named OnlyPainter which conforms to Painter protocol. Then We defined another struct PainterAndSculptor which conform to Painter and Sculptor protocol so that it can implement the required functionality of both.**

![Only Painter](https://github.com/CodeblazeTechnologies/guidelines/blob/master/Swift/support_images/OnlyPainter.png)

![Painter and Sculptor](https://github.com/CodeblazeTechnologies/guidelines/blob/master/Swift/support_images/PainterAndSculptor.png)


Protocol is a much cleaner approact against the inheritance. None of them are leaking any extra features into each others boundary. This is a Protocol-Oriented approach where the major things are driven by the protocols.

An advantage with protocols is that they can be extended to provide a default implementation of its requirements. For example, if some requirements of the protocol have common implementation across all of its conforming types, those requirements can be implemented by the protocol itself. All the conforming types get the default implementation for free and they do not need to implement it again.
