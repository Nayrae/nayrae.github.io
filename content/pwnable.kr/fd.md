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
	z
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

## First step - code reading
- `char buf[32];` is declared
- we enter the main functions which checks for `argc[2]`
	- if it isn't declared it asks for a numer as `argv[1]`
	- variable`fd` is called as `integer`, it equals to `atoi( argv[1] ) - 0x1234`
	- `atoi` converts a string to an integer
	- variable `len` is called as `integer`,  after setting it at `0`, it equals to 
		- `read(fd, buf, 32)`tries to read up to `32 bytes` from `fd` and stores them in `buf`
			- if our `file descriptor` = `0`, the function reads input from `cmd`
			- `read` function, outputs the number of bytes read
	- if statement
		- `!strcmp` function is called
			- `strcmp` function returns `0` if both strings have the same value, a positive one if `string1` > `string2` and a negative one if `string1` < `string2`
		- it reads the `flag` 


## Second step - Calcs

1. to get the flag we have to enter the loop
2. `fd` must equal to `0`
3. since `fd` =  `argv[1] - 0x1234` we have to make sure `argv[1] = 0x1234`
4. $0x1234_{16} = 4660_{10}$ 
5. since `fd` = `0`, we will be asked to input raw-text, we will type `LETMEWIN` to get to the flag

# Solution

![[Pasted image 20251217120318.png]]
