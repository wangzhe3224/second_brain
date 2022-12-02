
# Software Construction

Three targets in software constructions:

- Safe from bugs
- Easy to understand
- Ready for change

(
  Some design patterns mentioned:

  - Visitor
  - Interpreter
  - Composite
  - Message-passing
)

## Specifications

The `spec` is a contract: the implementer is responsible for meeting the contract,
and a client that uses the method rely on the contract. 

`spec` shields the client from the details of the module; it shields the implementer
from details of the usage of the module. `spec` decouples user and coder. 

( is this called abstraction? )

![Specs](https://raw.githubusercontent.com/wangzhe3224/pic_repo/master/images/20221120184205.png)

`spec` contains:

- function signature: name, inputs type, return type, exceptions (?)
- requires clause: restrictions on the inputs
- effect clause: return, exception, or other effects of the function call

When the require clause ( precondition ) is not meet, we should throw immediately.

## Design Specifications

Three dimensions for specs:

- how deterministic it is?
- how declarative it is?
- how strong it is?

## Avoid debuging

- Static type checking
- Immutability
- Localize bugs
  - Fail fast
  - Assert
    - arg requirements
    - return value requirements
  - Modularity
    - Break system into components
  - Encapsulation
    - Expose only interfaces not the internal details
- Incremental dev
  - unit test: test single module
  - regression test: test multiple modules

## Abstract Data Type, ADT

ADT, enable us to separate how we use a data structure from the particular form
of the data structure itself. In another word, ADT is a way to do abstraction with
*data* whereas the *spec* is a way to do abstraction with functions.

( ADT is kind of a `spec`? )

Any time you write an ADT, its specs must not talk about the rep. The concrete variants of a recursive ADT are its rep, so the specs must not mention them.

- Abstraction
  - Omitting or hiding low-level details with a simpler, higher-level idea
  - Abstraction: A `spec` is an Abstraction between client and implementation.
- Modularity
  - Divide system into components that could be {item} separately:
    - designed
    - implemented
    - tested
    - reasoned about
    - reused
  - Modularity: unit test and specs helps make functions to modules
- Encapsulation
  - Build a wall around a module so that module is responsible for its own internal states
  - local variables
- Information Hiding
  - Hide the implementation of the module from other part of the system
  - with `spec` implementer can implement the function without leak the internal states
- Separation of Concerns
  - Making a feature the responsibility of a single module, rather than across multiple modules.
  - A good function `spec` is [[coherent]], meaning it is responsible for just one concern

![ADT](https://raw.githubusercontent.com/wangzhe3224/pic_repo/master/images/20221120170322.png)

ADT, is defined by operations that can be done on the data structure. 
There are 4 types operations:

- creator, from args to object
- producer, from object(s) to object
- observer
- mutator

A few, simple operations that can be combined in powerful ways, rather than lots
of complex operations.

It should not mix generic and domain-specific features.

## Abstraction Functions and Rep Invariants

[[Invariants]] is a property of a program that is always true. ADT is responsible for 
ensuring that its own invariants hold.

abstract type has two spaces of values:

- the space of abstract values and the space of representation values (rep values)
- every A value is mapped by some R values
- Some A value is mapped to more than 1 R value
- Not all R values are mapped.

![Relationship between Abstract and Rep Value](https://raw.githubusercontent.com/wangzhe3224/pic_repo/master/images/20221120181419.png)

[[Abstract Function]] maps R to A, $AF: R \rArr A$. 

[[Rep Invariance]] maps R to boolean, $RI: R \rArr boolean$.

**We could use ADT to replace precondition, to guarantee the precondition.**

### How to establish invariants?

- Make the invariant true for the initial state of the object, and
- Ensure all changes to the object keeps the invariant true.

Translate to ADT operations:

- creators and producers must establish the invariant for new objects;
- mutators, observers, and producers must preserve the invariant of existing objects;
- No representation exposure occurs.

[[Abstract Data Types]] (ADT) are at the heart of software construction for correctness, clarity, and changeability.

### Some procedure to follow

**Writing a method**

1. Spec. Function signature, pre-condition, and post-condition in doc.
2. Test. Create systematic test cases and run automatically.
    - test cases will give more resistance on changing spec.
    - make sure at least some test cases failing at first.
3. Implementation.

**Writeing a [[ADT]]**

1. Spec. Write `spec` for operations of the ADT.
2. Test. Test cases for operations.
3. Implementation. 
   - Choose rep.
     - private fields
   - rep invariants (RI)
   - abstract functions (AF)

**Writing a program**

1. Choose Data Types.
   - mutable or immutable?
2. Choose functions
   - from main 
   - then break it down to smaller ones
3. Specs. 
4. Tests.
5. Implement simple version first!
6. Iterate

## Define ADTs with Interfaces, Generics, Enums, and Functions

### Interface

An `Interface` is a list of method signatures without method body.
`Interface` can specify the contract for the client and nothing more. 
Hence we can use it to define ADTs.

A `Type` is a set of values, with associated operations.

"B is a `subtype` of A" means "every B is an A", in another words, 
"every B satisfies the spec of A."

### Generic types

### Enum

## Debug

- Min test cast to reproduce
- Slicing the problem
- Binary search

## Message-passing for Concurrency

JS or node runtime is a single thread but async concurrent model.

## Grammars and Regular Expressions

A `Grammar` defines a set of strings. The literal strings in a grammar are called
`terminals`, as they cannot be expanded any further.

A `Grammar` is described by a set of `productions`, where each production defines
a `nonterminal`. non-terminal is defined by other non-terminals, operators, and 
terminals.

``` text
non-terminal ::= expression of terminals, non-terminals, and operators
```

For example:

``` text
// * operator, x is a non-terminal
x ::= y*
```

Grammar can have recursion:

``` text
url ::= 'http://' hostname (':' port)? '/'
hostname := word '.' hostname | word '.' word
port ::= [0-9]+
word ::= [a-z]+
```

### Markdown and HTML

Markdown grammar:

```text
markdown := ( normal | italic ) * 
italic ::= '_' normal '_'
normal ::= text
text ::= [^_]*
```

### Regular Expression

A `regular grammar` has a special property: by substituting every nonterminal (except the root one) with its righthand side, you can reduce it down to a single production for the root, with only terminals and operators on the right-hand side.

A regular expression does away with the quotes around the terminals, and the spaces between terminals and operators, so that it consists just of terminal characters, parentheses for grouping, and operator characters. 

### Context-free Grammar

Not all context-free grammar is regular. In general, any language with nested 
structure is context-free but not regular.

## Parsers

Make expression text within a grammar to a Abstract Syntax Tree, AST.

## Little Language

When you need to solve a problem, instead of writing a program to solve that
specific problem, build a *language* that can solve a range of related problem.

Another powerful idea is *representing code as data*.

A *language* has greater flexibility than a mere program. Most of time, we call 
this kind of language as [[domain-specific language]] (DSL). 

Some DSL is external, meaning it has its own syntax and semantics; some DSL is 
internal to a general programming language, meaning the syntax and semantics are
as same as the host language.

### Design Patterns for Language

Interpreter Pattern:

- declare the operation as an instance method in the interface that defines the data type
- implement the operation in each concrete variant
- `Dynamic Dispatch` of the method calls is what powers this pattern

Composite Pattern:

- implement the operations recursively
- single component and group of components has the same interface

Visitor Pattern:

- A visitor implements `switch` over types
- A visitor represents a function over a recursive type
- A visitor act as a iterator

![20221127210611](https://raw.githubusercontent.com/wangzhe3224/pic_repo/master/images/20221127210611.png)

The Interpreter pattern makes it easier to add new variants, because we don’t have to change any of our existing code: we just need to implement all the various operations as methods in the new variant class.

The Visitor pattern makes it easier to add new operations. Instead of having to modify both the interface and every variant class, we just need to create a new, e.g., FormulaVisitor implementation with all the code for our new operation. There is no change to existing classes or interfaces.