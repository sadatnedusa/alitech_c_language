### ✅ `switch-case` is **faster** than `if-else-if` **in many scenarios**, especially:

* When you are comparing the same variable against many constant integer values.
* When the values are dense and close together (e.g., 1, 2, 3, 4).

### Why is `switch-case` usually faster?

* **Compiler optimizations**: Compilers often convert `switch` into:

  * A **jump table** (O(1) time) if the case values are contiguous.
  * A **binary search** (O(log n)) if values are sparse but still manageable.
* Whereas `if-else-if` is typically **linear** (O(n)), checking each condition one by one until a match is found.

### Example:

```c
// switch (faster if dense values)
switch (x) {
  case 1: ...; break;
  case 2: ...; break;
  ...
}

// if-else (slower for many conditions)
if (x == 1) { ... }
else if (x == 2) { ... }
...
```
### ⚠️ Caveats:

* If you're doing **complex conditions** (e.g., `if (x > 10 && y < 5)`), then `switch` isn't even an option.
* For **small numbers of conditions**, the difference is negligible.
* Compilers like GCC/Clang/MSVC are smart and might optimize both well.

### TL;DR:

* For many simple comparisons on the **same variable**, use `switch-case` → likely faster.
* For complex logic or different variables/conditions, use `if-else`.

---
Benchmark results for 100,000 executions:

* **`if-elif` chain**: ⏱️ **\~0.0117 seconds**
* **`switch-case` simulation (dict dispatch)**: ⏱️ **\~0.0224 seconds**

### 🧠 Why is `if-elif` faster here in Python?

* Python **does not have a native `switch`**, so we simulated it using a **dictionary of lambdas**.
* The dictionary dispatch adds **function call overhead**, which slows it down slightly compared to direct `if-elif` logic.

### But in **C/C++** or similar compiled languages:

* A real `switch-case` often **outperforms `if-elif`** because of:

  * **Jump tables** (constant-time jumps),
  * **Efficient compiler optimizations**.

### Summary:

| Language   | `if-else`         | `switch-case`                          |
| ---------- | ----------------- | -------------------------------------- |
| **Python** | ✅ Faster          | ❌ Slower (simulated via dict)          |
| **C/C++**  | ❌ Slower (linear) | ✅ Faster (jump table or binary search) |

---

To see actual disassembly in C/C++ for comparison.

Let's walk through a **C example** to demonstrate the actual **assembly/disassembly difference** between `if-else` and `switch-case`. We’ll use GCC with `-O2` optimization to show how the compiler transforms each construct.

## ✅ Example Code (C)

```c
// test.c
int test_if(int x) {
    if (x == 1) return 10;
    else if (x == 2) return 20;
    else if (x == 3) return 30;
    else if (x == 4) return 40;
    else if (x == 5) return 50;
    return -1;
}

int test_switch(int x) {
    switch (x) {
        case 1: return 10;
        case 2: return 20;
        case 3: return 30;
        case 4: return 40;
        case 5: return 50;
        default: return -1;
    }
}
```

## 🛠️ Compile and Disassemble

Use GCC and `objdump` to see the machine code:

```bash
gcc -O2 -S test.c -o test.s      # Assembly output
objdump -d a.out > disasm.txt    # Full disassembly of binary
```

## 🧠 What to Look For

### 1. `test_if` (if-else chain):

You’ll see something like:

```asm
cmp     edi, 1
je      .L1
cmp     edi, 2
je      .L2
...
```

This is a **series of comparisons and jumps** — **linear time**, `O(n)`.

### 2. `test_switch`:

If the values are **dense and close together** (e.g., 1 to 5), the compiler may emit a **jump table**:

```asm
cmp     edi, 5
ja      .Ldefault
jmp     .LJUMPTABLE[edi * 4]
```

The **`jmp` through a table** indexed by `x` is **constant time**, `O(1)`.

## 🔬 Summary

| Construct     | Compiled As                 | Time Complexity  |
| ------------- | --------------------------- | ---------------- |
| `if-else`     | Series of `cmp` + `je/jne`  | O(n)             |
| `switch-case` | Jump table or binary search | O(1) or O(log n) |

---

Would you like:

* The full assembly output for this example?
* A downloadable file with comments?
* Or a visual comparison with annotated explanation?
