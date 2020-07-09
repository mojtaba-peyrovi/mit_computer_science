

## Reading 4: Code Review
Programmers often describe bad code as having a “bad smell” that needs to be removed. “Code hygiene” is another word for this.

#### Don't repeat yourself (DRY):
Duplicated code is a risk to safety. If you have identical or very similar code in two places, then the fundamental risk is that there’s a bug in both copies, and some maintainer fixes the bug in one place but not the other.
>Never copy-and-paste your code.

#### Commenting:
Good software developers write comments in their code, and do it judiciously. Good comments should make the code easier to understand, safer from bugs and ready for change.

- One of the most important ones, is the specifications written on the top of the method, like this:


```
/**
 * Compute the hailstone sequence.
 * See http://en.wikipedia.org/wiki/Collatz_conjecture#Statement_of_the_problem
 * @param n starting number of sequence; requires n > 0.
 * @return the hailstone sequence starting at n and ending with 1.
 *         For example, hailstone(3)=[3,10,5,16,8,4,2,1].
 */
public static List<Integer> hailstoneSequence(int n) {
    ...
}
```
- Another kind of comment, is when we copy a snippet from another source, and adopted, we can mention it with the source.
```
// read a web page into a string
// see http://stackoverflow.com/questions/4328711/read-url-to-string-in-few-lines-of-java-code
String mitHomepage = new Scanner(new URL("http://www.mit.edu").openStream(), "UTF-8").useDelimiter("\\A").next();
```
One reason for documenting sources is to avoid violations of copyright, Another reason for documenting sources is that the code can fall out of date.

#### Fail Fast:
The code should reveal the the bug as early as possible, because it is easier to debug. That's why Java is a powerful language because it uses static checking rather than dynamic checking (Python.) Dynamic checking fails at the runtime, while static one fails at the time of writing the code.

#### Avoid Magic Numbers:
do not hard-code any constant, especially the ones coming from your own calculations. for example: never use 1,2,3,4 for month numbers, instead, use JANUARY, FEBRUARY, etc.

#### One Purpose for each Variable:
Do not reuse your parameter or variables. Stop using them when you stop needing them. You will confuse your reader if a variable that used to mean one thing suddenly starts meaning something different a few lines down.
Method parameters, in particular, should generally be left unmodified. use `final` for method parameters, and as many other variables as you can. 
```
public static int dayOfYear(final int month, final int dayOfMonth, final int year) {
    ...
}
```
#### Use Good Names:
Good method and variable names are long and self-descriptive. Comments can often be avoided entirely by making the code itself more readable, with better names that describe the methods and variables.
Example:
```
int tmp = 86400;  // tmp is the number of seconds in a day (don't do this!)
```
can be replaced by:
```
int secondsPerDay = 86400;
```
> In general, variable names like `tmp` , `temp` , and `data` are awful, symptoms of extreme programmer laziness.

Better to use a longer, more descriptive name, so that your code reads clearly all by itself. Remember to use the naming conventions for each language. For Java:
-   methodsAreNamedWithCamelCaseLikeThis
-   variablesAreAlsoCamelCase
-   CONSTANTS_ARE_IN_ALL_CAPS_WITH_UNDERSCORES
-   ClassesAreCapitalized
-   packages.are.lowercase.and.separated.by.dots

**Important:** 
a. Method names are usually verb phrases, like `getDate` or `isUpperCase` , while variable and class names are usually noun phrases.

b. Avoid abbreviations. For example, `message` is clearer than `msg` , and `word` is so much better than `wd` .


c. Never use tab characters for indentation, only space characters. The reason for this rule is that different tools treat tab characters differently — sometimes expanding them to 4 spaces, sometimes to 2 spaces, sometimes to 8. If you run “git diff” on the command line, or if you view your source code in a different editor, then the indentation may be completely screwed up. Just use spaces. Always set your programming editor to insert space characters when you press the Tab key.

#### Don't use global variables:
In Java, a global variable is declared  `public static` . The  `public` modifier makes it accessible anywhere, and  `static` means there is a single instance of the variable.

In general, change global variables into parameters and return values, or put them inside objects that you’re calling methods on. 

#### Methods should return results not print them:
In general, only the highest-level parts of a program should interact with the human user or the console. Lower-level parts should take their input as parameters and return their output as results. The sole exception here is debugging output, which can of course be printed to the console.

## Reading 5: Version Controlling

some git commands or terms I didn't know:
1- Git show: shows the details of the latest commit. (the message and the modifications)
2- Git commit: without -m we can commit, then the editor pops up and asks for inputting the message.
3- Staged vs unstaged changes: unstaged means not added. staged means already added but could be committed or not committed.
4- Git diff: shows the difference between the unstaged changes with the latest commit.
5- Git diff --staged: shows the difference between the staged changes(but not committed) with the latest commit.
[here](https://git-scm.com/book/en/v2) is the full resource about Git.


## Reading 6: Specifications

types of specifications:
-   a  _precondition_ , indicated by the keyword  _requires_  The precondition is an obligation on the client
-   a  _postcondition_ , indicated by the keyword  _effects_ The postcondition is an obligation on the implementer of the method.
In order to write specifications, we should put the preconditions into `@param` where possible, and postconditions into `@return` and `@throws` . So a specification like this:

Here is an example:
```java
/**
 * Find a value in an array.
 * @param arr array to search, requires that val occurs exactly once
 *            in arr
 * @param val value to search for
 * @return index i such that arr[i] = val
 */
static int find(int[] arr, int val)
```
#### Javadoc: 
In Java programming, we can use the **javadoc** tool for generating API documentation from comments embedded in source code (Javadoc comments).  
For using it, Eclipse has  a **Javadoc Generation** wizard.  
[here](https://www.codejava.net/ides/eclipse/how-to-generate-javadoc-in-eclipse](https://www.codejava.net/ides/eclipse/how-to-generate-javadoc-in-eclipse) is a link about how to create javadoc in Eclipse.

 - Null values are troublesome and unsafe, so much so that you’re well advised to remove them from your design vocabulary.

#### Guava: Google Core Libraries for Java
Guava is a set of core Java libraries from Google that includes new collection types (such as multimap and multiset), immutable collections, a graph library, and utilities for concurrency, I/O, hashing, caching, primitives, strings, and more! It is widely used on most Java projects within Google, and widely used by many other companies as well.

A specification of a method can talk about the parameters and return value of the method, but it should never talk about local variables of the method or private fields of the method’s class. In Java, the source code of the method is often unavailable to the reader of your spec.


## Reading 7: Designing Specifications
For providing specs, we need to measure three things:
- How **deterministic** it is. Does the spec define only a single possible output for a given input, or allow the implementor to choose from a set of legal outputs?
- How **declarative** it is. Does the spec just characterize _what_ the output should be, or does it explicitly say _how_ to compute the outpu
- How **strong** it is. Does the spec have a small set of legal implementations, or a large set?

Deterministic:
Imagine this method which finds the index in a list of integers, if the value is equal to a given integer:
```
static int find(int[] arr, int val) {
    for (int i = 0; i < arr.length; i++) {
        if (arr[i] == val) return i;
    }
    return arr.length;
}
```
this spec is deterministic:
```
  _requires_: val occurs **exactly once** in arr
  _effects_:  returns index i such that arr[i] = val
```
because the input and output are completely determined. But the following is not deterministic:
```
 requires: val occurs in arr
  effects:  returns index i such that arr[i] = val
```
Because it doesn’t say which index is returned if `val` occurs more than once.
The ones that are not deterministic like the case above, we call them **Underdetermined.** but they are different from nondeterministic.
**nondeterministic specs:** sometimes behaves one way, and sometimes another way. for example if the method is using a random number, or has date as its input.
**Operational Specs:** They give a series of steps that the method performs.
**Declarative Specs:** They just give properties of the final outcome, and how it’s related to the initial state.
>Almost always, declarative specifications are preferable. They’re usually shorter, easier to understand, and most importantly, they don’t inadvertently expose implementation details that a client may rely on.
One reason programmers sometimes lapse into operational specifications is because they’re using the spec comment to explain the implementation for a maintainer. Don’t do that. When it’s necessary, use comments within the body of the method, not in the spec comment.

**Strong vs Weak conditions:**  If you want to update a spec, you can always weaken the precondition; placing fewer demands on a client will never upset them. And you can always strengthen the post-condition, which means making more promises.
For example this condition:
```
_requires_: val occurs **exactly once** in a
  _effects_:  returns index i such that a[i] = val
```
can be replaced with:
```
  _requires_: val occurs **at least once** in a
  _effects_:  returns index i such that a[i] = val
```
which has weaker preconditions, but we can make the effects part (implementor) to be:
 ```
  _effects_:  returns lowest index i such that a[i] = val
 ```
 It has a stronger postcondition.
 - The 	specifications should be abstract types where possible. Writing our specification with _abstract types_ gives more freedom to both the client and the implementor. In Java, this often means using an interface type, like `Map` or `Reader` , instead of specific implementation types like `HashMap` or `FileReader`.
 What if the client wants to use another kind of map, but you explicitly say you expect a hashMap?
 **About Access Controlling:**
- Keeping internal things _private_ makes your class’s public interface smaller and more coherent (meaning that it does one thing and does it well). Your code will be **easier to understand** and **safer from bugs.**


## Reading 8: Avoiding Debugging

The very first defense is:
>Make bugs impossible by design

One way of making bugs impossible, is **static checking.** Other languages like C or Python silently let the bad access happen, and they have security vulnerabilities.
The other design principle is **Immutability** which means not to let variables to be overwritten after creation.

second defense:
>Localize bugs, by defining error handling

Example:
```java
/**
 * @param x  requires x >= 0
 * @return approximation to square root of x
 */
public double sqrt(double x) { ... }
```
In order to localize the potential bugs to be easily found, we can make sure the parameter passed into the method, is positive, and if it is negative we can return an assertion error:
```java
/**
 * @param x  requires x >= 0
 * @return approximation to square root of x
 */
public double sqrt(double x) { 
    if (! (x >= 0)) throw new AssertionError();
    ...
}
```
Checking preconditions is an example of **Defensive Programming.**

In order to enforce the precondition of a method, we can assert the condition and the result of it can be a record in log, break the program, or email the maintainer.
 >An assertion is executable code that enforces the assumption at runtime.

We can define the simple assertion like this:
```java
assert x >= 0;
```
or also we can add more details to it by writing some description following the main assertion by a colon, e.g.
```java
assert (x >= 0) : "x is " + x;
```
if x== -1, the assertion result will be:
> x is -1

In Java, the assertions are off by default. you have to turn them on by passing the argument -ea to the Java VM. In eclipse, add -ea here:
> Preferences → Java → Installed JREs → Edit → Default VM Arguments

IMPORTANT:
>Never use assertions to test conditions that are external to your program, such as the existence of files, the availability of the network, or the correctness of input typed by a human user.
>External failures are not bugs, and there is no change you can make to your program in advance that will prevent them from happening. External failures should be handled using exceptions instead.

### Incremental Development:
A great way to localize bugs to a tiny part of the program is incremental development. Build only a bit of your program at a time, and test that bit thoroughly before you move on. That way, when you discover a bug, it’s more likely to be in the part that you just wrote, rather than anywhere in a huge pile of code.

### Other Techniques to Avoid Bugs:
**Modularity:** Modularity means dividing up a system into components, or modules, each of which can be designed, implemented, tested, reasoned about, and reused separately from the rest of the system. The opposite of a modular system is a monolithic system – big and with all of its pieces tangled up and dependent on each other.
>A program consisting of a single, very long main() function is monolithic – harder to understand, and harder to isolate bugs in. By contrast, a program broken up into small functions and classes is more modular.

**Encapsulation:** Encapsulation means building walls around a module (a hard shell or capsule) so that the module is responsible for its own internal behavior, and bugs in other parts of the system can’t damage its integrity.
One kind of encapsulation is **[access control](https://docs.oracle.com/javase/tutorial/java/javaOO/accesscontrol.html)** , using `public` and `private` to control the visibility and accessibility of your variables and methods.
Another kind of encapsulation comes from **variable scope** .
For example, suppose you have a loop like this:
```java
for (i = 0; i < 100; ++i) {
    ...
    doSomeThings();
    ...
}
```
and you’ve discovered that this loop keeps running forever – `i` never reaches 100. Somewhere, somebody is changing `i` . But where? If `i` is declared as a global variable like this:
```java
public static int i;
...
for (i = 0; i < 100; ++i) {
    ...
    doSomeThings();
    ...
}
```
then its scope is the entire program. It might be changed anywhere in your program: by `doSomeThings()` , by some other method that `doSomeThings()` calls, by a concurrent thread running some completely different code. But if `i` is instead declared as a local variable with a narrow scope, like this:
```java
for (int i = 0; i < 100; ++i) {
    ...
    doSomeThings();
    ...
}
```
then the only place where `i` can be changed is within the for statement.
>**Minimizing the scope of variables** is a powerful practice for bug localization.

Here are a few rules that are good for Java:
1- **Always declare a loop variable in the for-loop initializer.** So rather than declaring it before the loop:
```java
int i;
for (i = 0; i < 100; ++i) {
```
which makes the scope of the variable the entire rest of the outer curly-brace block containing this code, you should do this:

```java
for (int i = 0; i < 100; ++i) {
```
which makes the scope of `i` limited just to the for loop.
2- **Declare a variable only when you first need it, and in the innermost curly-brace block that you can.** Variable scopes in Java are curly-brace blocks, so put your variable declaration in the innermost one that contains all the expressions that need to use the variable. Don’t declare all your variables at the start of the function – it makes their scopes unnecessarily large. But note that in languages without static type declarations, like Python and Javascript, the scope of a variable is normally the entire function anyway, so you can’t restrict the scope of a variable with curly braces, alas.
3- **Avoid global variables.** Very bad idea, especially as programs get large.
 







