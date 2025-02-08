### **Prologue in Assembly Language**
A **prologue** in assembly language refers to the **setup code at the beginning of a function** that prepares the **stack frame** before executing the function body. 
The prologue typically **saves the caller's state**, **allocates stack space**, and **sets up the base pointer** for local variables.

---

## **Why is the Prologue Needed?**
1. **Preserves caller's stack state** (so it can be restored after function execution).
2. **Creates a new stack frame** for local variables and parameters.
3. **Maintains the base pointer (`rbp` or `ebp`)** for easier stack access.
4. **Ensures proper function return** by storing the return address.

---

## **Typical Function Prologue in x86/x86-64 Assembly**
When a function is called, the **calling convention** determines how parameters are passed and how the stack is managed.

### **x86 (32-bit) Function Prologue**
```assembly
push ebp        ; Save old base pointer
mov ebp, esp    ; Set new base pointer
sub esp, 16     ; Allocate space for local variables
```
- `push ebp` → Saves the caller's **base pointer** (`ebp`) on the stack.
- `mov ebp, esp` → Sets the **base pointer** (`ebp`) to the **current stack pointer** (`esp`).
- `sub esp, 16` → Allocates **16 bytes** on the stack for local variables.

---

### **x86-64 (64-bit) Function Prologue**
```assembly
push rbp        ; Save caller's base pointer
mov rbp, rsp    ; Set new base pointer
sub rsp, 32     ; Allocate 32 bytes for local variables
```
- `push rbp` → Saves the old **base pointer** (`rbp`).
- `mov rbp, rsp` → Establishes the **stack frame**.
- `sub rsp, 32` → Allocates **32 bytes** for local storage.

---

## **Function Epilogue (Cleanup)**
At the end of a function, the **epilogue** restores the previous stack frame and returns to the caller.

### **x86 Epilogue**
```assembly
mov esp, ebp    ; Restore stack pointer
pop ebp         ; Restore old base pointer
ret             ; Return to caller
```

### **x86-64 Epilogue**
```assembly
mov rsp, rbp    ; Restore stack pointer
pop rbp         ; Restore old base pointer
ret             ; Return to caller
```

---

## **Example: Full Function with Prologue & Epilogue**
### **C++ Code**
```cpp
int add(int a, int b) {
    int sum = a + b;
    return sum;
}
```

### **Equivalent x86-64 Assembly (Using System V Calling Convention)**
```assembly
add:
    push rbp        ; Save base pointer
    mov rbp, rsp    ; Set up stack frame
    mov eax, edi    ; Move first parameter (a) into eax
    add eax, esi    ; Add second parameter (b) to eax
    pop rbp         ; Restore base pointer
    ret             ; Return result in eax
```
- `edi` and `esi` are registers holding function arguments.
- `eax` stores the return value.
- `push rbp` and `mov rbp, rsp` form the **prologue**.
- `pop rbp` and `ret` form the **epilogue**.

---

## **Summary**
| **Stage**  | **Instruction** | **Purpose** |
|------------|---------------|-------------|
| **Prologue** | `push rbp` | Save old base pointer |
| | `mov rbp, rsp` | Create a new stack frame |
| | `sub rsp, X` | Allocate space for local variables |
| **Function Body** | Function code executes | Uses stack frame for local storage |
| **Epilogue** | `mov rsp, rbp` | Restore stack pointer |
| | `pop rbp` | Restore old base pointer |
| | `ret` | Return to caller |

The **prologue** ensures that the function can safely execute while preserving the caller’s context. 🚀
