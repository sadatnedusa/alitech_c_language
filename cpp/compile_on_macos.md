You're seeing **linker errors** because you're trying to compile a C++ source file (`hello.cpp`) using the `gcc` compiler, which is meant for C, **not C++**.

### ✅ Solution

Use `g++` instead of `gcc`:

```bash
g++ hello.cpp -o hello
```

This ensures that the C++ standard library (`libc++` or `libstdc++`) is properly linked.

---

### 🔍 Explanation of the Error

The linker errors like:

```
Undefined symbols for architecture x86_64:
  "std::__1::locale::use_facet..."
```

...indicate that the compiler compiled the source file, but the **linker failed to resolve symbols** from the C++ standard library (like `std::cout`, `std::locale`, `std::string`, etc.), which `gcc` does **not** automatically include.

---

### 🔄 Alternative (if you still want to use `gcc`)

If you must use `gcc` to compile C++ files, you must **explicitly link the C++ standard library**:

```bash
gcc hello.cpp -lstdc++ -o hello
```

But this is **not recommended**. Use `g++` for C++ files.

