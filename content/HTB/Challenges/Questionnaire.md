---
{"publish":true,"created":"2025-12-15T19:56:33.349+01:00","modified":"2025-12-15T19:59:52.345+01:00","tags":["HTB","PWN"],"cssclasses":""}
---

This challenge is extremely easy, our job will be analyzing a downloaded `.c` file and it's binary, and answer correctly to the questions in asked once connected to the target via nc.

---
# Analyzing

Let's start by reading the `test.c`
![[Pasted image 20251215210630.png]]

And here's the checksec output
## findings

- the `main()` function calls `vuln()`, `gg()` is never called
- `vuln()` uses `fgets`, which reads a total of $0x100 == 256_{10}$ characters
-  the `char buffer`'s size is $[0x20]$ which equals to $32_{10}$
	- `char` takes `8 bits` of space, this means that if we input more than 32 chars we're going to get an overflow

---
# Tests

Let's test it with the binary:
- after running the binary we're being asked for an input

![[Pasted image 20251215223438.png]]