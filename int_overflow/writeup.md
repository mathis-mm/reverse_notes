The executable asks for a password. If the correct password is entered, it prints "Password Correct!!!", otherwise "Incorrect Password!!!".

# Step 1 – Analysis with Ghidra

Using Ghidra, we find a function GetPass() that prints a message and reads a string with scanf. It returns this string.

In main, the code looks like this (interpret code):

__nptr = GetPass();
iVar1 = atoi(__nptr);
if (iVar1 == -0x2023e3c2) {
  printf("Password Correct!!!");
} else {
  printf("Incorrect Password!!!");
}


At first glance, the program compares the integer value of the input to -0x2023e3c2, which is -539215298.

# Step 2 – Running the program

We test the password with -539215298:

$ ./pass
input your password here: -539215298
Incorrect Password!!!

It does not work.

# Step 3 – Check the disassembly

Looking at the raw disassembly in Ghidra, we see:

CMP dword ptr [RBP + local_14], 0xdfdc1c3e

The value compared is actually 0xDFDC1C3E.

Converted to unsigned decimal:

0xDFDC1C3E = 3755744318

So the program expects this number.

# Step 4 – Final test

We enter 3755744318:

$ ./pass
input your password here: 3755744318
Password Correct!!!

Password found.
Final password: 3755744318