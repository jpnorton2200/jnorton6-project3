JP Norton CS2160-002

Pieces of my program:

Main with canary, gets and puts functions, sekret function with sekret data, messages

Resources reffered to:

projects 1-2

Test cases:

Part 1:

AAAAAAAAAAAAAAAAAAAABBBBCCCC

AAAAAAAAAAAAAAAAAAAA|2



Part 2:

AAAAAAAAAAAAAAAAAAAACNRY

AAAAAAAAAAAAAAAAAAAAYRNC

AAAAAAAAAAAAAAAAAAAACNRY|2

AAAAAAAAAAAAAAAAAAAAYRNC|2


Challenges:

Time took:

6 hours

Documentation of functionality, bugs:

no bugs. Functionality is project 2 complete, along with a canary to protect against stack overflow. If the user entry is larger than the allocated stack space, then the canary is filled with characters which do not match the original contents, which will end the program instead of executing the command or the garbage inside RA.

Screenshots:

(See Below)

Miscellaneous notes:

None.


Part 0: Complete Project 2 (0 pts)
If you did not complete Project 2, you need to complete it, as you need to have a correct implementation of Project 2 part 3 to succesfully complete Project 3.


Part 1: The Stack Canary (40 pts)
In Project 2 Part 3, you caused the provided project2.S to call the function named sekret_fn by exploiting a buffer overflow in gets() and overwriting the RA register of main(), the caller of gets(). Your job in this part is to implement a stack canary, which is a word of data that gets pushed on the stack just below the RA register. You will work from a created copy of your project 2 source code. Add the stack canary implementation to the main() function prolog. Choose a stack canary value that is a printable ASCII or UTF-8 string. You should allocate additional stack space and adjust the stack offsets for saving RA to do this. Then, in the main() function epilog, implement a check to compare the stack canary on the stack with the expected stack canary value that was stored in the prolog. If the values do not match, call the exit syscall through the following ecall:
li a0, 0 li a7, __NR_EXIT ecall
If the values do match, return as usual from main().
In your README report, include the stack canary value you added to main() as both the printable string, and as a hexadecimal value. Show a screenshot of your QtRVSim window showing the register window when reproducing the attack you came up with from Project 2 Part 3 that calls sekret_fn through the buffer overflow.

Value in my Canery: 0x434e5259  aka  "CNRY"

Showing that the overflow protection works. The stored value in the canery is 4242424241 which is equivalent to BBBA. I added a warning message for my own sanity check to make sure that the overflow protection was working:
![image](https://github.com/user-attachments/assets/4529a16a-ca55-4742-b1e9-6c43beea41e2)


Before attempt of attack. Terminal entry will be AAAAAAAAAAAAAAAAAAAA|2, as the address of the sekret_fn is at |2 Nul Nul, aka 0x0000327c:
![image](https://github.com/user-attachments/assets/5afc4aa4-596d-4a6b-adf1-221bc2905d74)


after attack attempt. in this case the value of 0x434e5259 is overwritten by |2 Nul, and leaves the last piece of the address 43 untouched. the canary value is not equal to its original and so the program exits:
![image](https://github.com/user-attachments/assets/2a3ae861-b191-45e1-83af-0e24f05f2bda)


Part 2: Bypassing the Stack Canary (40 pts)
Using the results of the previous part, your task in this part is to conduct a buffer overflow attack against the canary protected program. For this, you will need to understand the relationship between the input string provided to gets() and the location in the stack where the stack canary is located (followed by the return address). You will need to modify your input string to include the stack canary, which will most likely be at the location in the input string where you used to put the address of sekret_fn. Then you will place the address of sekret_fn at the new location where it is located offset from the start of the vulnerable buffer. You should be able to make your main() return to sekret_fn.
Include a copy of your input string in your README. Provide a screenshot of QtRVSim showing the values of the registers and the terminal window when you have successfully called sekret_fn.
Assuming the attacker has a copy of your program, is there any stack canary value that you could use that would prevent the attack from succeeding, or that would make the attack more challenging? Briefly explain your reasoning.

By entering the value AAAAAAAAAAAAAAAAAAAAYRNC|2, the canary has CNRY in it validating that there was apparently no overflow, and RA has |2 Nul Nul, which is the address of sekret_fn, which prints out the secret message, despite the input string being longer than the allocated stack space.
![image](https://github.com/user-attachments/assets/0a2da1d8-a264-4aac-b871-e690806c01c1)

It would be difficult to stop an attacker who has a copy of the program. Unless the canary was random every time. Otherwise the only other way is to use more obscure chars OR more nul values which do not have an ascii.





Part 3: Reporting and Logging (20 pts)
Create the following documents for your submission.

2.	Documentation of the versioning of your code as a separate pdf or doc file (call it logs.pdf or logs.doc). It is recommended that you use git source version control with a private repository. You may use any method available to you for hosting a private git repository, such as github, gitlab, or self-hosted.
For example if you use github, create a directory called {USERNAME}-project3/. Initialize the repository with your (working) project2.S file, renamed as project3.S. Use a descriptive commit message (e.g. “import previous files"). As you code, you should save your work frequently by committing significant iterations in this repository. By the time you finalize your source code, I would like you to take a screenshot of the commit logs in your repository.

Submission Instructions
Please submit into Canvas a single compressed file (zip or tar) named {USERNAME}-project3 that contains one directory that contains your readme.pdf, your source code, and your logs. Your USERNAME should just be your UCCS username (email address without the @uccs.edu part).

