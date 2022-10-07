# CSE 11 Fall 2022 PA2 - Password Security
**Due date: Thursday, Oct 13 @ 11:59PM PDT**

There is an [FAQ post](https://piazza.com/class/l871flmwsqy8i/post/144) on Piazza. Please read that post first if you have any questions.

## Provided Files
- PasswordSecurity.java

## Goal
Programming Assignment 2 is an introduction to loops and Strings in Java. You will use loops, String methods, and other programming techniques to complete the assignment.

## Overview

- Password Security [Gradescope, 100 points]
    - Implementation [95 points]
    - Style [5 points]

## Password Security [100 Points]
As the world becomes more and more digital, many of us have an increasing number of accounts and passwords to manage. Having strong and unique passwords is an important step to protect our accounts.

In this programming assignment, we will evaluate the strength of passwords using a simplified model which will only calculate a password's strength based on the presence of uppercase letters, lowercase letters, numbers, and symbols. You can learn more about how password strengths are assessed in the real world [here](https://www.uic.edu/apps/strong-password/).

### Your Task
Write a program called `PasswordSecurity` that
- Reads a user's password 
- Evaluates the strength of the password
- Suggests a stronger password if their password is not strong enough

### Implementation [95 Points]

Your file should be named as `PasswordSecurity.java` and should contain a single class `PasswordSecurity` which has only 1 method: `main`.

#### Input
The program reads user inputs from `System.in` using a [Scanner](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/util/Scanner.html). When an user runs `java PasswordSecurity`, they will be prompted to type a password into our program, ended by a newline character. *(See examples below on how your input and output should be formatted.)*

You may safely assume that the characters in the password are within the [ASCII](https://www.rapidtables.com/code/text/ascii-table.html) range [33,126] (from `!` to `~`, inclusive) and the length of the entered password is $\leq$ 128.

#### Password Strength Evaluation

First, we need to validate the length of the entered password. If the password is less than 8 characters long, print `Password is too short` and exit the program.

Then, determine the presense of the following categories in the input password: *(you may find it helpful to count and store the number of characters for later)*
- Uppercase letters
- Lowercase letters
- Numbers
- Symbols

Which map to the following strengths:
- Very weak: having exactly 1 of the above categories
- Weak: having exactly 2 of the above categories
- Medium: having exactly 3 of the above categories
- Strong: having exactly all 4 of the above categories

For example, if a password only has uppercase and lowercase letters, it would be considered "Weak". A password that has uppercase, lowercase, numbers, and symbols would be considered "Strong".

Print the calculated password strength to standard output. *(See examples below on how you should format your output.)*

You may find methods in the [Character class](https://docs.oracle.com/en/java/javase/17/docs/api/java.base/java/lang/Character.html) helpful for determining which category a character falls under. For this PA, all characters in the ASCII range [33,126] that are not letters or numbers are considered as symbols.

#### Password Suggestion

If the user's password is not considered "Strong" by our evaluation model, we will suggest a stronger password for the user by applying these rules in the following order:
1. If there fewer than 2 letters, **prepend** `Cse` to the current password. If this rule is applied, skip Rule 2 and Rule 3.
2. If there are no lowercase letters, change the **first uppercase letter to lowercase**. If this rule is applied, skip Rule 3.
3. If there are no uppercase letters, change the **last occurrence** of the highest-ASCII-valued lowercase character to uppercase.
4. If there are no numbers, insert $k$ into the **current password** every 4 characters, where $k$ is the **original length** of the password mod 10.
*If the orginal length is divisible by 4, $k$ should be inserted at the very end as well.*
6. If there are no symbols, **append** `@!` to the current password.

*Note that only up to one of Rules 1-3 should apply to a given password.*

**Examples:**
#### Example 1:
```
$ java PasswordSecurity
Please enter a password: 12345678
Password strength: very weak
Here is a suggested stronger password: Cse12345678@!
```
*Rules 1 and 5 were applied.*

#### Example 2:
```
$ java PasswordSecurity
Please enter a password: paulcao<3
Password strength: medium
Here is a suggested stronger password: paUlcao<3
```
*Only Rule 3 was applied, the letter `u` is the highest-ASCII-value lowercase character.*

#### Example 3:
```
$ java PasswordSecurity
Please enter a password: PASSWORD???
Password strength: weak
Here is a suggested stronger password: pASS1WORD1???
```
*Rules 2 and 4 were applied.
Since the original password's length is 11, $k=11$ $mod$ $10=1$
The digit $1$ is inserted every 4 characters.*


#### Additional Implementation Requirements
- Write the output to the standard output. There should be a **newline character** at the end of each line. 
**Note:** `System.out.println()` method prints the text on the console and the cursor remains at the start of the next line at the console (i.e. it appends a newline character at the end of the line, so you **do not** need to add a newline character by yourself if you are using `System.out.println`). 
**Please follow the exact output format, or otherwise you will not get any credit.**
- You should not import anything other than `java.util.Scanner`. If you import any other libraries, you may get a 0 for this part, even though if you had passed all the test cases.
- You should not use `System.exit`. Doing so will cause Autograder to throw exceptions and you'll get "EXCEPT" for all of your test cases.
- You do not need to deal with cases where the suggested password MAY exceed 128 characters.


### Style [5 Points]
Coding style is an important part of ensuring readability and maintainability of your code. We will grade your code style in all submitted code files according to the style guidelines. Namely, there are a few things you must have in each file/class/method:

1. File header
2. Class header
3. Method header(s)
4. Inline comments
5. Proper indentation
6. Descriptive variable names
7. No magic numbers
8. Reasonably short methods (if you have implemented each method according to specification in this write-up, you’re fine). This is not enforced as strictly.
9. Lines shorter than 80 characters
10. Javadoc conventions (@param, @return tags, /** comments */, etc.)

A full style guide can be found [here](https://github.com/CaoAssignments/style-guide/blob/main/README.md) and a sample styled file can be found [here](https://github.com/CaoAssignments/style-guide/blob/main/SampleFile.java). If you need any clarifications, feel free to ask on Piazza.


### Additional Examples

#### Example 4: (Password length < 8)
```
$ java PasswordSecurity
Please enter a password: cse11
Password is too short
```
#### Example 5: (Strong password, no suggestion made)
```
$ java PasswordSecurity
Please enter a password: e&@!Ksn#6$vq59DV
Password strength: strong
```

#### Example 6 (Rule 4):
```
$ java PasswordSecurity
Please enter a password: StartEarlyStartOften!!
Password strength: medium
Here is a suggested stronger password: Star2tEar2lySt2artO2ften2!!
```

#### Example 7 (Rules 1, 4):
```
$ java PasswordSecurity
Please enter a password: J&*&*&*&*&*&*&*
Password strength: weak
Here is a suggested stronger password: CseJ5&*&*5&*&*5&*&*5&*
```
#### Example 8 (Rules 3, 4, 5):
```
$ java PasswordSecurity
Please enter a password: qwertyuiop
Password strength: very weak
Here is a suggested stronger password: qwer0tYui0op@!
```


## Testing
Starting with this programming assignment, there will be some testers that are initially hidden. The table of test cases will be releasesd along with the Autograder.

## Submission
Submit the following file(s) to Gradescope by **Thursday, Oct 13 @ 11:59PM PDT**.
 - `PasswordSecurity.java`

Even if your code does not pass all the tests, you will still be able to submit your work to receive partial points for the tests that you passed. Make sure your code compiles in order to receive partial credit.
