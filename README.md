# ParrotVector 🦜

**A lightweight, cross-platform, C-style "Class" vector implementation for Systems Programming and UEFI development.**

`ParrotVector` is a header-only library that brings C++ vector-like functionality to pure C environments. It is specifically designed to work in restricted environments like OS kernels, UEFI applications, and embedded systems, while remaining compatible with standard user-space applications.

## Key Features

* **Zero Dependencies:** Works with `stdint.h` and `stddef.h`.
* **C++ Style Syntax:** Uses function pointers to simulate methods (`v.Push(&v, ...)`).
* **Dual-Mode Memory:** Supports both standard `malloc/free` and UEFI `BootServices`.
* **ID-Based Retrieval:** Each element can have a custom `int32_t ID` for fast lookups.

---

---

## Installation

Just copy `ParrotVector.h` into your project's include directory.

## Usage

### 1. Standard C (Windows/Linux)

Simply include the header and use it. It will automatically use `stdlib.h`.

```c
#include "Vector.h"
#include <stdio.h>

int main() {
    Vector v;
    VectorInit(&v, 4); // Initial capacity

    int myData = 100;
    v.Push(&v, 101, &myData); // Store data with ID 101

    int* retrieved = (int*)v.GetById(&v, 101);
    printf("Value: %d\n", *retrieved);

    v.Clear(&v);
    return 0;
}

```

### 2. UEFI Development (ParrotOS / EDK2)

Define `__UEFI__` before including the header or via compiler flags (`-D__UEFI__`). This switches the library to use `gBS->AllocatePool` and `gBS->FreePool`.

```c
#define __UEFI__
#include "Vector.h"

EFI_STATUS UefiMain(...) {
    Vector procList;
    VectorInit(&procList, 10);
    
    // Works with UEFI memory management!
    procList.Push(&procList, 1, currentProcess);
}

```

---

## API Reference

### Methods

| Method | Description |
| --- | --- |
| `Push(self, id, data)` | Adds an element and assigns a custom ID. |
| `GetById(self, id)` | Searches for an element by its custom ID. |
| `GetAt(self, index)` | Accesses an element by its position in the array (0 to size-1). |
| `Remove(self, id)` | Removes the element with the specified ID and shifts the array. |
| `Clear(self)` | Frees all allocated memory and resets the vector. |

### Properties

* `v.size`: Number of elements currently in the vector.
* `v.capacity`: Total allocated slots in memory.

---

## License

MIT License. Feel free to use it in your own OS or projects!
