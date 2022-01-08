# Shellcode Primer



I found this [cheat sheet from Brown University cs department](https://cs.brown.edu/courses/cs033/docs/guides/x64_cheatsheet.pdf) to help reinforce what I am learning in the challenge.  We will skip a few of the early parts of this challenge that are mostly explanatory.  For the challenges I do go over, I will include the notes associated with each challnge



## Challenge 4. Returning a Value

Challenge Notes:

>A register is like a variable, except there are a small number of them - you have about eight general purpose 64-bit integers registers on amd64 (we won't talk about floating point or other special registers):
>
>    rax
>    rbx
>    rcx
>    rdx
>    rdi
>    rsi
>    rbp
>    rsp
>
>All mathy stuff that a computer does (add, subtract, xor, etc) operates on registers, not directly on memory. So they're super important!
>
>Specific registers have some implicit meaning, mostly by convention. For example, when a function returns, its return value is typically put in rax. Let's do that!
>
>To move a value into a register, use the mov instruction; for example:
>
>mov rdx, 1
>
>In a higher-level language this would be equivalent to:
>
>rdx = 1 —



Part 4 of the challenge is where we really start to write shellcode.  We have to return the number 1337.  Based off of what we learned above, I'll use the rax register
```
; TODO: Set rax to 1337
mov rax, 1337

; Return, just like we did last time
ret
```

## Challenge 5. 

Challenge Notes:

>If you've made it this far, I bet you're wondering how to make your shellcode do something!
>
>If you're familiar with Python, you might know how to use the open() function. If you know C, you might know the fopen() function. But what these and similar functions have in common is one thing: they're library code. And because shellcode needs to be self contained, we don't have (easy) access to library code!
>
>So how do we deal with that?
>
>Linux has something called a syscall, or system call. A syscall is a request that a program makes that asks Linux - the kernel - to do something. And it turns out, at the end of the day, all of those library calls ultimately end with a syscall. [Here is a list of available syscalls on x64 (alternative)](https://blog.rchapman.org/posts/Linux_System_Call_Table_for_x86_64/)
>
>To perform a syscall:
>
>    - The number for the desired syscall is moved into rax
>    - The first parameter is moved into rdi, the second into rsi, and the third into rdx (there are others, but not many syscalls need more than 3 parameters)
>    - Execute the syscall instruction
>
>The second syscall executes, Linux flips into kernel mode and we can no longer debug it. When it's finished, it returns the result in rax.

For this Challenge, We have to execute a system_exit call

```
; TODO: Find the syscall number for sys_exit and put it in rax
mov rax, 60

; TODO: Put the exit_code we want (99) in rdi
mov rdi, 99
; Perform the actual syscall
syscall

```

## Challenge 7: Getting Rip

>What happened in the last exercise? Why did it crash at 0x12345678? And did you notice that 0x12345678 was on top of the stack when ret happened?
>
>The short story is this: call pushes the return address onto the stack, and ret jumps to it. Whaaaat??
>
>This is going to be long, but hopefully will make it all clear!
>
>Let's back up a bit. At any given point, the instruction currently being executed is stored in a special register called the instruction pointer (rip), which you may also hear called a program counter (pc).
>
>What is the rip value at the first line in our code? Well, since we have a debugger, we know that it's 0x13370000. But sometimes you don't know and need to find out.
>
>The most obvious answer is to treat it like a normal register, like this:
>
>mov rax, rip
>ret
>
>Does that work? Nope! You can't directly access rip. That means we need a trick!
>
>When you use call in x64, the CPU doesn't care where it's calling, or whether there's a ret waiting for it. The CPU assumes that, if the author put a call in, there will naturally be a ret on the other end. Doing anything else would just be silly! So call pushes the return address onto the stack before jumping into a function. When the function complete, the ret instruction uses the return address on the stack to know where to return to.
>
>The CPU assumes that, sometime later, a ret will execute. The ret assumes that at some point earlier a call happened, and that means that the top of the stack has the return address. >The ret will retrieve the return address off the top of the stack (using pop) and jump to it.
>
>Of course, we can execute pop too! If we pop the return address off the stack, instead of jumping to it, the address goes into a register. Hmm! Does that also sound like mov REG, rip to you?

For this challenge, we just need to add the pop line.

```
; Remember, this call pushes the return address to the stack
call place_below_the_nop

; This is where the function *thinks* it is supposed to return
nop

; This is a 'label' - as far as the call knows, this is the start of a function
place_below_the_nop:

; TODO: Pop the top of the stack into rax
pop rax

; Return from our code, as in previous levels
ret


```

## Challenge 8: Hello World!

>So remember how last level, we got the address of nop and returned it?
>
>Did you see that nop execute? Nope! We jumped right over it, but stored its address en-route. What can we do by knowing our own address?
>
>Well, since shellcode is, by definition, self-contained, you can do other fun stuff like include data alongside the code!
>
>What if the return address isn't an instruction at all, but a string?

For this challenge, we are getting a pointer to the string "Hello World"
```
; This would be a good place for a call
call hello_world

; This is the literal string 'Hello World', null terminated, as code. Except
; it'll crash if it actually tries to run, so we'd better jump over it!
db 'Hello World',0

; This would be a good place for a label and a pop
hello_world:


pop rax
; This would be a good place for a re... oh wait, it's already here. Hooray!
ret

```

## Challenge 9: Hellp World!!

>Remember syscalls? Earlier, we used them to call an exit. Now let's try another!
>
>This time, instead of getting a pointer to the string Hello World, we're going to print it to standard output (stdout).
>
>Have another look at the syscall table. Can you find sys_write, and use to to print the string Hello World! to stdout?

This time, we are actually printing the string instead of just getting the pointer to it.  I got stuck a bit here but figured out I had to pop rsi set it to the Hello World string

```
; TODO: Get a reference to this string into the correct register
call hello_world
db 'Hello World!',0

; Set up a call to sys_write
hello_world:

; TODO: Set rax to the correct syscall number for sys_write
mov rax, 1


; TODO: Set rdi to the first argument (the file descriptor, 1)
mov rdi, 1

; TODO: Set rsi to the second argument (buf - this is the "Hello World" string)
pop rsi

; TODO: Set rdx to the third argument (length of the string, in bytes)
mov rdx, 12



; Perform the syscall
syscall

; Return cleanly
ret

```

## Challenge 10:Open a file

For this challenge, we are going to use the sys_open syscall to open a password file

```
; TODO: Get a reference to this string into the correct register
call open_file
db '/etc/passwd',0

; Set up a call to sys_open
open_file:

; TODO: Set rax to the correct syscall number
mov rax, 2

; TODO: Set rdi to the first argument (the filename)
pop rdi

; TODO: Set rsi to the second argument (flags - 0 is fine)
mov rsi, 0

; TODO: Set rdx to the third argument (mode - 0 is also fine)
mov rdx, 0

; Perform the syscall
syscall

; syscall sets rax to the file handle, so to return the file handle we don't
; need to do anything else!
ret

```

## Challenge 11: Reading a File

```

```
