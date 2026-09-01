# 04-Software-Basics

Welcome to the fourth module report in the **Pre-Security Path**. This document provides a technical synthesis covering binary representation, encoding schemes, scripting environments (Python & JavaScript/Node.js), and persistent database structures (SQL).

---

## 📄 Client Vulnerability Briefing (Technical Summary)

> **Security Assessment Context:**  
> Modern applications operate across a stack spanning low-level data representation, transport encoding, dynamic script execution, and backend database interaction. Flaws across these layers lead to distinct vulnerabilities:
> - **Encoding Discrepancies:** Differences between character sets (e.g., ASCII vs multi-byte UTF-8) can be abused to bypass Web Application Firewalls (WAFs) or input sanitization filters.
> - **Client-Side Logic Manipulation:** Relying on client-side script execution (such as JavaScript) for core validation allows attackers to bypass business logic and state restrictions.
> - **Database Persistence & Ingestion Flaws:** Direct interpolation of unsanitized input into database abstraction layers exposes applications to **SQL Injection (SQLi)**, leading to authentication bypass or data exfiltration.

---

## 🎨 1. Data Representation

Computers process and store all data as binary bits (`0` and `1`). Grouping and interpreting these bits determines system functionality and technical assessment methods.

### Number Systems Matrix

| Base System | Name | Character Set | Primary Security / Technical Use |
| :--- | :--- | :--- | :--- |
| **Base-2** | Binary | `0, 1` | Low-level memory structure & bitwise operations |
| **Base-8** | Octal | `0 - 7` | UNIX/Linux filesystem permission masks (`chmod 755`) |
| **Base-10** | Decimal | `0 - 9` | Standard numeric operations and counters |
| **Base-16** | Hexadecimal | `0 - 9, A - F` | Memory addressing, shellcode analysis & HTTP byte streams |

### Color Representation Models
- **3-Bit Model:** Represents 8 basic colors using individual bits for Red, Green, and Blue channels.
- **24-Bit / True Color (RGB):** Utilizes 3 Bytes (8 bits per channel: Red, Green, Blue) ranging from `0` to `255` ($2^8$).
  - *Example:* The binary sequence `10100011 11101010 00101010` maps to Hexadecimal `#A3EA2A`.

---

## 🔤 2. Data Encoding Standards

Encoding defines the mapping between binary values in memory and human-readable text characters or control codes.

### Encoding Standards Overview
1. **ASCII (7-bit):**
   - Maps values `0-127` to basic English letters, numbers, and system control codes.
   - *Example:* Character `A` = Decimal `65` = Hexadecimal `41` = Binary `01000001`.
2. **Extended ASCII / ISO-8859:**
   - Employs 8 bits (256 slots) for regional character sets, leading to parsing inconsistencies between systems.
3. **Unicode & UTF-8:**
   - **Unicode:** Assigns a unique Code Point (`U+XXXX`) to every global character and symbol.
   - **UTF-8:** A variable-width encoding (1 to 4 bytes) backward-compatible with ASCII. It prevents character truncation flaws when processing multi-byte input.

---

## 🐍 3. Python: Simple Demo

Python is a high-level dynamically typed language used extensively for exploit script development, tool building, and automation.

### Key Constructs Evaluated
- **Explicit Type Casting:** Inputs read via `input()` return string values (`str`). Numerical manipulation requires explicit conversion using `int()`.
- **Flow Control:** Conditional execution relies on `if`, `elif`, and `else` blocks to enforce logical branching.

```python
import random

# Game setup
secret = random.randint(1, 20)
tries = 0

text = input("Take a guess: ")
guess = int(text)  # String to integer conversion
tries += 1

if guess < 1 or guess > 20:
    print("That number is out of range. Try again.")
elif guess < secret:
    print("Too low, try again.")
elif guess > secret:
    print("Too high, try again.")
else:
    print(f"You got it in {tries} tries!")

🌐 4. JavaScript: Simple Demo (Node.js)

JavaScript operates both on the client side inside web browsers and server side using the Node.js runtime.
Key Constructs Evaluated

    Variable Declarations:

        let: Used for re-assignable block-scoped variables (e.g., tracking tries and guess).

        const: Holds read-only constants (e.g., module imports and static values).

    Asynchronous Execution: Terminal input via node:readline/promises uses await rl.question(). Input strings are converted via parseInt(text, 10).

    Loop Control: The while (guess !== secret) loop maintains process execution until state criteria are satisfied.

JavaScript

import * as readline from "node:readline/promises";
import { stdin as input, stdout as output } from "node:process";

const rl = readline.createInterface({ input, output });

try {
    const secret = Math.floor(Math.random() * 20) + 1; // Generates 1 to 20
    let tries = 0;
    let guess = 0;

    console.log("I'm thinking of a number between 1 and 20");

    while (guess !== secret) {
        const text = await rl.question("Take a guess: ");
        guess = parseInt(text, 10);
        tries = tries + 1;

        if (guess < 1 || guess > 20) {
            console.log("That number is out of range. Try again.");
        } else if (guess < secret) {
            console.log("Too low, try again.");
        } else if (guess > secret) {
            console.log("Too high, try again.");
        } else {
            console.log("You got it in", tries, "tries!");
        }
    }
} finally {
    rl.close();
}

🗄️ 5. Database SQL Basics

Volatile memory (RAM) is erased when applications terminate. Relational Database Management Systems (RDBMS) provide structured, persistent storage.
Relational Model Structure

    Tables: Organized datasets formatted into rows and columns.

    Columns (Attributes): Define parameter data types across records (e.g., Order_ID, Item, Price).

    Rows (Records): Individual instances of data within a table. Adding or removing records affects row count without altering schema.

Structured Query Language (SQL)

SQL enables fast retrieval and filtration of persistent records:
SQL

-- Retrieving filtered records from a table
SELECT drink, price FROM cafe_orders WHERE price < 5.00;
