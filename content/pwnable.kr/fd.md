---
{"publish":true,"created":"2025-12-17T10:27:32.786+01:00","modified":"2025-12-17T10:44:51.620+01:00","tags":["CTF","PWN","Easy","C","Linux-I/O"],"cssclasses":""}
---

> Mommy! what is a file descriptor in Linux?

# Challenge

After connecting to the machine with `ssh` and `ls`, we can see the 3 files of the challenge:
- `fd`
- `fd.c`
- `flag`
To read the flag we must find a way to enter the if statement
![[files/Pasted image 20251217103201.png]]
```
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
char buf[32];
int main(int argc, char* argv[], char* envp[]){
	if(argc<2){
		printf("pass argv[1] a number\n");
		return 0;
	}
	int fd = atoi( argv[1] ) - 0x1234;
	int len = 0;
	len = read(fd, buf, 32);
	if(!strcmp("LETMEWIN\n", buf)){
		printf("good job :)\n");
		setregid(getegid(), getegid());
		system("/bin/cat flag");
		exit(0);
	}
	printf("learn about Linux file IO\n");
	return 0;

}
```
# Observations

Let's analyze the code in parts

## First step
- `char buf[32];` is declared
- we eneter the main functions which checks for `argc[2]`
	- if it isn't declared it asks for a numer as `argv[1]`
	- 