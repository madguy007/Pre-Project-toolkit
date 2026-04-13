# 🧠 AI Setup Mind Map (Windows vs WSL vs Ubuntu)

## 🧩 System Overview

```text
Your Laptop
   ├── 🪟 Windows (Host OS)
   │       └── System control (CMD, files, config)
   │
   └── 🐧 WSL (Linux layer)
           └── Ubuntu (your working environment)
                   ├── Ollama
                   ├── Models
                   └── OpenClaw
```

---

## 🪟 Windows (Host Layer)

### 👉 Use for:

* Starting WSL → `wsl`
* Config → `.wslconfig`
* Restart → `wsl --shutdown`
* File management (basic)

### ❌ Don’t use for:

* Running Ollama
* Running models
* Installing AI tools

---

## 🐧 WSL (Linux Layer)

### 👉 This is your MAIN working area

* Real Linux environment
* Where all AI tools run

---

## 🐧 Ubuntu (inside WSL)

### 👉 You will see:

```bash
madan@LAPTOP:~
```

### 👉 Use for:

* Install Ollama
* Run models
* Run OpenClaw
* Development work

---

## 📂 File System Rule (VERY IMPORTANT)

### ❌ Avoid:

```bash
/mnt/c/Users/madan
```

### ✅ Always use:

```bash
/home/madan
# OR
~
```

### 🧠 Why:

* `/mnt/c` → Windows filesystem (slow + unstable)
* `/home` → Linux filesystem (fast + reliable)

---

## ⚙️ Command Separation

| Task                    | Where        |
| ----------------------- | ------------ |
| `wsl`, config, restart  | Windows      |
| `apt`, `ollama`, models | WSL (Ubuntu) |

---

## 🔁 Workflow (Correct Flow)

```text
1. Windows → open Terminal / CMD / PowerShell
2. Type `wsl` to enter WSL
3. In WSL → use `cd ~` to go to home
4. Install/run Ollama
5. Run model
6. Build AI agent
```

---

## ⚠️ Common Mistakes

* Mixing Windows + WSL installs ❌
* Working in `/mnt/c` ❌
* Using large models first ❌
* Not restarting WSL after config ❌

---

## ✅ Golden Rules

* ✔ Windows = control layer
* ✔ WSL/Ubuntu = working layer
* ✔ Always `cd ~` before work
* ✔ Start small → scale later

---

## 🔑 Final One-Liner

👉 Windows controls, WSL runs, Ubuntu builds — always work in `~` not `/mnt/c`.
