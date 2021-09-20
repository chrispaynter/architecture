# Modules (A.K.A Packages)

- Can look at a MODULE without being overwhelmed by the whole
- Can look at relationships between MODULES in views that exclude interior detail.
- The MODULES in the domain layer should emerge as a meaningful part of the model, telling the story of the domain on a large scale.
- There should be:
  - low coupling between modules
  - high cohesions within them.
- It's not code being divided into MODULES, but concepts.
- There is a limit to how many things a person can think about at once (hence low coupling).
- Incoherent fragments of ideas are as hard to understand as an undifferentiated soup of ideas (hence high cohesion).
- **Whever two model elements are separated into different modules, the relationships between them become less direct than they were**
  - Inreases the overhead of understanding their place in the design.
  - Low coupling between MODULES minimises this cost
    - Makes it possible to analyse the contents of one mODULE with a minimum of reference to others that interact.
  - The high cohesion of objects with related responsibilities allows modeling and design work to concerntrate within a single MODULE
    - A scale of complexity a human mind can easily handle.
- Like everything in DDD, MODULES are a *communications mechanism*.
  - The meaning of the objects being partitioned needs to drive the choice of MODULES.
  - 💭 When you place some classes together in a MODULE, you are telling the next developer who looks at your design to think about them together.
  - If your model is telling a store, your MODULES are chapters.
  - These names enter the UBIQUITOUS LANGUAGE
    - 💡 **"Now let's talk about the 'customer' module"** you might say to a business expert, and the context is set for your conversation.

## Therefore

- Choose MODULE names that tell the story of the system and contain a cohesive set of concepts.
  - This often yields low coupling between MODULES
  - If it doesn't, look for a way to change the model to disentangle the concepts
  - Search for an overloaded concept that might be the basis of a MODULE that would bring the elements together in a meaningful way.
- Seek low coupling
  - Concepts that can be understood and reasoned about independently of each other.
  - Refine the model till it partitions according to high-level domain concepts and the corresponding code is decoupled as well.
- Give the MODULES names that become part of the UBIQUITOUS LANGUAGE
  - MODULES and their names should reflect insight into the domain.

## Conclusion

- Resist the temptation to add anything to the domain objects that does not closely relate to the concepts they represent.