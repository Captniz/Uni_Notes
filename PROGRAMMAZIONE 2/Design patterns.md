---
Date created: "14-05-25 • 13:53"
tags: 
Related PDF/DOC: 
Related Pages:
---
     ## What is a design pattern
> A **design pattern** in programming is a **general, reusable solution to a common problem** in software design. It's not a finished piece of code, but rather a **template or blueprint** that can be adapted to fit specific problems in various situations.

### Common Types of Design Patterns

Design patterns are typically categorized into three main groups:
1. **Creational Patterns** – deal with object creation mechanisms.
    - Examples:
        - **Singleton**: Ensures a class has only one instance.
        - **Factory Method**: Delegates instantiation to subclasses.
        - **Builder**: Constructs a complex object step-by-step.
2. **Structural Patterns** – deal with object composition.
    - Examples:
        - **Adapter**: Allows incompatible interfaces to work together.
        - **Composite**: Treats individual objects and compositions uniformly.
        - **Decorator**: Adds behavior to objects dynamically.
3. **Behavioral Patterns** – focus on communication between objects.
    - Examples:
        - **Observer**: Notifies dependents of state changes.
        - **Strategy**: Allows selecting an algorithm at runtime.
        - **Command**: Encapsulates a request as an object.

## Patterns
### Template method
> The **Template Pattern** (also known as the **Template Method Pattern**) is a **behavioral design pattern** that defines the **skeleton of an algorithm** in a base class but lets **subclasses override specific steps** of the algorithm without changing its overall structure.

> [!example] Example
> > [!example] Pseudo-Code
> > ```pseudo
> > AbstractClass
├── templateMethod()   <- defines the steps of the algorithm
├── stepOne()
├── stepTwo()
└── stepThree()
> > ```  
> > - The `templateMethod()` is usually `final` or not overridden—it defines the **fixed sequence**.
> > - The steps ( `stepOne()`, `stepTwo()`, ... ) can be **implemented** or **overridden** by subclasses.
>
> > [!example] Python
> > ```py
> > class DataProcessor:
> > 	def process(self):
> > 		self.load_data()
> > 		self.process_data()
> > 		self.save_data()
> >
> > 	def load_data(self):
> > 		print("Loading data...")
> >
> > 	def process_data(self):
> > 		raise NotImplementedError
> >
> > 	def save_data(self):
> > 		print("Saving data...")
> >
> > class CSVProcessor(DataProcessor):
> > 	def process_data(self):
> > 		print("Processing CSV data...")
> >
> > class JSONProcessor(DataProcessor):
> > 	def process_data(self):
> > 		print("Processing JSON data...")
> > ```


Absolutely! The **State Pattern** is a **behavioral design pattern** that allows an object to **change its behavior when its internal state changes**—as if the object changed its class at runtime.

---

### 🧠 Core Idea:

Instead of using a bunch of `if` or `switch` statements to handle behavior based on state, the State Pattern puts the **state-specific behavior into separate classes**.

---

### 🔧 Real-World Analogy:

Think of a **traffic light**:

- It can be **Green**, **Yellow**, or **Red**.
    
- Each state has its own behavior (e.g. duration, what to do next).
    
- The light object **changes state**, and its behavior changes accordingly.
    

---

### 🏗️ Structure:
