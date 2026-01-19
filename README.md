# skillforge

> Automatically turn real-world coding workflows into reusable Claude Code Skills.

## ✨ What is this?

When you solve a programming problem, you implicitly perform a *workflow*:

* understanding the problem
* exploring options
* making decisions
* writing and refining code

This project captures that workflow and **automatically converts it into a reusable Claude Code Skill**.

In short:

**coding session → structured workflow → Claude Code skill**

---

## 🎯 Why does this matter?

* Writing skills manually is slow and error-prone
* Most valuable skills already exist implicitly in your past work
* We waste solved knowledge by not turning it into reusable tools

This project lets you:

* distill experience into skills
* reuse solutions across projects
* scale your problem-solving能力

---

## 🧩 Core Idea

We treat a solved coding task as **data**, not just text.

The workflow is:

1. Capture the problem-solving trace
2. Extract intent, steps, and constraints
3. Normalize into a minimal skill schema
4. Output a ready-to-use Claude Code Skill

---

## 📦 Input → Output

**Input**

* A coding session (chat log, terminal history, editor diff, or markdown notes)

**Output**

* A Claude Code Skill (YAML / JSON / Markdown)

---

## 🚀 Current Status

* [x] Concept validated
* [x] Minimal skill schema defined
* [ ] Script-based prototype
* [ ] CLI tool
* [ ] Claude-native integration

---

## 🛠️ Prototype Usage (Script Mode)

```bash
python generate_skill.py session.md
```

---

## 📐 Skill Schema

See: `docs/skill_schema.md`

---

## 🧠 Design Philosophy

* Skills are distilled workflows, not code snippets
* Minimal abstraction first, extensible later
* Human-readable > magic automation

---

## 📄 License

MIT
