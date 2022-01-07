# Shellcode Primer
~~~


I found this cheat sheet from Brown University cs department to help reinforce what I am learning in the challenge.  We will skip a few of the early parts of this challenge that are mostly explanatory.  For the challenges I do go over, I will include the notes associated with each challnge



## Challenge 4. Registers

```
A register is like a variable, except there are a small number of them - you have about eight general purpose 64-bit integers registers on amd64 (we won't talk about floating point or other special registers):

    rax
    rbx
    rcx
    rdx
    rdi
    rsi
    rbp
    rsp

All mathy stuff that a computer does (add, subtract, xor, etc) operates on registers, not directly on memory. So they're super important!

Specific registers have some implicit meaning, mostly by convention. For example, when a function returns, its return value is typically put in rax. Let's do that!

To move a value into a register, use the mov instruction; for example:

mov rdx, 1

In a higher-level language this would be equivalent to:

rdx = 1 —

For this level, can you return the number '1337' from your function?

That means that rax must equal 1337 when the function returns.
```


Part 4 of the challenge is where we really start to write shellcode.  We have to return the number 1337.  Based off of what we learned above, I'll use the rax register
```
mov rax, 1337

ret
```
