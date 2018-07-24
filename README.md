Chris Warrick’s Style Guide
===========================

**Revision 4 (2018-07-24)**

This document lists coding conventions and standards followed by Chris Warrick.
This guide is highly opinionated and does not have to be followed by anyone
else.

*The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
"SHOULD NOT", "RECOMMENDED",  "MAY", and "OPTIONAL" in this document are to be
interpreted as described in RFC 2119.*

This document is licensed under [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/).

Basic principles
----------------

The most important principles of this document, and any good code style, are:

* (R) readability
* (C) consistency
* (E) ease of application
* (I) IDE-friendliness

Code should be readable (cf. [PEP
20](https://www.python.org/dev/peps/pep-0020/)).  Code that is written without
readability in mind becomes a maintenance burden in the future.

Inconsistency in similar situations (eg. variable name style vs class member
name style) means that the developer must spend more time remembering which
rules to apply. It can also make automated refactoring less automated, if the
IDE is not geared to apply a code style for the developer.

Ease of application is the most important factor. When faced with rules that
require special work from the developer, the developer is more likely not to
apply them, and to produce inconsistent code.

---

If a specific topic is not mentioned in this code, the following guides SHOULD
be followed instead, if applicable:

* [PEP 8](https://www.python.org/dev/peps/pep-0008/)
* [Google Style Guide](https://google.github.io/styleguide/) for the specified
  language
* Other standard guidelines for the specific language.

The stated rules may, of course, be broken, if they are not compatible with a
language. Example: in C++98-compatible code, `vector< pair<int, int> >` is
allowed, even though it breaches spacing rules, although using C++98 breaches
the modern code rule.

I. Indentation, layout and blocks
=================================

A. Indentation and braces
-------------------------

1. (R) All blocks MUST be indented by **four spaces**. Vertical tabs (^I) MUST NOT
   be used to indent code. File-wide structures (namespaces, `extern "C"`) MAY
   be left unindented.
2. Continuation lines can use two styles, as per PEP 8:

   * hanging indent: continuation lines MUST be indented by 4 more spaces than
     the start of the statement. If the next line is indented by that many due
     to being a new block, use 8 more spaces instead. The line on which the
     statement starts should have no arguments.
   * visual indent: continuation lines are indented to appear directly below
     the thing they are continuing.

3. (C, E) Java/1TBS-style braces are used.

   * braces MUST NOT be ommitted, even for one-line statements
   * for multi-line blocks, including both control strucures and functions/classes/namespaces:
   * braces MUST be on the same line as the opening statement
   * in multi-branch structures (eg. `if-else if-else`, `try-catch-finally`, the further branches MUST have brackets on both sides of the keyword
   * in `do...while` construct, `while` MUST be on the same line as the closing brace
   * if multiple blocks are closed at the same time, their braces MUST appear on different lines
   * empty blocks SHOULD be written on one line (as `{}`)
   * the same rules apply to other structures, eg. Python list literals (so
     closing brackets are not indented)


Example I.1 (Java):

```java
public void setTimeout(int seconds) {
    if (number < 0) {
        throw new NegativeTimeoutException(seconds);
    } else if (number < 100) {
        setShortTimeout(seconds);
    } else {
        setLongTimeout(seconds);
    }
}
```

Example I.2 (C):

```c
int read_byte_from_file(FILE *fd, int *byte) {
    if (fd == NULL) { return ERROR_FILE_CLOSED; }
    byte = fgetc(fd);
    return SUCCESS;
}
```

Example I.3 (Python list literals):

```python
foo = [
    1,
    2,
    3
]
```

B. Line lengths and breaks
--------------------------

1. (R) Lines SHOULD be limited to 120 characters. This limit may be broken in
   cases where the readability of a line is negatively affected. A lower limit
   of 100 or 79 columns MAY be adopted if desired. (Don’t forget that modern
   text editors support soft word wrap. If yours does not, it’s high time to
   change it.)
2. In Python, parentheses are preferred to backslashes for line continuation
   where applicable.
3. Lines SHOULD be broken before operators. (See Example I.4)
4. At least one blank line SHOULD be between functions, classes, and blocks.
   Blank lines SHOULD also be used for logical separation of code.
5. Multiple statements SHOULD NOT be written on one line. *Exception:*
   the three statements in a `for (init; condition; increment)` clause,
   one-line `if`s (sparingly, and never in Python).

C. Spaces
---------

1. (R) Spaces MUST be placed before and after ALL binary (two-argument)
   operators (`foo + bar - baz`) and each element of ternary operators
   (`condition ? true_expr : false_expr`).

   Spaces SHOULD NOT be placed between an unary operator and its argument
   (`!foo`, `bar--`)

2. (R) Spaces MUST NOT be placed between:
   * the function name and argument list opening parenthesis,
   * the argument list opening parenthesis and the first argument,
   * the last argument and the argument list closing parenthesis,
   * the argument list (See Example I.4)
3. (R) Spaces MUST be placed after commas, colons and semicolons, unless they are at
   the end of a line. Spaces MUST NOT be placed before commas, colons and
   semicolons. (See Example I.4)
4. (R) Spaces MUST NOT be placed after opening quotes/brackets (of any type) and
   before closing quotes/brackets. (Newlines and indentation does not count.)
5. (R) Unless syntax rules require it, there MUST NOT be trailing whitespace at
   the end of lines.
6. Python-specific exceptions:

   * no spaces before/after colons in a Python slice
   * no spaces before/after `=` when listing default arguments, but not when
     annotations are used
     (cf. PEP 8)

Example I.4:

```
// C++
some_function(foo, bar, baz + quux);
int sum_and_count = 0;
for (int i = 0; i < 5; i++) {
    sum_and_count = sum_and_count
                    + i;
}

# Python
some_dict = {'foo': bar, 'baz': quux}
some_string = ("hello "
               "world")
```

II. Naming variables, classes, and other entities
=================================================

A consistent and simple entity naming style is crucial.

| Feature                      | Python                        | C, C++                            | Java                          | Swift              | C#                 |
|------------------------------|-------------------------------|-----------------------------------|-------------------------------|--------------------|--------------------|
| Classes, structs, enums      | `PascalCase`                  | `PascalCase`                      | `PascalCase`                  | `PascalCase`       | `PascalCase`       |
| Functions, methods           | `snake_case`                  | `snake_case`                      | `camelCase`                   | `camelCase`        | `PascalCase`       |
| Variables, fields, arguments | `snake_case`                  | `snake_case`                      | `camelCase`                   | `camelCase`        | `camelCase`        |
| Constants, enum members      | `UPPER_SNAKE_CASE`            | `UPPER_SNAKE_CASE`                | `UPPER_SNAKE_CASE`            | `UPPER_SNAKE_CASE` | `UPPER_SNAKE_CASE` |
| Folders/packages             | `snake_case` or `runtogether` | `snake_case` or `runtogether`     | `snake_case` or `runtogether` | `PascalCase`       | `PascalCase`       |
| Filenames                    | `snake_case`                  | `snake_case` (`.cpp`, `.c`, `.h`) | `PascalCase`                  | `PascalCase`       | `PascalCase`       |

1. (C) The base naming scheme is based upon the language conventions. Every
   language has a different set of rules
2. (C) The scheme MAY be overriden if the code is primarily based on a library
   that uses different convention (eg. Qt-based code may use `camelCase`)
3. (R) Names MUST represent the entities named by them.
4. (R, E) Using prefixes and suffixes is NOT RECOMMENDED. Hungarian notation
   MUST NOT be used.
5. (R) Single-letter variable names MUST NOT be used. *Exception:* iterator
   variables, but only if there is one level (typically `i`). Thus, `i` and `j`
   MAY NOT be used together. With iterator-based `for` loops, prefer  `for
   horse in horses:` over `for h in horses:`
6. (R) Entities in code MUST be named in English. Strunk and White does not
   apply. *Exception:* entities that are country-specific (implementing laws,
   taxes specific to a country) SHOULD be written in the native language to
   make them non-ambiguous. In those cases, the language used for the
   surrounding code MAY also not be English.
7. (R, E) Entity names MUST be encodable in ASCII.
8. (R) Uppercase abbreviations SHOULD be left as-is (eg. `XMLHTTPRequest` is
   better than `XmlHttpRequest`, and much better than the real API,
   `XMLHttpRequest`).  However, `iOSFooBar` MUST NOT be used.

A. Prefixes and suffixes
------------------------

1. (R, E, I) In general, prefixes MUST NOT be used due to negative effects on
   autocompletion and formatting.
2. (R, E, I) Any form of Hungarian notation MUST NOT be used.
3. Prefixes for all entities with something to distinguish the library (cf.
   `QLabel`, `QWidget` in Qt) MUST NOT be used in languages that support
   namespacing, modules, or other ways to distinguish them without prefixes.
   In other languages, they are allowed but NOT RECOMMENDED.
4. Suffixes are discouraged. They are allowed in the following cases:

   1. When the function works with the same variable in multiple types. The
      main (most important) variable SHOULD NOT have a suffix. For example,
      `time_str = time_input.text; time = Time(time_str)`
   2. In GUI code, form components MAY be suffixed with their type (eg.
      `_input`, `_label`).
   3. When using an out-parameter, `_out` SHOULD be used to highlight the fact.
   4. In C++, references MAY be suffixed by `_r` or `_out` (see rule 3 above)
   5. In C/C++, pointers MAY be suffixed by `_p`. The letter `p` MAY be repeated
      in the case of multiple pointers, eg. `int **foo_pp`. Use only when this
      enhances readability, eg. when the same variable is used as pointer and
      as a regular variable, or a single vs double pointer.

III. Comments and documentation
===============================

1. Comments SHOULD make it easier to understand what the code does. They MAY
   also be used to make it able to tell at a glance what a given segment does.
2. Comments and documentation MUST be written in English. Same exceptions as in
   II.A.6 apply.
3. (E, I) End-braces of functions/blocks SHOULD NOT be documented with a repeat
   of the function/block declaration, since those are prone to becoming
   outdated. Modern IDEs can describe the block currently under cursor.
4. Documentation of function/classes/methods in the standard language style
   (Python docstrings, JavaDoc, etc.) is HEAVILY RECOMMENDED. However, if the
   documentation would be a repeat of the entity name, it MAY be omitted.
   The same rules apply to arguments or return values. (Example: a
   `get_current_foo` method does not need documentation, since it would say
   `Get current Foo`, which is unhelpful.)
5. (E) Limit the use of special commenting structures. Instead of starting every
   function with 10 lines of argument/return/exception details (half of which
   are empty), only document the important things, and only if they actually
   occur.

IV. Syntax rules and technical stuff
====================================

1. `goto` MUST NOT be used.
2. (R) Constructs like `while (line = read() != null)` (using assignment as an
   expression) are NOT RECOMMENDED and should be used sparingly. In Python, the
   `:=` operator MUST NOT be used.
3. Modern language features and standards are heavily RECOMMENDED. This means
   Python 3.3+, Java 8+, C++17 (or at least C++11).  *Versions as of July
   2018.*
4. **JavaScript:** Even though semicolons are not necessary, their use is
   RECOMMENDED due to the automatic insertion being error-prone.
5. Use Unix-style (LF) newlines and UTF-8 without BOM as the file encoding, if
   possible.

A. Exceptions and error handling
--------------------------------

1. (R, E, I) Prefer exceptions to returning error codes. Using exceptions makes
   maintenance easier and reduces clutter. They also improve debugging in many
   cases due to available stack traces. When working with lower-level code,
   converting error codes to exceptions is welcome.
   ([Rationale](https://nedbatchelder.com/text/exceptions-vs-status.html))
2. Exceptions MUST NOT be caught if they cannot be reasonably handled, unless
   to throw a different exception with better error information.
3. (C) Displaying errors MUST be done in UI-related code only, with either a
   catch-all handler or function-specific handlers.

B. The Python section
---------------------

1. Lambdas SHOULD NOT be assigned to variables. If a lambda is to
   be instantly and explicitly named, use a regular function definition
   instead.
2. Using type hinting is encouraged, although not mandatory. Type
   hints SHOULD NOT be used if they hinder readability.
3. Single leading underscores to make stuff private, as per
   convention, are allowed. Double leading underscores (without trailing
   underscores), and therefore name mangling, MUST NOT be used.
4. A bare `except:` MUST NOT be used. `except Exception (as e):` is NOT
   RECOMMENDED if a narrower exception can be caught.
5. Single quotes for strings are RECOMMENDED, but not mandatory, especially if
   the string contains single quotes (so escaping can be avoided).
6. PEP 572 (Assignment Expressions, the `:=` operator) MUST NOT be used,
   because it severely hinders code readability.
