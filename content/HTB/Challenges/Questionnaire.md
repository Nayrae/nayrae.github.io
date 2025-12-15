---
{"publish":true,"created":"2025-12-15T19:56:33.349+01:00","modified":"2025-12-15T19:59:52.345+01:00","tags":["HTB","PWN"],"cssclasses":""}
---

This challenge is extremely easy, our job will be analyzing a downloaded `.c` file and it's binary, and answer correctly to the questions in asked once connected to the target via nc.

# Analyzing

Let's start by reading the `test.c`
![[Pasted image 20251215210630.png]]
## findings

- the `main()` function calls `vuln()`, `gg()` is never called
- `vuln()` uses `fgets`, a vulnerable command that doesn't limit input based on buffer size
- 