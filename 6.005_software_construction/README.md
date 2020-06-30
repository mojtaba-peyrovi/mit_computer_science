

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
















> Written with [StackEdit](https://stackedit.io/).


