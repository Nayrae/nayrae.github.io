---
{"publish":true,"created":"2025-12-15T19:56:33.349+01:00","modified":"2025-12-15T19:59:52.345+01:00","tags":["HTB","PWN"],"cssclasses":""}
---



This challenge is extremely easy, our job will be analyzing a downloaded `.c` file and it's binary, and answer correctly to the questions in asked once connected to the target via nc.

---
# Analyzing
Let's start by seeing what are we going to work with:
![[files/Pasted image 20251215230440.png]]

Let's read the `test.c`
![[files/Pasted image 20251215210630.png]]

Let's start by reading the `test.c`
![[files/Pasted image 20251215210630.png]]

And here's the checksec output from `GDB` plus 
![[Pasted image 20251215224930.png]]
![[Pasted image 20251215233217.png]]

And here's the checksec output from `GDB`
![[files/Pasted image 20251215224930.png]]
![[files/Pasted image 20251215233217.png]]
## findings
- the file is a `64-bit`  `ELF` , `dynamically linked` and `not stripped`
- the `main()` function calls `vuln()`, `gg()` is never called
- `vuln()` uses `fgets`, which reads a total of $0x100 == 256_{10}$ characters
-  the `char buffer` fills $[0x20]$ bytes (which equals to $32_{10}$ `bytes`) with `0`s
	- `char` takes `8 bits` of space, this means that if we input more than 32 chars we're going to get an overflow
- since `Canary` and `Fortify` are disabled, there is no protection against buffer overflows
	- `PIE`, if enabled, it allows the executable file to be loaded at different memory addresses each time it executes, fortifying the program against buffer overflows 
---
# Tests

Let's test it with the binary:
- after running the binary we're being asked for an input
- as predicted, if we input 32 chars, the program executes normally
- however, if we input 40 chars the program crashes 

![[files/Pasted image 20251215223438.png]]

---

# Flag 

Let's spawn the machine and connect to it using `nc`, we will get a tutorial of how to analyze the files followed by the questions
![[files/Pasted image 20251215230912.png]]![[files/Pasted image 20251215231158.png]]
![[files/Pasted image 20251215233109.png]]


  

