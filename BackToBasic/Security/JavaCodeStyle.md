title: Java Code Style
date: 2017-07-12 09:27:33
tags:
- basic
- java
- code style
---


# Reference Standard

## Google Java style

https://google.github.io/styleguide/javaguide.html

* Source files are encoded in UTF-8.
* Tab characters are not used for indentation; use two spaces
* A source file consists of, in order:

 * License or copyright information, if present
 * Package statement
 * Import statements
  > no wildcard imports

 * Exactly one top-level class
  > never split overloads classes
  > always use braces for if,else,for,do and while
  > style of braces

  ```java
  return () -> {
    while (condition()) {
      method();
    }
  };

  return new MyClass() {
    @Override public void method() {
      if (condition()) {
        try {
          something();
        } catch (ProblemException e) {
          recover();
        }
      } else if (otherCondition()) {
        somethingElse();
      } else {
        lastThing();
      }
    }
  };
  ```

  > line-wrapping normally at 100, but there are exceptions; wrapped lines indent 4+ spaces

 * __Exactly one blank line__ separates each section that is present
