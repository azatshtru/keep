In a world of endless plugins and heavy configs, the simple git difftool and git mergetool stand as super lightweight, yet powerful alternatives that integrate well with the existing editor ecosystem. Abiding by the GNU philosophy of doing one thing and doing it well, I present the a workflow guide with git difftool and git mergetool.

---
### 🔹 Open

- Diff two files:
    
    ```bash
    nvim -d file1 file2
    ```
    
- Use Git:
    
    ```bash
    git difftool
    git mergetool
    ```
    

---

### 🔹 Navigation

- `]c` → jump to **next change**
    
- `[c` → jump to **previous change**
    

---

### 🔹 Apply changes

- `do` (**diff obtain**) → bring change **from other window → into current**
    
- `dp` (**diff put**) → send change **from current window → to other**
    

---

### 🔹 Folds (hidden unchanged lines)

- `zR` → **open all folds** (show everything)
    
- `zM` → **close all folds**
    
- `zo` → open fold under cursor
    
- `zc` → close fold under cursor
    

---

### 🔹 When resolving merge conflicts

- Git opens 3–4 panes (`LOCAL`, `REMOTE`, `BASE`, `MERGED`).
    
- Edit the **MERGED** buffer.
    
- Use `do` / `dp` between panes to pull in chunks.
    
- Save & quit MERGED when done.
    

---

✅ **Minimal workflow:**

1. Run `git mergetool`.
    
2. In Neovim: press `zR` to see all lines.
    
3. Jump conflicts with `[c` / `]c`.
    
4. Use `do`/`dp` to copy hunks between versions.
    
5. Edit the **MERGED** buffer, save, quit.
    

---

That’s it - clean, portable, no config required.