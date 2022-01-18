# Baiscs

## 1. Different types: OOP vs Functional programming

Ref: [Medium-blog](https://medium.com/@shaistha24/functional-programming-vs-object-oriented-programming-oop-which-is-better-82172e53a526)

- Definition
  - Functional emphasis function
  - OOP based on "object"
- Data
  - Functional programming use immutable data
  - OOP support both mutable data and immutable
- Model
  - Functional follow declarative programming model
    - Declarative [[dɪˈklærətɪv]: 公告的，陈述的
  - OOP follow imperative programming model
    - Imperative [ɪmˈperətɪv]: 强制的, 命令的

### 1.1. Mutable vs immutable

Ref: [ImaginaryCloud-blog](https://www.imaginarycloud.com/blog/functional-programming-vs-oop/#mut)

- Mutable (data/object): data can be modified after created
  - E.g. general object
  - Pro: flexiable, can be changed without additional allocations, easy in dev.
- Immutable (data/object): data cannot be modified after created
  - E.g. numbers, booleans, frozensets, strings, and tuples object in python. 
  - Pro: fixed once created, can be used for multi-thread applications, not changed

### 1.2. Declarative vs Imperative

Ref: [ImaginaryCloud-blog](https://www.imaginarycloud.com/blog/functional-programming-vs-oop/#imperative), [StackOverflow](https://stackoverflow.com/questions/1784664/what-is-the-difference-between-declarative-and-imperative-paradigm-in-programmin)

- Declarative programming: declare the result but not the detailed processes.
  - Personal understanding: e..g just call a function to get the result. During the process you can get result from the function, but not-explicitly-know/have-no-access to what happens inside the function.
  - E.g.

    ```python
    def get_odds(list):
        Ans = [num in list if num%2==0]
        Return ans
    
    Odds = get_odds(list)
    ```

- Imperative programming: tell the program how to get result step by step.
  - E.g.

  	```python
  	Odds =  [num in list if num%2==0]
    ```

## 2. Different OOP languages

### 2.1. Python vs Java

Ref: [blog](https://www.bmc.com/blogs/python-vs-java/)

- (Syntax?) type:
  - Java is compiled language
  - Python is interpreted language
- type-checking
  - Java Static-typed
  - Python dynamic-typed



### Type-checking: static vs dynamic

type-checking: the process verifying and enforcing the constraints of type. Ref: [Wiki-Type_checking](https://en.wikipedia.org/wiki/Type_system#Type_checking)



- Static-typing: 
  - Perform type-check at compile time
  - Declare data-type before you use
  - Pro: more safe (less bugs) at run time.
- Dynamic-typing
  - Perform type-checking at run time
  - No need to declare data-type.
  - Pro: easy to write, higher dev efficiency.
- Ref: [Oracle-doc](https://docs.oracle.com/cd/E57471_01/bigData.100/extensions_bdd/src/cext_transform_typing.html)
						

### (Syntax?) Type: complied vs interpreted

Ref: [FreeCodeCamp-blog](https://www.freecodecamp.org/news/compiled-versus-interpreted-languages/), [GeeksForGeeks](https://www.geeksforgeeks.org/difference-between-compiled-and-interpreted-language/)

- Compiled: not directly run the code, need a "build" step that directly covert code to machine code direclty executable by processor.
  - E.g. C, C++, Java, etc.
  - Pro: 
    - more control on hardware like memory or cpu, 
    - Usually more efficient than interpreted language
    - Compilation error can be found at compilation, debugging easier.
- Interpreted: the code run line by line, don't need the "build" step
  - Pro: higher dev - efficiency, more readable.
  - Con: All error reported & debug while running, slower than compiled language.

**Note**: A dynamic type-checking language is not necessarily a interpreted language. Dynamic typec-checking language can be compiled language. 
- "Dynamic vs Static" is **NOT** related to "Compile vs interpreted", they are different ideas:
  - "Dynamic vs Static": when type-checking happens
  - "Compile vs interpreted": how the written code is executed. they are different ideas. 
  - Ref: [StackExchange](https://softwareengineering.stackexchange.com/questions/88645/are-dynamic-languages-always-interpreted)
- E.g.
  - Lisp: compiled and dynamic type-checking 
  - C# (C sharp): compiled and dynamic type-checking
- Further reading:
  - [StackExchange](https://softwareengineering.stackexchange.com/questions/332214/can-an-interpreted-statically-typed-language-be-considered-type-safe): Seems ""interpreted" and "static type-check" are exclusive:
    - static type-checking language canNOT be interpreted
    - interpreted canNOT be static type-checking
