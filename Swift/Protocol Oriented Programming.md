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

#### Solution in OOPs:

- Considering the factor that *during the course of time more actors will be added to the system*, we consider creating an Artist class and then inherit Painter and Singer from it.

**Here a Painter and a Sculptor inherits from their parent, Artist, they both get the common features implemented by the Artist implicitly.**

![OOPs design](https://github.com/CodeblazeTechnologies/guidelines/blob/master/Swift/support_images/pop_solution.png)



**Now if the requirement demands that some painter could be an sculptor too then the inheritance tree would look something like this**

![OOPs issues](https://github.com/CodeblazeTechnologies/guidelines/blob/master/Swift/support_images/issue_in_oops.png)

**But we are making every Painter a Sculptor, which forces irrelevant features of Sculptor in a Painter.**




#### Solution in POPs:

- The philosophy of POPs, *Start with a Protocol instead of a concrete type*

**We declared three protocols here Artist, Painter and Sculptor. Painter and Sculptor inherits from Artist protocol. Each protocol has declared their requirements (ie. methods, properties)**

![POPs design](https://github.com/CodeblazeTechnologies/guidelines/blob/master/Swift/support_images/pop_solution.png)


**Here we define a struct named OnlyPainter which conforms to Painter protocol. Then We defined another struct PainterAndSculptor which conform to Painter and Sculptor protocol so that it can implement the required functionality of both.**

![Only Painter](https://github.com/CodeblazeTechnologies/guidelines/blob/master/Swift/support_images/OnlyPainter.png)

![Painter and Sculptor](https://github.com/CodeblazeTechnologies/guidelines/blob/master/Swift/support_images/PainterAndSculptor.png)


Protocol is a much cleaner approact against the inheritance. None of them are leaking any extra features into each others boundary. This is a Protocol-Oriented approach where the major things are driven by the protocols.

An advantage with protocols is that they can be extended to provide a default implementation of its requirements. For example, if some requirements of the protocol have common implementation across all of its conforming types, those requirements can be implemented by the protocol itself. All the conforming types get the default implementation for free and they do not need to implement it again.



## Lets dive in!!
 Its monday morning and suddenly I get a call from your boss asking to create a game. He further adds that this game revolves around a Duck. Thats all what he said.....
 
### First step:
 So as per the philosophy of POPs, *Start with a Protocol instead of a concrete type*, I create a protocol Duck instead of class or struct Duck.

    protocol Duck
    {
        var name: String {get}
    }

Now I further thought that all Duck can swim, so I created protocol Swimable


    protocol Swimable
    {
       func swim()
    }


#### Protocol extending Protocol

Now we are making our protocol Duck extends to protocol Swimable, this will give every Duck the ability to Swim.

    protocol Duck: Swimable
    {
        var name: String {get}
    }
    
    
#### Extending Protocols With Default Implementations

As every Duck can swim, lets provide a default implementation to it. So whenever we create a Duck, it will by default has the implemented swim() method.

    extension Duck
    {
        func swim()
        {
           print("I am a duck who can swim!")
        }
    }
    
I was very happy about what I created, so I went to my boss and showed him. He really liked the idea of using protocols over the concrete type. He further added that now he wants me to do the following:

 - Create three kinda ducks
   - OrdinaryDuck who can Quack
   - RubberDuck who can Quack
   - NinjaDuck who canNOT Quack
   
Lets create Quackable protocol and Duck
                
    protocol Quackable
    {
        func quack()
    }
   
**Ordinary Duck**

    struct OrdinaryDuck: Duck, Quackable
    {
        var name = "Ordinary Duck"
    }

**Rubber Duck**

    struct RubberDuck: Duck, Quackable
    {
        var name = "Rubber Duck"
    }

**Ninja Duck**

    struct NinjaDuck: Duck
    {
       var name = "Ninja Duck"
    }
    
 **Here OrdinaryDuck and RubberDuck extends Quackable but not the NinjaDuck, so there are two ways we can implement the Quack method**
  
  - Implement quck() method in OrdinaryDuck and RubberDuck individually.
  - Implement quck() method in Duck.
  - Implement quck() method in a subtype that conforms to certain type.
  
  **Lets see which of the above option can work for us**
  
  - Implementation of quck() method in OrdinaryDuck and RubberDuck individually *would create an ambiguity, if the implementation is default.*
  - Implement quck() method in Duck would also *make NinjaDuck to quack, which we don't want*
  - Implement quck() method in a subtype that conforms to certain type is *the option that I would choose to go further with.*
  
 #### Extending Protocols With Default Implementations for a subset who conforms to certain requirements
 
    extension Duck where Self: Quackable
    {
       func quack()
       {
          print("Quack")
       }
    }
 
 After I have shown this to my boss, he was quite impressed and put me up against another challenge, he says that OrdinaryDuck does quack but RubberDuck should not quack but squeek....
 
 
 #### Overriding Default Behavior
 
     struct RubberDuck: Duck, Quackable
    {
        var name = "Rubber Duck"
     
       func quack()
      {
          print("Squeek")
      }
    }
    

### PATs the way!!
 PAT means Protocol with Associated Type, in simple terms its a protocol with generics. Lets take a look how it's gonna add value to the above scenario.
 
  Lets give our Ducks some powers..............
  
  **Power Trait**
    
    protocol PowerTrait
    {
       associatedtype Pow
       var power: Pow { get set }
    }

  **Pokemon**
  
    struct Pikachu: PowerTrait
    {
       typealias Pow = Int
       var power = Int()
    }

    var pika = Pikachu()
    print(pika.power)
    
 We can ease it with this below syntax
   
    struct Pikachu: PowerTrait
    {
       var power = Int()
    }
    
  **Output**
  
    0
    
 My boss was impressed with PAT but was upset that how come Integer can be a power in context of Pokemon..... So I thought of implementing some kinda constraint.
 
  **Power**
  
    protocol Power{}
    
  **Power Trait**
  
    protocol PowerTrait
    {
       associatedtype Pow: Power
       var power: Pow { get set }
    }
    
  This will restrict the associated types to Power.
  
  **The above are simple examples to showcase the power of Protocol Oriented Programming but it has lot in store.**

 
 
    
    
  
  
  Add OOPS is still a great help
