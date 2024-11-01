---
title: Ternary Operation
description: >-
  We are commonly using if, else, else if, and switch case operations to handle our conditional tasks. In this article, we are going to discuss a quick and efficient operation known as Ternary Operation, used to handle conditional tasks in most programming languages.
author: mashkarharis
date: 2024-11-01 00:00:00 +0000
categories: [Java, Optimizations]
tags: [java, ternary operations, effetive programming, conditional operations, Optimizations]
pin: false
---
## When To Use Ternary Operators

- Ideal for simple conditional statements.
- Avoid using for complex conditions as it can reduce readability.

## Advantages of Ternary Operators

1. **Conciseness**: Reduces the amount of code.
2. **Readability**: Once familiar with structure, it can make the code easier to read.

## Structure

As mentioned, We are commonly using if, else, else if, and switch case operations to handle our conditional tasks. Ternary operation is also one of the conditional operations introduced by many programming languages.

First of all, Let's look at the structure of this operator in java.

```
variable var1 = (condition) ? (value If True) : (value If False)
```

This operator has three main parts. Those are,

```
Variable
Condition
Return Values
```
## Example Usages

When we run this operator, first it checks whether the given **condition** is true. If true, it stores **Value If True** in **var1**. If the given **condition** is false, it stores **Value If False** in **var1**. The java code below shows how this works.

```java
class Ternary{
    public static void main(String args[]) {
        int score;
        String result;
        score=75;
        result = (score>=50) ? ("Passed") : ("Failed");
        System.out.println(result);
        score=30;
        result = (score>=50) ? ("Passed") : ("Failed");
        System.out.println(result);
    }
}
```
Output :
```
Passed
Failed
```

Now you have the question: *“Why do we need this operation? We can use an “If” Statement instead of this”*. Yes, you are correct, we can use an “if” condition instead of using ternary operation. But after reading the below example I’m sure that **you will choose ternary over if**.

**Grading Students Mark Using If, else, else if Condition :**
```java
class Ternary{
    public static void main(String args[]) {
        
        int score;
        String result;
        score=67;

        if(score>=75){
            result = "A";
        }
        else if(score>=65){
            result = "B";
        }
        else if(score>=55){
            result = "C";
        }
        else if(score>=40){
            result = "S";
        }
        else{
            result = "F";
        }
        
        System.out.println(result);
    }
}
```

It took **26** code lines to define the logic !!!

**Grading Students Mark Using Ternary Operation :**
```java
class Ternary{
    public static void main(String args[]) {
        int score;
        String result;
        score=30;
        result = (score>=75)? "A":
                 (score>=65)? "B":
                 (score>=55)? "C":
                 (score>=40)? "S":
                 "W" ;
        System.out.println(result);
    }
}
```
Now it took **13** code lines !!!

Now you can see that **codes can be shrunk** using ternary operation.

## Conclusion

As discussed The ternary operator is a valuable addition to a developer's toolkit. It allows for cleaner and more efficient code, making it a preferred choice in many scenarios.

I hope that this article was helpful to you. Thank you for reading.
