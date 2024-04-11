- aims for clean code that works
- isolated tests!
- test first, assert first. try writing assertions first
- log strings are useful when expectations need to come in order
- triangulation -> abstract only when you have 2 or more examples. 
most of the time obvious implementation or fake it will make more sense, 
only use triangulation when you are really, really unsure about the abstraction
- design patterns
  - command
    - when we need invocation to be a little more concrete and manipulable, 
    make an object representing the invocation, ex: Java Runnable interface
  - value object
    - objects that will be shared and their identity is not important, only the value is.
    - all value objects have to implement equality, because identity is not important, only the values!
  - null object
    - representing the special cases as objects instead of nulls, nils
  - template method
    - setUp(), teardown() in tests
  - pluggable object
    - removing conditional duplications spread across at different parts of the code 
    using an object and using on that different parts instead of checking w/ if/else
  - factory method
    - creating an object in a method instead of a constructor
    - downside of this pattern is that it is indirect, hard to remember that it is actually creating an object.
    - only useful if the flexibility it provides is worth it
  - imposter
    - an object with the same protocol as an existing object but a different implementation
  - composite 
  - collecting parameter
    - passing a parameter to a method and collect data from it 

- long setup code -> objects are too big, need to split
- setup duplication -> objects are too tightly intertwined
- long-running tests -> test that run a long time, won't run that often...
- fragile tests -> tests should not break unexpectedly, if some tests are affecting other tests you need to tackle that problem and than continue.