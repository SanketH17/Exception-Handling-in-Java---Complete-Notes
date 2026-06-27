# 🛡️ Exception Handling in Java — Complete Notes

> **Source:** Based on the reference material provided in `reference.txt`.
> **Last Updated:** 11 Feb 2026

---

## 📖 Table of Contents

| # | Topic |
|---|-------|
| 1 | [What is an Exception?](#1--what-is-an-exception-in-java) |
| 2 | [What is Exception Handling?](#2--what-is-exception-handling-in-java) |
| 3 | [Why Do We Need Exception Handling?](#3--why-do-we-need-exception-handling) |
| 4 | [Hierarchy of Java Exception Classes](#4--hierarchy-of-java-exception-classes) |
| 5 | [Types of Exceptions — Checked vs Unchecked](#5--types-of-exceptions--checked-vs-unchecked) |
| 6 | [Error vs Exception](#6--error-vs-exception) |
| 7 | [Mermaid Diagram — Exception Hierarchy](#7--mermaid-diagram--complete-exception-hierarchy) |
| 8 | [Try-Catch Exception Handling](#8--try-catch-exception-handling) |
| 9 | [Control Flow in Try-Catch](#9--control-flow-in-try-catch) |
| 10 | [Multiple Catch Blocks](#10--multiple-catch-blocks) |
| 11 | [The finally Block](#11--the-finally-block) |
| 12 | [Various Possible Combinations of try-catch-finally](#12--various-possible-combinations-of-try-catch-finally) |
| 13 | [Methods to Print Exception Information](#13--methods-to-print-exception-information) |
| 14 | [Best Practices for Exception Handling](#14--best-practices-for-exception-handling) |
| 15 | [Difference Between final, finally and finalize()](#15-difference-between-final-finally-and-finalize-in-java) |
| 16 | [The throw Keyword](#16--the-throw-keyword-in-java) |

---

## 1. 🤔 What is an Exception in Java?

An **exception** is an event that occurs **during the execution** of a program and **disrupts the normal flow** of instructions.

### Real-Life Analogy 🎯

> Imagine you're driving on a highway (your program is running).
> Suddenly, a tyre gets punctured (an exception occurs).
> If you don't have a spare tyre (no exception handling), you're stuck on the road (program crashes).
> But if you carry a spare (exception handling), you change the tyre and continue your journey (normal flow is maintained).

### Common Causes of Exceptions

| Cause | Example |
|-------|---------|
| Invalid user input | Entering text where a number is expected |
| File not found | Trying to read a file that doesn't exist |
| Division by zero | `int result = 10 / 0;` |

### Key Point

When an exception occurs, it is represented by an **object** of a subclass of the `java.lang.Exception` class.

> 💡 **Think of it this way:** Every exception is like an "incident report" — Java creates an object that holds all the details about what went wrong.

---

## 2. 🔧 What is Exception Handling in Java?

**Exception Handling** is a mechanism that allows a program to **handle runtime errors** so that the **normal flow of the application can be maintained**.

### Real-Life Analogy 🎯

> Think of exception handling like a **safety net under a tightrope walker**.
> The walker (your program) might slip (encounter a runtime error), but the safety net (exception handling) catches them and prevents a disaster (program crash).

### Exceptions It Can Handle

| Exception | When It Occurs |
|-----------|----------------|
| `ClassNotFoundException` | When the JVM can't find the class you're trying to use |
| `IOException` | When an input/output operation fails |
| `SQLException` | When a database operation fails |
| `RemoteException` | When a remote method invocation fails |

---

## 3. 🎯 Why Do We Need Exception Handling?

Without exception handling, when a runtime error occurs, the program **abruptly terminates** and all subsequent code is skipped.

### Analogy 🎯

> Imagine a chef following a recipe (program).
> If step 3 says "add sugar" but there's no sugar (error), without exception handling, the chef **throws away the entire dish** and stops cooking.
> With exception handling, the chef says "No sugar? Use honey instead!" and **continues cooking the rest of the dish**.

**In short:** Exception handling ensures your program is **robust and error-free** — it doesn't crash at the first sign of trouble.

---

## 4. 🏗️ Hierarchy of Java Exception Classes

Exceptions in Java are organized in a **hierarchical structure** (like a family tree). All exception classes are part of the `java.lang` package.

### The Family Tree 🌳

```
                        Object
                          │
                      Throwable          ← The "Grandparent" of all errors & exceptions
                      /        \
                 Exception      Error
                /     \            \
         Checked   RuntimeException  (System-level problems)
        Exceptions  (Unchecked)
```

### Real-Life Analogy 🎯

> Think of `Throwable` as the **principal of a school**.
> Under the principal, there are **two vice-principals**:
> - **`Exception`** — Manages student issues that teachers CAN handle (checked + unchecked exceptions).
> - **`Error`** — Manages infrastructure problems like building collapse that teachers CANNOT handle (e.g., `OutOfMemoryError`).

### Understanding Each Level

| Class | Role | Analogy |
|-------|------|---------|
| **`Object`** | Root of all Java classes | The foundation of the school building |
| **`Throwable`** | Superclass of ALL errors and exceptions | The principal — everything reports to them |
| **`Error`** | Serious system errors that apps **usually cannot handle** | A power outage — students can't fix it |
| **`Exception`** | Conditions that a program **might want to catch** | A student forgot their homework — teacher can handle it |

---

## 5. 🔀 Types of Exceptions — Checked vs Unchecked

Java divides exceptions into **two main categories**. Think of it like this:

> 🎯 **Simple Analogy:**
> Imagine you're going on a road trip 🚗
> - **Checked Exception** = Your mom forces you to **carry a first-aid kit** before you leave. You can't leave the house without it. *(Compiler forces you to handle it before running.)*
> - **Unchecked Exception** = You **hit a pothole** on the road that you didn't see coming. Nobody warned you. *(Happens while the program is running.)*

---

### 📋 Checked Exceptions (Compile-Time Exceptions)

**In simple words:** These are problems that Java **already knows might happen**, so it **forces you** to prepare for them **before your code even runs**.

If you DON'T handle them → ❌ **Your code won't compile at all!**

You MUST do one of these:
- ✅ Wrap the risky code in a `try-catch` block, **OR**
- ✅ Add `throws` in the method signature to pass the responsibility

#### 💡 Real-Life Analogy

> Checked exceptions are like a **seatbelt in a car** 🚗
> The car (Java compiler) **won't start** unless you buckle up (handle the exception).
> The accident might never happen — but the car still makes you prepare for it!

#### ✏️ Example 1 — `FileNotFoundException`

You're trying to read a file, but what if the file doesn't exist?

```java
import java.io.File;
import java.io.FileReader;
import java.io.FileNotFoundException;

public class CheckedExample {
    public static void main(String[] args) {

        // ❌ WITHOUT handling — This WON'T compile!
        // FileReader fr = new FileReader("notes.txt");   // Compiler Error!

        // ✅ WITH handling — Now it compiles and runs safely
        try {
            FileReader fr = new FileReader("notes.txt");
            System.out.println("File opened successfully!");
        } catch (FileNotFoundException e) {
            System.out.println("⚠️ Oops! File not found: " + e.getMessage());
        }
    }
}
```

**What happens here?**
1. Java knows that `FileReader` might fail (file may not exist).
2. So the compiler says: *"Hey! Handle this, or I won't compile your code."*
3. We wrap it in `try-catch` → Now if the file is missing, instead of crashing, it prints a friendly message.

#### ✏️ Example 2 — `ClassNotFoundException`

```java
public class CheckedExample2 {
    public static void main(String[] args) {
        try {
            // Trying to load a class that doesn't exist
            Class.forName("com.example.MyClass");
        } catch (ClassNotFoundException e) {
            System.out.println("⚠️ Class not found: " + e.getMessage());
        }
    }
}
```

#### 📌 Common Checked Exceptions at a Glance

| Exception | When Does It Happen? | Simple Example |
|-----------|---------------------|----------------|
| `FileNotFoundException` | File you want to read doesn't exist | Opening `marks.txt` that was deleted |
| `IOException` | Any input/output operation fails | Reading from a broken USB drive |
| `SQLException` | Database query goes wrong | Wrong table name in SQL query |
| `ClassNotFoundException` | Java can't find a class you asked for | Typo in class name |
| `InterruptedException` | A sleeping thread gets disturbed | Waking up a `Thread.sleep()` early |

---

### ⚡ Unchecked Exceptions (Runtime Exceptions)

**In simple words:** These are problems that Java **doesn't warn you about** beforehand. They happen **while your program is running** — usually because of **mistakes YOU made** in the code (bugs!).

The compiler **won't force** you to handle them. But if they happen → 💥 **Program crashes!**

#### 💡 Real-Life Analogy

> Unchecked exceptions are like **stepping on a banana peel** 🍌
> Nobody warned you it was there (compiler doesn't check).
> You slipped because **someone left it on the floor** (bug in your code).
> Solution? **Don't leave banana peels around!** (Write better code!)

#### ✏️ Example 1 — `ArithmeticException` (Division by zero)

```java
public class UncheckedExample1 {
    public static void main(String[] args) {
        int a = 10;
        int b = 0;

        // 💥 This will CRASH at runtime!
        // int result = a / b;   // ArithmeticException: / by zero

        // ✅ Safe way — check before dividing
        if (b != 0) {
            int result = a / b;
            System.out.println("Result: " + result);
        } else {
            System.out.println("⚠️ Cannot divide by zero!");
        }
    }
}
```

**What happens here?**
1. Dividing by 0 is mathematically impossible.
2. Java doesn't warn you at compile time — the code compiles fine!
3. But when it runs → 💥 `ArithmeticException`
4. Fix: Simply check if `b != 0` before dividing.

#### ✏️ Example 2 — `NullPointerException` (The most common bug!)

```java
public class UncheckedExample2 {
    public static void main(String[] args) {
        String name = null;    // name is pointing to NOTHING

        // 💥 This will CRASH! You can't call a method on "nothing"
        // System.out.println(name.length());   // NullPointerException!

        // ✅ Safe way — check for null first
        if (name != null) {
            System.out.println("Length: " + name.length());
        } else {
            System.out.println("⚠️ Name is null, can't find length!");
        }
    }
}
```

> 🧠 **Think of it this way:** `null` means "empty/nothing". Calling `.length()` on `null` is like asking *"How tall is an invisible person?"* — it makes no sense!

#### ✏️ Example 3 — `ArrayIndexOutOfBoundsException`

```java
public class UncheckedExample3 {
    public static void main(String[] args) {
        int[] marks = {85, 90, 78};    // indices: 0, 1, 2

        // 💥 Index 5 doesn't exist! Only 0, 1, 2 are valid
        // System.out.println(marks[5]);   // ArrayIndexOutOfBoundsException!

        // ✅ Safe way — check array length first
        int index = 5;
        if (index >= 0 && index < marks.length) {
            System.out.println("Marks: " + marks[index]);
        } else {
            System.out.println("⚠️ Invalid index! Array has only " + marks.length + " elements.");
        }
    }
}
```

> 🧠 **Think of it this way:** Your array is like a train with 3 coaches (0, 1, 2). Asking for coach number 5 is impossible — it doesn't exist!

#### ✏️ Example 4 — `NumberFormatException`

```java
public class UncheckedExample4 {
    public static void main(String[] args) {
        String value = "Hello";

        // 💥 "Hello" is NOT a number! Can't convert it
        // int num = Integer.parseInt(value);   // NumberFormatException!

        // ✅ Safe way — use try-catch
        try {
            int num = Integer.parseInt(value);
            System.out.println("Number: " + num);
        } catch (NumberFormatException e) {
            System.out.println("⚠️ '" + value + "' is not a valid number!");
        }
    }
}
```

#### 📌 Common Unchecked Exceptions at a Glance

| Exception | When Does It Happen? | Simple Example |
|-----------|---------------------|----------------|
| `ArithmeticException` | Dividing by zero | `10 / 0` |
| `NullPointerException` | Using `null` like a real object | `null.length()` |
| `ArrayIndexOutOfBoundsException` | Wrong array index | `arr[10]` when array has 3 items |
| `StringIndexOutOfBoundsException` | Wrong string character index | `"Hi".charAt(5)` |
| `NumberFormatException` | Converting non-number string | `Integer.parseInt("abc")` |
| `ClassCastException` | Forcing wrong type conversion | Casting `Dog` to `Cat` |
| `IllegalArgumentException` | Passing invalid value to method | Negative age to `setAge(-5)` |

---

### 🆚 Side-by-Side Comparison

| Feature | Checked Exception ✈️ | Unchecked Exception 🍌 |
|---------|----------------------|------------------------|
| **When caught?** | At **compile time** (before running) | At **runtime** (while running) |
| **Must handle?** | ✅ Yes — compiler forces you | ❌ No — compiler stays silent |
| **Caused by?** | External things (file, network, DB) | YOUR mistakes in code (bugs) |
| **Parent class** | `Exception` | `RuntimeException` |
| **If not handled?** | Code **won't compile** ❌ | Code compiles but **crashes** 💥 |
| **How to fix?** | `try-catch` or `throws` | Fix the bug / add safety checks |
| **Analogy** | Seatbelt — car won't start without it 🚗 | Banana peel — you slip unexpectedly 🍌 |
| **Examples** | `IOException`, `SQLException` | `NullPointerException`, `ArithmeticException` |

### 🧠 Super Simple Memory Trick

```
📋 Checked   →  "C" = Compiler Checks it    →  Handle it OR it won't compile!
⚡ Unchecked →  "U" = YOU caused it (bug)   →  Fix your code!
```

---

## 6. 🚨 Error vs Exception

Both `Error` and `Exception` are children of `Throwable` — but they are **very different** in nature.

> 🧠 **One-line difference:**
> - **Error** = Something went wrong with the **JVM/system** — you usually **cannot fix it**.
> - **Exception** = Something went wrong in your **code/logic** — you usually **can fix it**.

---

### 💥 What is an Error?

An **Error** is a **serious problem** at the system/JVM level that your program **cannot recover from**.

> Think of it like a **power outage in a hospital** — the hospital (your app) just can't run. You can't "catch" a power cut and keep going.

#### Common Errors with Examples

**1. `StackOverflowError`** — happens when a method calls itself infinitely (infinite recursion)

```java
public class Demo {
    static void infiniteMethod() {
        infiniteMethod(); // calls itself forever!
    }

    public static void main(String[] args) {
        infiniteMethod(); // 💥 StackOverflowError
    }
}
```

**2. `OutOfMemoryError`** — happens when the JVM runs out of heap memory

```java
public class Demo {
    public static void main(String[] args) {
        int[] arr = new int[Integer.MAX_VALUE]; // 💥 OutOfMemoryError
    }
}
```

> ❌ These errors **cannot be recovered** from — even if you try to `catch` them, the JVM is already in a broken state.

---

### ✅ What is an Exception?

An **Exception** is a **problem in your application code** that you **can handle** and recover from gracefully.

> Think of it like a **flat tyre** — annoying, but you have a spare tyre (exception handling). Fix it and continue the journey!

#### Common Exceptions with Examples

**1. `ArithmeticException`** — dividing a number by zero

```java
public class Demo {
    public static void main(String[] args) {
        int result = 10 / 0; // 💥 ArithmeticException: / by zero
    }
}
```
✅ **Fixed with try-catch:**
```java
try {
    int result = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Cannot divide by zero! " + e.getMessage());
}
// Program continues normally ✅
```

**2. `NullPointerException`** — calling a method on a `null` object

```java
public class Demo {
    public static void main(String[] args) {
        String name = null;
        System.out.println(name.length()); // 💥 NullPointerException
    }
}
```
✅ **Fixed:**
```java
if (name != null) {
    System.out.println(name.length());
} else {
    System.out.println("Name is null!");
}
```

**3. `ArrayIndexOutOfBoundsException`** — accessing an index that doesn't exist

```java
int[] nums = {10, 20, 30};
System.out.println(nums[5]); // 💥 ArrayIndexOutOfBoundsException
```
✅ **Fixed:**
```java
try {
    System.out.println(nums[5]);
} catch (ArrayIndexOutOfBoundsException e) {
    System.out.println("Index does not exist!");
}
```

---

### 🆚 Error vs Exception — Side-by-Side

| Feature | ❌ Error | ✅ Exception |
|---------|----------|-------------|
| **Who causes it?** | JVM / System | Your code / logic |
| **Can you handle it?** | Usually **NO** | Usually **YES** |
| **Should you catch it?** | ❌ Almost never | ✅ Yes, with try-catch |
| **Program recovery?** | Usually impossible | Yes, program can continue |
| **Examples** | `StackOverflowError`, `OutOfMemoryError` | `NullPointerException`, `IOException` |
| **Analogy** | Power outage 🔌 (can't fix) | Flat tyre 🚗 (can fix with spare) |

---

### 📌 Quick Memory Tip

```
Throwable
├── Error        → JVM is sick 🤒 — you can't cure it
└── Exception    → Your code has a bug 🐛 — you CAN fix it
```

---

## 7. Complete Exception Hierarchy
![Exception Hierarchy](imgs/img1.png)

---

## 8. 🧰 Try-Catch Exception Handling

Now that you know **what** exceptions are, let's learn **how** to handle them using `try-catch`.

### What is try-catch?

The `try-catch` block is Java's way of saying:
> "Hey, this code **might fail**. If it does, **don't crash** — do this instead."

- **`try` block** — contains the "risky" code that might throw an exception.
- **`catch` block** — contains the "backup plan" that runs **only if** an exception occurs.

### Real-Life Analogy 🎯

> Imagine you're cooking 🍳 and a recipe says "crack an egg."
> The **try** block = cracking the egg.
> The **catch** block = if the egg is rotten, throw it away and grab a new one — don't ruin the whole dish!

### Basic Syntax

```java
try {
    // risky code that might throw an exception
} catch (ExceptionType e) {
    // what to do if the exception occurs
}
```

### ✏️ Example

```java
public class Main {
    public static void main(String[] args) {
        try {
            int result = 10 / 0;            // 💥 This will throw ArithmeticException
            System.out.println(result);      // ❌ This line will NOT run
        } catch (ArithmeticException e) {
            System.out.println("Cannot divide by zero!");  // ✅ This runs instead
        }

        System.out.println("Program continues...");       // ✅ Program keeps going!
    }
}
```

**Output:**

```text
Cannot divide by zero!
Program continues...
```

> 💡 **Key takeaway:** Without `try-catch`, the program would crash at `10 / 0`. With `try-catch`, the error is handled gracefully and the program continues running.

---

## 9. 🔄 Control Flow in Try-Catch

Understanding **what runs and what gets skipped** is essential. There are three possible cases:

### Case 1: No Exception Occurs ✅

If nothing goes wrong, the `catch` block is simply **skipped**.

```java
public class Main {
    public static void main(String[] args) {
        try {
            System.out.println("Inside try block");
            int result = 10 / 2;                         // ✅ No exception
            System.out.println("Result: " + result);     // ✅ Runs normally
        } catch (ArithmeticException e) {
            System.out.println("Exception handled");     // ❌ Skipped
        }

        System.out.println("After try-catch");           // ✅ Runs
    }
}
```

**Output:**

```text
Inside try block
Result: 5
After try-catch
```

**Flow:**

```text
try block starts → No exception → catch block SKIPPED → program continues
```

---

### Case 2: Exception Occurs and Is Caught ✅

If an exception occurs, Java **immediately jumps** to the matching `catch` block. Any code after the exception line inside the `try` block is **skipped**.

```java
public class Main {
    public static void main(String[] args) {
        try {
            System.out.println("Line 1");         // ✅ Runs
            int result = 10 / 0;                   // 💥 Exception here!
            System.out.println("Line 2");          // ❌ Skipped
        } catch (ArithmeticException e) {
            System.out.println("Exception caught"); // ✅ Runs
        }

        System.out.println("After try-catch");      // ✅ Runs
    }
}
```

**Output:**

```text
Line 1
Exception caught
After try-catch
```

Once an exception occurs inside the try block, Java immediately jumps to the matching catch block and skips the remaining code inside the try block.

**Flow:**

```text
try block starts → Line 1 runs → Exception at 10/0 → remaining try SKIPPED
                 → matching catch runs → program continues
```

---

### Case 3: Exception Occurs but Is NOT Caught ❌

If the exception type **doesn't match** the `catch` block, the program **terminates**.

```java
public class Main {
    public static void main(String[] args) {
        try {
            int[] numbers = {1, 2, 3};
            System.out.println(numbers[5]);        // 💥 ArrayIndexOutOfBoundsException
        } catch (ArithmeticException e) {          // ❌ Wrong type! Doesn't match
            System.out.println("Arithmetic exception caught");
        }

        System.out.println("After try-catch");     // ❌ Never reaches here
    }
}
```

**Output:**

```text
Exception in thread "main" java.lang.ArrayIndexOutOfBoundsException
```

> 🧠 **Analogy:** You prepared an umbrella (ArithmeticException catch), but it started snowing (ArrayIndexOutOfBoundsException). Your umbrella doesn't help — you still get stuck!

**Flow:**

```text
try block starts → ArrayIndexOutOfBoundsException occurs
                 → No matching catch found → ❌ Program terminates
```

---

## 10. 🎯 Multiple Catch Blocks

You can have **more than one `catch` block** to handle different types of exceptions.

### ✏️ Example

```java
public class Main {
    public static void main(String[] args) {
        try {
            int[] numbers = {1, 2, 3};
            System.out.println(numbers[5]);
        } catch (ArithmeticException e) {
            System.out.println("Math problem");
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Invalid array index");
        } catch (Exception e) {
            System.out.println("Some other exception");
        }

        System.out.println("Program continues...");
    }
}
```

**Output:**

```text
Invalid array index
Program continues...
```

### ⚠️ Important Rule: Order Matters!

Always place **specific exceptions first** and **general exceptions last**.

✅ **Correct:**

```java
catch (ArithmeticException e) { }    // Specific — comes FIRST
catch (Exception e) { }              // General — comes LAST
```

❌ **Wrong:**

```java
catch (Exception e) { }              // General catches EVERYTHING
catch (ArithmeticException e) { }    // ❌ Unreachable! Compiler error
```

Because Exception is the parent class of many exceptions. If it comes first, it can catch almost everything, making the later specific catch blocks unreachable.

---

## 11. 🧹 The `finally` Block

The `finally` block is code that **always runs** — whether an exception occurred or not.

### Why Do We Need It?

It is used for **cleanup tasks** — things that MUST happen no matter what:

- 📁 Closing files
- 🗄️ Closing database connections
- 🌐 Closing network resources
- 🧹 Releasing memory

### Real-Life Analogy 🎯

> Think of `finally` like **washing your hands after cooking** 🧼
> Whether your dish turned out great (no exception) or you burnt it (exception occurred), you still wash your hands!

### Syntax

```java
try {
    // risky code
} catch (ExceptionType e) {
    // handling code
} finally {
    // cleanup code — ALWAYS runs
}
```

### ✏️ Example

```java
public class Main {
    public static void main(String[] args) {
        try {
            System.out.println("Inside try");
            int result = 10 / 0;              // 💥 Exception!
        } catch (ArithmeticException e) {
            System.out.println("Exception handled");
        } finally {
            System.out.println("Finally block executed");  // ✅ Always runs
        }

        System.out.println("After try-catch-finally");
    }
}
```

**Output:**

```text
Inside try
Exception handled
Finally block executed
After try-catch-finally
```

### Control Flow with `finally`

| Scenario | try | catch | finally | After try-catch |
|----------|-----|-------|---------|-----------------|
| No exception | ✅ Runs | ❌ Skipped | ✅ Runs | ✅ Runs |
| Exception caught | ✅ Partial | ✅ Runs | ✅ Runs | ✅ Runs |
| Exception NOT caught | ✅ Partial | ❌ Skipped | ✅ Runs | ❌ Program terminates |

> 💡 **Key point:** `finally` **always** executes — even if the exception is NOT caught and the program is about to terminate. It gets one last chance to clean up.

### Complete Control Flow Diagram

```text
Start
  |
  v
try block
  |
  |-- No exception ─────────────────────|
  |                                      |
  |-- Exception occurs                   |
  |     |                                |
  |     v                                |
  | Is matching catch available?         |
  |     |         |                      |
  |    Yes        No                     |
  |     |         |                      |
  |     v         |                      |
  | catch block   |                      |
  |     |         |                      |
  |     v         v                      v
  |   finally   finally              finally
  |     |         |                      |
  |     v         v                      v
  | Continue   Program             Continue
  | program    terminates          program
```

---

## 12. 🔀 Various Possible Combinations of try-catch-finally

Java has strict rules about how you can combine `try`, `catch`, and `finally`. Not every combination is allowed!

Let's go through **every possible combination** — what works ✅ and what doesn't ❌.

### Real-Life Analogy 🎯

> Think of it like cooking:
> - **`try`** = Attempting the recipe 🍳
> - **`catch`** = Your backup plan if something goes wrong 🧯
> - **`finally`** = Cleaning the kitchen — you ALWAYS do this 🧹
>
> You can't just have a backup plan without attempting something first! And you can't attempt something without either a backup plan or a cleanup step.

---

### Combo 1: `try` + `catch` ✅ (Most Common)

The classic combination — try something risky, and catch the error if it happens.

```java
public class Demo {
    public static void main(String[] args) {
        try {
            int result = 10 / 0;
        } catch (ArithmeticException e) {
            System.out.println("Error: " + e.getMessage());
        }
        System.out.println("Program continues!");
    }
}
```

**Output:**

```text
Error: / by zero
Program continues!
```

> ✅ **Verdict:** Perfectly valid. This is the combination you'll use most often.

---

### Combo 2: `try` + `finally` (without `catch`) ✅

You can skip `catch` entirely! This is useful when you don't want to handle the exception yourself but still need to **clean up resources**.

```java
public class Demo {
    public static void main(String[] args) {
        try {
            System.out.println("Trying something...");
            int result = 10 / 0;   // 💥 Exception!
        } finally {
            System.out.println("Cleanup done!");  // ✅ Still runs!
        }
    }
}
```

**Output:**

```text
Trying something...
Cleanup done!
Exception in thread "main" java.lang.ArithmeticException: / by zero
```

> ✅ **Verdict:** Valid! The `finally` block runs, then the exception propagates (crashes the program since nobody caught it).

> 💡 **When to use:** When a method wants to let the exception pass to the caller but still needs to release resources (close files, connections, etc.).

---

### Combo 3: `try` + `catch` + `finally` ✅

The **full package** — handle the error AND do cleanup.

```java
public class Demo {
    public static void main(String[] args) {
        try {
            int result = 10 / 0;
        } catch (ArithmeticException e) {
            System.out.println("Caught: " + e.getMessage());
        } finally {
            System.out.println("Finally always runs!");
        }
        System.out.println("Program continues!");
    }
}
```

**Output:**

```text
Caught: / by zero
Finally always runs!
Program continues!
```

> ✅ **Verdict:** The most complete and safest combination.

---

### Combo 4: `try` alone (without `catch` or `finally`) ❌

A `try` block **must** be followed by at least one `catch` or a `finally`. You can't have `try` standing alone.

```java
// ❌ COMPILE ERROR!
public class Demo {
    public static void main(String[] args) {
        try {
            int result = 10 / 2;
        }
        // No catch, no finally — ERROR!
    }
}
```

**Error:**

```text
error: 'try' without 'catch', 'finally' or resource declarations
```

> ❌ **Verdict:** Not allowed! `try` needs a buddy — either `catch` or `finally` (or both).

> 🧠 **Think of it this way:** `try` is like saying "I'm going to attempt something risky." But if you don't have a backup plan (`catch`) or a cleanup crew (`finally`), what's the point of saying it?

---

### Combo 5: `catch` alone (without `try`) ❌

`catch` can **never** exist without `try`. It only makes sense as a response to a `try` block.

```java
// ❌ COMPILE ERROR!
public class Demo {
    public static void main(String[] args) {
        catch (Exception e) {
            System.out.println("Error!");
        }
    }
}
```

**Error:**

```text
error: 'catch' without 'try'
```

> ❌ **Verdict:** Makes no sense! You can't catch something if you never tried anything.

> 🧠 **Analogy:** It's like having a safety net 🥅 with no trapeze artist. Who are you trying to catch?

---

### Combo 6: `finally` alone (without `try`) ❌

`finally` can **never** exist without `try`.

```java
// ❌ COMPILE ERROR!
public class Demo {
    public static void main(String[] args) {
        finally {
            System.out.println("Cleanup!");
        }
    }
}
```

**Error:**

```text
error: 'finally' without 'try'
```

> ❌ **Verdict:** `finally` is always attached to a `try`. It can't stand on its own.

---

### Combo 7: `try` + Multiple `catch` blocks ✅

You can have **many** `catch` blocks to handle different exception types.

```java
public class Demo {
    public static void main(String[] args) {
        try {
            int[] arr = {1, 2};
            System.out.println(arr[5]);
        } catch (ArithmeticException e) {
            System.out.println("Math error!");
        } catch (ArrayIndexOutOfBoundsException e) {
            System.out.println("Array index error!");
        } catch (Exception e) {
            System.out.println("Some other error!");
        }
    }
}
```

**Output:**

```text
Array index error!
```

> ✅ **Verdict:** Valid! Remember — **specific exceptions first, general exceptions last.**

---

### Combo 8: `try` + Multiple `catch` + `finally` ✅

The ultimate combo — handle multiple exception types AND always clean up.

```java
public class Demo {
    public static void main(String[] args) {
        try {
            String text = null;
            System.out.println(text.length());
        } catch (ArithmeticException e) {
            System.out.println("Math error!");
        } catch (NullPointerException e) {
            System.out.println("Null error: can't use null!");
        } finally {
            System.out.println("Cleanup done!");
        }
    }
}
```

**Output:**

```text
Null error: can't use null!
Cleanup done!
```

> ✅ **Verdict:** Perfectly valid. This is common in real-world applications.

---

### Combo 9: Nested `try-catch` (try inside try) ✅

You can put a `try-catch` **inside** another `try-catch`. This is useful when different parts of your code might throw different exceptions.

```java
public class Demo {
    public static void main(String[] args) {
        try {
            System.out.println("Outer try");

            try {
                int result = 10 / 0;   // 💥 Inner exception
            } catch (ArithmeticException e) {
                System.out.println("Inner catch: " + e.getMessage());
            }

            System.out.println("Back in outer try");
        } catch (Exception e) {
            System.out.println("Outer catch");
        }
    }
}
```

**Output:**

```text
Outer try
Inner catch: / by zero
Back in outer try
```

> ✅ **Verdict:** Valid! The inner `catch` handles the inner exception, so the outer `try` continues normally.

> 💡 **Tip:** Don't nest too deeply — it makes code hard to read. If you find yourself nesting 3+ levels, consider breaking the code into separate methods.

---

### 📋 Quick Reference Table — All Combinations

| # | Combination | Valid? | Notes |
|---|------------|:------:|-------|
| 1 | `try` + `catch` | ✅ | Most common pattern |
| 2 | `try` + `finally` | ✅ | Cleanup without handling |
| 3 | `try` + `catch` + `finally` | ✅ | Full package — handle + cleanup |
| 4 | `try` alone | ❌ | Must have `catch` or `finally` |
| 5 | `catch` alone | ❌ | `catch` needs a `try` |
| 6 | `finally` alone | ❌ | `finally` needs a `try` |
| 7 | `try` + multiple `catch` | ✅ | Specific first, general last |
| 8 | `try` + multiple `catch` + `finally` | ✅ | Most complete pattern |
| 9 | Nested `try-catch` | ✅ | Don't nest too deep |

### 🧠 Simple Memory Rule

```text
✅ try MUST have → at least one catch OR finally (or both)
❌ catch ALONE   → NOT allowed (needs try)
❌ finally ALONE → NOT allowed (needs try)
❌ try ALONE     → NOT allowed (needs catch or finally)
```

> 💡 **One-liner to remember:** `try` never walks alone — it always needs a `catch` or a `finally` by its side!

---

## 13. 🖨️ Methods to Print Exception Information

When an exception occurs, you'll want to know: **What went wrong? Where? Why?**

Java gives you several methods to get this information. All of these are available in the `Throwable` class (the grandparent of all exceptions).

### Real-Life Analogy 🎯

> Imagine a car accident 🚗💥
> - `getMessage()` → "The tyre burst" (just the short description)
> - `toString()` → "TyreException: The tyre burst" (description + category)
> - `printStackTrace()` → Full accident report — what happened, where, which road, what time (complete details)

---

### 📋 Common Example Used Below

We'll use this same code for all examples so you can easily compare the outputs:

```java
public class Main {
    public static void main(String[] args) {
        try {
            int result = 10 / 0;                  // 💥 ArithmeticException
            System.out.println(result);
        } catch (ArithmeticException e) {
            // We'll try different methods here
        }
    }
}
```

---

### Method 1: `getMessage()` — Just the Error Message

Returns **only** the short description of what went wrong. No class name, no line number.

```java
catch (ArithmeticException e) {
    System.out.println(e.getMessage());
}
```

**Output:**

```text
/ by zero
```

> 💡 **When to use:** When you want a **clean, user-friendly message** — great for showing errors to end users.

---

### Method 2: `toString()` — Class Name + Message

Returns the **exception class name** followed by the **message**.

```java
catch (ArithmeticException e) {
    System.out.println(e.toString());
}
```

**Output:**

```text
java.lang.ArithmeticException: / by zero
```

> 💡 **Fun fact:** If you directly print the exception object like `System.out.println(e);`, Java automatically calls `e.toString()` behind the scenes — so the output is the same!

---

### Method 3: `printStackTrace()` — The Full Report

Prints the **complete** exception details:

- ✅ Exception class name
- ✅ Error message
- ✅ File name
- ✅ Method name
- ✅ Line number
- ✅ Call chain (which methods called which)

```java
catch (ArithmeticException e) {
    e.printStackTrace();
}
```

**Output:**

```text
java.lang.ArithmeticException: / by zero
    at Main.main(Main.java:4)
```

**Reading the output:**

```text
java.lang.ArithmeticException   ← Exception class
/ by zero                       ← Error message
at Main.main(Main.java:4)       ← Class: Main, Method: main, File: Main.java, Line: 4
```

> 💡 **When to use:** Best for **debugging and learning** — it tells you exactly where the problem is.

> ⚠️ **Note:** In real projects, use a logging framework (like Log4j or SLF4J) instead of `printStackTrace()`.

---

### Method 4: Printing the Exception Object Directly

You can simply put the exception object inside `System.out.println()`.

```java
catch (ArithmeticException e) {
    System.out.println(e);
}
```

**Output:**

```text
java.lang.ArithmeticException: / by zero
```

This gives the **same output** as `e.toString()` because Java internally calls `toString()` when you print an object.

---

### Method 5: `getStackTrace()` — Stack Trace as an Array

Returns stack trace details as an **array** of `StackTraceElement` objects, so you can process each element individually.

```java
catch (ArithmeticException e) {
    StackTraceElement[] stackTrace = e.getStackTrace();

    for (StackTraceElement element : stackTrace) {
        System.out.println(element);
    }
}
```

**Output:**

```text
Main.main(Main.java:4)
```

You can also extract individual details:

```java
StackTraceElement element = e.getStackTrace()[0];

System.out.println("Class Name:  " + element.getClassName());     // Main
System.out.println("Method Name: " + element.getMethodName());    // main
System.out.println("File Name:   " + element.getFileName());      // Main.java
System.out.println("Line Number: " + element.getLineNumber());    // 4
```

> 💡 **When to use:** When you want to **custom-format** the stack trace or extract specific details programmatically.

---

### Method 6: `printStackTrace(PrintStream)` — Redirect Output

By default, `printStackTrace()` prints to the **error stream** (`System.err`). You can redirect it to `System.out` or any other `PrintStream`.

```java
catch (ArithmeticException e) {
    e.printStackTrace(System.out);   // Prints to standard output instead of error stream
}
```

---

### Method 7: `printStackTrace(PrintWriter)` — Write to a File or String

This lets you capture the stack trace as a `String` — useful for logging, APIs, or saving to files.

```java
import java.io.PrintWriter;
import java.io.StringWriter;

catch (ArithmeticException e) {
    StringWriter stringWriter = new StringWriter();
    PrintWriter printWriter = new PrintWriter(stringWriter);

    e.printStackTrace(printWriter);

    String exceptionAsString = stringWriter.toString();
    System.out.println(exceptionAsString);
}
```

> 💡 **When to use:** When you need the stack trace as a `String` — for example, to store in a database, send in an API response, or write to a custom log file.

---

### 🆚 Comparison Table — All Methods at a Glance

| Method | What It Prints | Includes Class Name? | Includes Line Number? | Best Use |
|--------|---------------|:--------------------:|:---------------------:|----------|
| `e.getMessage()` | Only the error message | ❌ | ❌ | User-friendly messages |
| `e.toString()` | Class name + message | ✅ | ❌ | Basic technical info |
| `System.out.println(e)` | Class name + message | ✅ | ❌ | Quick printing |
| `e.printStackTrace()` | Full stack trace | ✅ | ✅ | Debugging & learning |
| `e.getStackTrace()` | Stack trace as array | ✅ | ✅ | Custom formatting |
| `e.printStackTrace(PrintStream)` | Full trace to stream | ✅ | ✅ | Redirecting output |
| `e.printStackTrace(PrintWriter)` | Full trace to writer | ✅ | ✅ | Converting to String/file |

---

### ✏️ Practical Example — All Methods Together

```java
public class Main {
    public static void main(String[] args) {
        try {
            String name = null;
            System.out.println(name.length());    // 💥 NullPointerException
        } catch (NullPointerException e) {

            System.out.println("--- getMessage() ---");
            System.out.println(e.getMessage());

            System.out.println("\n--- toString() ---");
            System.out.println(e.toString());

            System.out.println("\n--- Printing object directly ---");
            System.out.println(e);

            System.out.println("\n--- printStackTrace() ---");
            e.printStackTrace();
        }
    }
}
```

**Sample Output:**

```text
--- getMessage() ---
Cannot invoke "String.length()" because "name" is null

--- toString() ---
java.lang.NullPointerException: Cannot invoke "String.length()" because "name" is null

--- Printing object directly ---
java.lang.NullPointerException: Cannot invoke "String.length()" because "name" is null

--- printStackTrace() ---
java.lang.NullPointerException: Cannot invoke "String.length()" because "name" is null
    at Main.main(Main.java:5)
```

### 🧠 Quick Memory Trick

```text
getMessage()      →  📝 Just the note        →  "/ by zero"
toString()        →  📋 Note + label         →  "ArithmeticException: / by zero"
printStackTrace() →  📑 Full incident report →  class + message + file + line number
```

---

## 14. ✅ Best Practices for Exception Handling

### 1. Never Leave `catch` Blocks Empty

Empty catch blocks **hide errors** — you'll never know something went wrong!

❌ **Bad:**

```java
try {
    int result = 10 / 0;
} catch (Exception e) {
    // ignored — DANGEROUS!
}
```

✅ **Good:**

```java
try {
    int result = 10 / 0;
} catch (Exception e) {
    System.out.println("Error occurred: " + e.getMessage());
}
```

---

### 2. Catch Specific Exceptions First

Always go from **most specific** → **most general**.

```java
// ✅ Correct order
catch (FileNotFoundException e) { }
catch (IOException e) { }
catch (Exception e) { }

// ❌ Wrong — Exception catches everything, making the rest unreachable
catch (Exception e) { }
catch (FileNotFoundException e) { }    // Compiler error!
```

---

### 3. Show User-Friendly Messages to End Users

Don't scare your users with technical stack traces!

❌ **Bad for users:**

```text
java.lang.ArithmeticException: / by zero
    at Main.main(Main.java:4)
```

✅ **Good for users:**

```text
Something went wrong. Please try again.
```

> 💡 **Tip:** Log the full exception details **internally** for developers, but show a simple message **externally** to users.

---

## 15. Difference Between final, finally and finalize() in Java

### Java: `final` vs `finally` vs `finalize()`

A classic interview topic — three keywords/constructs that sound alike but serve completely different purposes in Java.

---

## Quick Comparison

| Aspect | `final` | `finally` | `finalize()` |
|---|---|---|---|
| **What it is** | Keyword / modifier | Block | Method |
| **Used with** | Variables, methods, classes | `try-catch` statement | `Object` class (overridden) |
| **Purpose** | Restrict modification / inheritance | Guarantee cleanup code runs | Cleanup before garbage collection |
| **When it runs** | N/A (compile-time restriction) | Always, after try-catch | Uncertain — GC decides |
| **Status** | Actively used | Actively used | Deprecated since Java 9 |

---

## 1. `final` — Keyword

Applies restrictions on variables, methods, or classes. There are **three uses**:

### a) `final` variable — value can't be reassigned
```java
final int x = 10;
x = 20; // ❌ Compile error
```

### b) `final` method — can't be overridden by a subclass
```java
class Vehicle {
    final void start() {
        System.out.println("Starting...");
    }
}

class Car extends Vehicle {
    void start() { } // ❌ Compile error — cannot override
}
```

### c) `final` class — can't be extended/inherited
```java
final class Vehicle { }

class Car extends Vehicle { } // ❌ Compile error
```

> **Note:** A `final` reference variable can't be reassigned, but if it refers to a mutable object, the object's internal state can still change.
```java
final List<String> names = new ArrayList<>();
names.add("Sanket"); // ✅ Allowed — modifying object, not reassigning reference
names = new ArrayList<>(); // ❌ Compile error
```

---

## 2. `finally` — Block

A block that **always executes** after a `try-catch`, regardless of whether an exception occurred — typically used for cleanup (closing files, DB connections, sockets, etc.).

```java
try {
    int result = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Caught exception");
} finally {
    System.out.println("This always runs");
}
```

**Output:**
```
Caught exception
This always runs
```

### Key behaviors
- Executes even if there's a `return` statement inside `try` or `catch`.
- Executes even if an exception is **not** caught (propagates after `finally` runs).
- The **only** way to skip `finally` is calling `System.exit()` or a JVM crash.

```java
public int test() {
    try {
        return 1;
    } finally {
        System.out.println("finally runs before return completes");
    }
}
```

---

## 3. `finalize()` — Method

A method inherited from the `Object` class, historically called by the **garbage collector** just before an object is destroyed — meant for last-chance resource cleanup.

```java
@Override
protected void finalize() throws Throwable {
    System.out.println("Object is being garbage collected");
}
```

### Why it's avoided in modern Java
- **Deprecated since Java 9**, marked for removal in future versions.
- Execution timing is **unpredictable** — depends entirely on the GC, and it may **never run** at all.
- Can cause performance issues and resource leaks if relied upon.

### Modern replacement: try-with-resources + `AutoCloseable`
```java
try (FileInputStream fis = new FileInputStream("file.txt")) {
    // use resource
} // automatically closed here, deterministically
```

Or use `java.lang.ref.Cleaner` (introduced in Java 9) for cases needing GC-triggered cleanup without `finalize()`.

---

## Interview One-Liner

> `final` is a keyword used to restrict reassignment, overriding, or inheritance. `finally` is a block that always executes after try-catch, typically for cleanup. `finalize()` is a deprecated `Object` method once called by the garbage collector before object destruction — replaced today by try-with-resources and `AutoCloseable`.

---

## Common Follow-up Questions

**Q: Can a `finally` block contain a `return` statement?**
Yes, but it's bad practice — a `return` in `finally` overrides any `return`/exception from `try`/`catch`, silently swallowing them.

**Q: Is `final` the same as immutability?**
No. `final` only prevents reassignment of the reference. For true immutability, the object itself must be designed to not expose mutator methods (like `String` or records).

**Q: Why was `finalize()` deprecated?**
Because of non-deterministic execution, potential to delay GC, and risk of resource leaks — replaced by `try-with-resources`, `AutoCloseable`, and `Cleaner`.

---

## 16. 🚀 The `throw` Keyword in Java

So far, we've seen Java **automatically** throwing exceptions when something goes wrong (like dividing by zero). But what if **you** want to throw an exception **yourself**?

That's exactly what the `throw` keyword does!

### What is `throw`?

The `throw` keyword lets you **manually create and throw an exception** from your code.

> 💡 **Simple definition:** `throw` = "Hey Java, I found a problem — here, deal with this exception!"

### Real-Life Analogy 🎯

> Imagine you're a teacher checking exam papers 📝
> - Java automatically catches cheating (built-in exceptions like `ArithmeticException`).
> - But what if a student submits a **blank paper**? Java doesn't know that's wrong — **you** have to raise the flag yourself!
> - `throw` = You (the teacher) **manually raising a red flag** 🚩 to say "This is not acceptable!"

---

### Basic Syntax

```java
throw new ExceptionType("Your error message here");
```

**Key points:**
- `throw` is followed by an **exception object** (using `new`).
- You provide a **message** that explains what went wrong.
- After `throw` executes, the remaining code in that block is **skipped** — just like when Java throws an exception automatically.

---

### ✏️ Example 1 — Throwing a Built-in Exception

Let's say you're writing a method that accepts a person's age. A negative age makes no sense — so you **throw** an exception yourself.

```java
public class Main {
    public static void checkAge(int age) {
        if (age < 0) {
            throw new IllegalArgumentException("Age cannot be negative: " + age);
        }
        System.out.println("Age is valid: " + age);
    }

    public static void main(String[] args) {
        checkAge(25);    // ✅ Works fine
        checkAge(-5);    // 💥 Throws exception!
    }
}
```

**Output:**

```text
Age is valid: 25
Exception in thread "main" java.lang.IllegalArgumentException: Age cannot be negative: -5
```

> 🧠 **What happened?**
> 1. `checkAge(25)` — age is valid, prints normally.
> 2. `checkAge(-5)` — age is negative, so **we** throw an `IllegalArgumentException`.
> 3. Since nobody caught it, the program crashes with our custom message.

---

### ✏️ Example 2 — Throwing and Catching with try-catch

In the previous example, the program crashed. Let's **catch** the thrown exception instead:

```java
public class Main {
    public static void checkAge(int age) {
        if (age < 0) {
            throw new IllegalArgumentException("Age cannot be negative: " + age);
        }
        System.out.println("Age is valid: " + age);
    }

    public static void main(String[] args) {
        try {
            checkAge(-5);
        } catch (IllegalArgumentException e) {
            System.out.println("⚠️ Error caught: " + e.getMessage());
        }

        System.out.println("Program continues normally!");
    }
}
```

**Output:**

```text
⚠️ Error caught: Age cannot be negative: -5
Program continues normally!
```

> ✅ Now the exception is caught, and the program **doesn't crash**!

---

### ✏️ Example 3 — Throwing Different Built-in Exceptions

You can throw **any** exception class that Java provides:

```java
public class Main {
    public static void withdraw(double balance, double amount) {
        if (amount <= 0) {
            throw new IllegalArgumentException("Amount must be positive!");
        }
        if (amount > balance) {
            throw new ArithmeticException("Insufficient balance!");
        }
        System.out.println("Withdrawn: ₹" + amount);
    }

    public static void main(String[] args) {
        try {
            withdraw(1000, 500);     // ✅ Works
            withdraw(1000, 2000);    // 💥 Throws ArithmeticException
        } catch (IllegalArgumentException e) {
            System.out.println("⚠️ Invalid input: " + e.getMessage());
        } catch (ArithmeticException e) {
            System.out.println("⚠️ Transaction failed: " + e.getMessage());
        }
    }
}
```

**Output:**

```text
Withdrawn: ₹500.0
⚠️ Transaction failed: Insufficient balance!
```

---

### 🛠️ Custom Exceptions — Why Do We Need Them?

Java's built-in exceptions (`ArithmeticException`, `NullPointerException`, etc.) are general-purpose. But sometimes you need **your own exception** that describes a **specific problem** in your application.

> 🎯 **Analogy:** Java's built-in exceptions are like **generic medicine** 💊 — they work for common problems.
> Custom exceptions are like **prescription medicine** 📋 — made specifically for YOUR problem!

There are **two types** of custom exceptions you can create:

| Type | Extends | Must Handle? | When to Use |
|------|---------|:------------:|-------------|
| **Custom Unchecked** | `RuntimeException` | ❌ No (optional) | Programming errors, validation failures |
| **Custom Checked** | `Exception` | ✅ Yes (forced) | External failures (file, network, DB) |

---

### 📦 Custom Unchecked Exception (extends `RuntimeException`)

Unchecked exceptions are for problems caused by **mistakes in the code** — like passing invalid values. The compiler **won't force** you to handle them.

#### Step 1: Create the Custom Exception Class

```java
// This is our custom exception — it extends RuntimeException (unchecked)
public class InvalidAgeException extends RuntimeException {
    public InvalidAgeException(String message) {
        super(message);    // Pass the message to the parent class
    }
}
```

> 💡 `super(message)` sends the error message up to `RuntimeException`, which stores it. Later you can retrieve it using `getMessage()`.

#### Step 2: Throw It in Your Code

```java
public class AgeValidator {
    public static void validate(int age) {
        if (age < 0 || age > 150) {
            throw new InvalidAgeException("Invalid age: " + age + ". Age must be between 0 and 150.");
        }
        System.out.println("✅ Age " + age + " is valid.");
    }

    public static void main(String[] args) {
        validate(25);     // ✅ Valid
        validate(200);    // 💥 Throws InvalidAgeException
    }
}
```

**Output:**

```text
✅ Age 25 is valid.
Exception in thread "main" InvalidAgeException: Invalid age: 200. Age must be between 0 and 150.
```

#### Step 3: Handle It with try-catch (Optional but Recommended)

Since it's unchecked, the compiler **won't force** you — but it's good practice to handle it:

```java
public static void main(String[] args) {
    try {
        validate(200);
    } catch (InvalidAgeException e) {
        System.out.println("⚠️ Caught: " + e.getMessage());
    }

    System.out.println("Program continues!");
}
```

**Output:**

```text
⚠️ Caught: Invalid age: 200. Age must be between 0 and 150.
Program continues!
```

---

### 📦 Custom Checked Exception (extends `Exception`)

Checked exceptions are for problems caused by **external factors** — things outside your code's control (missing files, failed network calls, etc.). The compiler **forces** you to handle them.

#### Step 1: Create the Custom Exception Class

```java
// This is our custom exception — it extends Exception (checked)
public class InsufficientFundsException extends Exception {
    public InsufficientFundsException(String message) {
        super(message);
    }
}
```

#### Step 2: Throw It in Your Code

Since this is a **checked** exception, the method that throws it **must declare** it using `throws`:

```java
public class BankAccount {
    private double balance;

    public BankAccount(double balance) {
        this.balance = balance;
    }

    // ⬇️ Notice the 'throws' keyword in the method signature
    public void withdraw(double amount) throws InsufficientFundsException {
        if (amount > balance) {
            throw new InsufficientFundsException(
                "Cannot withdraw ₹" + amount + ". Available balance: ₹" + balance
            );
        }
        balance -= amount;
        System.out.println("✅ Withdrawn: ₹" + amount + " | Remaining: ₹" + balance);
    }
}
```

> 🧠 **Why `throws` in the method signature?**
> Because it's a checked exception — Java says: *"This method might fail. Warn everyone who calls it!"*
> The `throws` keyword is like a **warning label** ⚠️ on the method.

#### Step 3: Handle It with try-catch (Mandatory!)

The compiler **forces** you to handle checked exceptions — you can't ignore them:

```java
public class Main {
    public static void main(String[] args) {
        BankAccount account = new BankAccount(1000);

        try {
            account.withdraw(500);     // ✅ Works
            account.withdraw(800);     // 💥 Throws InsufficientFundsException
        } catch (InsufficientFundsException e) {
            System.out.println("⚠️ Transaction failed: " + e.getMessage());
        }

        System.out.println("Program continues!");
    }
}
```

**Output:**

```text
✅ Withdrawn: ₹500.0 | Remaining: ₹500.0
⚠️ Transaction failed: Cannot withdraw ₹800.0. Available balance: ₹500.0
Program continues!
```

> ❌ **What happens if you DON'T handle it?**
> ```java
> // This WON'T COMPILE!
> public static void main(String[] args) {
>     BankAccount account = new BankAccount(1000);
>     account.withdraw(2000);   // ❌ Compiler Error: Unhandled exception
> }
> ```
> The compiler says: *"You must handle `InsufficientFundsException` with try-catch or declare it with throws!"*

---

### 🆚 Custom Unchecked vs Custom Checked — Side-by-Side

| Feature | Custom Unchecked 🍌 | Custom Checked ✈️ |
|---------|---------------------|--------------------|
| **Extends** | `RuntimeException` | `Exception` |
| **Compiler forces handling?** | ❌ No | ✅ Yes |
| **Needs `throws` in method?** | ❌ No | ✅ Yes |
| **Caused by** | Code mistakes (bugs) | External problems |
| **If not handled?** | Compiles but may crash 💥 | Won't compile ❌ |
| **Example use case** | Invalid age, negative price | No funds, file missing |
| **Analogy** | Banana peel 🍌 — your fault | Flat tyre ✈️ — not your fault |

---

### 🧠 `throw` vs `throws` — Don't Confuse Them!

These two look similar but do **completely different** things:

| Feature | `throw` | `throws` |
|---------|---------|----------|
| **What it is** | A **statement** | A **declaration** |
| **Where it's used** | Inside a method body | In the method signature |
| **Purpose** | Actually **creates and throws** the exception | **Warns** that this method might throw an exception |
| **How many?** | Throws **one** exception at a time | Can declare **multiple** exceptions |

---

### 📝 Quick Recap

```text
throw   →  Manually create and throw an exception object
throws  →  Declare that a method might throw an exception

Custom Unchecked  →  extends RuntimeException  →  compiler won't force handling
Custom Checked    →  extends Exception          →  compiler WILL force handling
```


