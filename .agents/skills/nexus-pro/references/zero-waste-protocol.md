# 🛑 Zero-Waste Context Protocol (ZWCP)
## The Token Optimization Engine for Antigravity

This protocol strictly defines how Antigravity must interact with the terminal and file system to prevent massive token consumption (Log-Dumping, Context Amnesia, and Conversational Echo).

### 1. Intelligent Input Filtering (No Full-File Dumps)
Before reading raw data, the AI must classify the input. **Full-file reading is strictly forbidden for files larger than 150 lines unless actively refactoring the entire file.**

- **Large Source Files (>150 lines):** Do not read the whole file. Use `grep_search` to find the signature of the function or class, then use `view_file` to read ONLY the specific lines (`StartLine`, `EndLine`).
- **Terminal Builds/Installs (npm, composer, docker):** Never output raw builds. Pipe and grep for errors:
  `npm run build 2>&1 | grep -iE "(error|fail|warn|fatal)"`
- **Error Logs (laravel.log, syslog):** Never read raw logs. Extract the latest stack trace:
  `tail -n 100 storage/logs/laravel.log | grep -A 20 -B 5 -iE "(error|exception|stack trace)"`
- **Git History:** Never `git log` without limits. Always limit to recent commits:
  `git log --oneline -n 10`

### 2. Surgical Output (Zero-Echo Rule)
Output tokens are highly expensive and slow. The AI must be surgical when writing code.

- **No Conversational Echo:** Never restate the user's prompt or repeat the problem. Start immediately with the diagnosis or the 5 Whys.
- **No Filler Text:** Eliminate phrases like "Here is the code", "I understand", "Let me help you with that".
- **Surgical Edits:** If changing 1 line in a 300-line file, **DO NOT print the whole file**. Use the `multi_replace_file_content` tool strictly. If writing manually, use `Diff` blocks (`--- a/file` `+++ b/file`).
- **Batched Edits:** If making multiple changes in a single file, batch them in one response rather than executing them one by one.

### 3. Context Pressure & Amnesia Defense
- If a file has already been read in the current session, **do not read it again** unless the user or another agent modified it. Assume the context is retained.
- If a session becomes too long and the active memory window surpasses 80%, immediately invoke the **Memory Offloading Protocol** (see `memory-offloading.md`).

> **Golden Rule of ZWCP:** Every command execution and file read must be the narrowest, most restrictive query possible. If a command outputs more than 50 lines of text, it was a bad command.
