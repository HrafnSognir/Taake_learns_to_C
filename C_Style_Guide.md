# **_C Style Guide_**

This style guide is based on the Google C style, trimmed down and adjusted
for clarity and practical day-to-day use in small projects and hobby games/tools development.  
The goal is simple: readable, maintainable and consistent code.

---

### **_File Structure:_**

Use one `.h` and one `.c` per module.

```
player.h
player.c
```

Header files must have include guards:

```
#ifndef PLAYER_H
#define PLAYER_H

/* declarations */

#endif
```

Headers include only what they need. Keep them minimal.

---

### **_Naming:_**

Use descriptive names.

- `snake_case` for functions and variables.
- `UPPER_CASE` for macros and constants.
- Avoid unclear abbreviations.

Example:

```
const int MAX_PLAYERS = 5;

int load_level(const char *path);
```

---

### **_Formatting:_**

- Four spaces for indentation.
- Braces on the same line.
- Always use braces, even for single statements.
- Keep lines under 100 characters.

```
if (is_ready) {
    start_game();
} else {
    log_error("Not ready");
}
```

---

### **_Functions:_**

Keep functions focused. Return instead of nesting.

```
int read_file(const char *path, Buffer *out) {
    if (!path || !out) {
        return ERR_INVALID;
    }

    FILE *f = fopen(path, "rb");
    if (!f) {
        return ERR_OPEN_FAILED;
    }

    /* ... work ... */

    fclose(f);
    return OK;
}
```

---

### **_Pointers and const:_**

Use `const` wherever something should not change.  
Place a `*` next to the variable name, not the type.

```
void print_message(const char *msg);
int process_values(int *values, size_t count);
```

---

### **_Magic Numbers:_**

Don't leave raw numbers "hidden" in code. Use named constants or enums.

```
const int MAX_ENEMIES = 25;

for (int enemy = 0; enemy < MAX_ENEMIES; enemy++) {
    spawn_enemy(enemy);
}
```

---

### **_Error Handling:_**

Always check return values.  
Return error codes, not arbitrary numbers.

```
int save_score(const char *path, int score) {
    FILE *f = fopen(path, "w");
    if (!f) {
        return ERR_OPEN_FAILED;
    }

    if (fprintf(f, "%d\n", score) < 0) {
        fclose(f);
        return ERR_WRITE_FAILED;
    }

    fclose(f);
    return OK;
}
```

---

### **_Memory:_**

Every `malloc` has a matching `free`.  
Make ownership clear.

```
char *buffer = malloc(size);
if (!buffer) {
    return ERR_OUT_OF_MEMORY;
}

/* ... use buffer ... */

free(buffer);
```

---

### **_Comments:_**

Comment intent, not mechanics.  
Explain "why" not "what".

```
/* BAD: says nothing useful */
cache_enemy_data();  // Cache enemy data

/* GOOD: explains the purpose, not the action */
cache_enemy_data();  // Avoids recalculating enemy stats every frame
```

---

### **_Build:_**

Use strict compiler flags.

```
-Wall -Wextra -Werror
```

During development, treat warnings as errors.

```
-Werror
```
