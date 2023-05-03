Download Link: https://assignmentchef.com/product/solved-cs061-lab5-building-subroutines
<br>
Today you will learn the art of building subroutines, and become invincible.

<ul>

 <li><strong>Our Objectives for This Week </strong></li>

</ul>

<ol>

 <li>If you have not done so already, complete Lab 4, exercise 4</li>

</ol>

<em>(Go back to your your lab 4 directory, and follow the instructions in your lab 4 specs) <strong>You should have completed this step BEFORE the start of your lab 5 session) </strong></em>

<ol start="2">

 <li>Lab 4 review, &amp; intro to subroutines –  Exercise 01</li>

 <li>Roll your own subs –   Exercises 02 – 03</li>

 <li>Feel smug about it all — Exercise 04</li>

</ol>




<ol start="3">

 <li><strong> Subroutines: The Art of writing something <em>once</em></strong>​</li>

</ol>

Here is the basic structure of every subroutine you will ever write in LC3:

<em>;======================================================================= </em>

<em>; Subroutine: SUB_intelligent_name_goes_here_3200 </em>

<em>; Parameter (Register you are “passing” as a parameter): [description of parameter] </em>

<em>; Postcondition: [a short description of what the subroutine accomplishes] </em>

<em>; Return Value: [which register (if any) has a return value and what it means] </em>

<em>;=======================================================================  </em>

<em>.orig x3200       ; use the starting address as part of the sub name </em>

<em>;======================== </em>

<em>; Subroutine Instructions </em>

<em>;======================== </em>

<em>; (1) Backup R7 and any registers that this subroutine changes </em>​<strong><em><u>except for Return Values</u></em></strong> <em>; (2) Whatever algorithm this subroutine is intended to perform – only ONE task per sub!! </em>

<em>; (3) Restore the registers that you backed up </em>

<em>; (4) RET – i.e. return to where you came from </em>

<em>;======================== ; Subroutine Data </em>

<em>;======================== </em>

<em>BACKUP_R0_3200   .BLKW #1 ; Make one of these for each register that the subroutine </em><u>​</u><em><u>changes</u></em>

<em>BACKUP_R7_3200   .BLKW #1 ; … EXCEPT for Return Value(s) </em>

The header contains information you will need when reusing the subroutine later <em>(</em>​ <em>which you will be doing </em><u>​<strong><em>a lot</em></strong></u>​<em> from now on!)</em>​

<ul>

 <li><strong>.ORIG value:</strong></li>

</ul>

Each subroutine that you write needs to be placed somewhere specific in memory (just like our “main” code which we always locate at x3000).

A good convention to use is {x3200, x3400, x3600, …} for the {first, second, third, …} subroutine.

<ul>

 <li><strong>Subroutine name:</strong></li>

</ul>

Give the subroutine a good name and append the subroutine’s address to it to make it <em><u>unique</u></em><u>​ </u>​.  For example, if the subroutine “SUB_PRINT_ARRAY” starts at x3600, you should name the subroutine “SUB_PRINT_ARRAY_3600” to make it completely unique.

<ul>

 <li><strong>Parameters:</strong></li>

</ul>

Any parameters that you pass to the subroutine. This is a <em>little</em>​ ​ bit like passing params into a function in C++, except in assembly you pass them in via specific <em><u>registers</u></em><u>​ </u>​ rather than named <em><u>variables</u></em><u>​</u>.

<ul>

 <li><strong>Postcondition:</strong></li>

</ul>

What the subroutine actually does so you won’t have to guess later …

<ul>

 <li><strong>Return Value:</strong></li>

</ul>

The register(s) in which the subroutine returns its result (if any)—again, so you don’t have to try to guess later when you want to reuse the subroutine. Unlike C++ functions, you can return multiple values from a subroutine, one per register. If you are <em><u>really</u></em><u>​ </u>​ careful you can even return a value in a register that was used to pass in a parameter <em>(</em>​ <em>but not until you really know what you’re doing!!)</em>

Once your header is done, you can write your subroutine. This is a 4-step process:

<ol>

 <li><strong>Backing up registers: </strong></li>

</ol>

Have you run into the problem yet where you said, “awh heck… I ran out of registers to use!”?

Well, without this step, you would encounter that problem a ​<strong>lot</strong>​ more.

In this step, use ST to backup R7 and any other registers that this subroutine changes <u>​<em>except for</em></u><em> <u>registers used for passing in parameters and/or for returning values</u></em>​.

You <u>​<strong><em>must</em></strong></u>​ backup R7 because – as we will see below – R7 stores the address to return to after executing, and if you use any TRAPs inside your subroutine, they will use R7 for the same purpose themselves – and you will not be able to get back to where you came from!

<ol start="2">

 <li><strong>Write your subroutine code: </strong></li>

</ol>

Write whatever code is necessary to make the subroutine do its thing.

<u>IMPORTANT:​</u> all prompts &amp; error messages relating to the subroutine should be part of the subroutine – so, for instance, if a subroutine takes input from the user, the corresponding prompt must be in the subroutine data block, not in “main” where the subroutine is invoked.

<ol start="3">

 <li><strong>Restore registers:</strong></li>

</ol>

In this step, use LD to restore the registers that you backed up in step 1.

Remember to <u>​<em>always</em></u>​ backup/restore R7.

Remember to <u>​<em>never</em></u>​ backup/restore register(s) that contain your Return Value(s).

<ol start="4">

 <li><strong>Return: </strong></li>

</ol>

Use the RET instruction (alias for JMP R7) to return to where you came from. RET is a little bit like “return” in C


<strong><u>Reminder about Register Transfer Notation:</u> </strong>●      Rn = a register

<ul>

 <li>(Rn) = the contents of that register</li>

 <li>Mem[ some value ] = the contents of the memory address (expressed as a hex value) a &lt;– b  =   transfer (i.e. copy)  the ​<em><u>value</u></em>​ b to the <u>​<em>location</em></u>​ a.</li>

</ul>

○    R5 &lt;– (R4) means “copy the contents of Register 4 to Register 5, overwriting any previous contents of Register 5” – e.g.      ​ADD R5, R4, #0

○    Mem[ xA400 ] &lt;– Mem[ (R3) ]  means “obtain the value stored in R3 and treat it as a memory address; obtain the <em>value</em>​ stored ​<em>at that address</em>​; copy that ​<em><u>value</u></em><u>​</u> into memory at the <u>​<em>address</em></u>​ xA400.

<h1>The instruction you will need</h1>

<strong>JSR and JSRR</strong>​ ​<em>(two versions of the same instruction, differing only in their memory addressing modes) </em>




<u>JSR label</u> works just like BRnzp – i.e. it ​           <em><u>unconditionally</u></em>​           ​ transfers control to the instruction at label; and <u>​JSRR R6</u>​ works just like JMP – i.e. it transfers control to the instruction located at the address stored in R6, known as the ​<em>base register</em>​;

– ​<strong><em>with one very big difference:</em></strong>​ before transferring control with JSR or JSRR, the very first thing that happens is that the address of the next instruction – i.e. the return point – is stored in ​<strong>R7</strong>.​

This means that at the end of the subroutine, we can get back to where we jumped from with RET <strong>Example Code: Yay!!! </strong>

Following is an example that calls a really short subroutine that takes the 2’s complement of R1:

Next is a slightly longer program that has three different strings (each stored in its own array) and calls a subroutine that prints out the contents of an array – in other words, we call the same subroutine 3 times. The address of the string to print is passed to the subroutine in R1.

This time, the main code block is in a ​<em>different file</em>​ than the subroutine code. Each file has its own .ORIG and its own .END. You can load both files into the simpl simulator by typing the following on the command line:       ​simpl main.asm library.asm​<em>  (note that the main routine must be listed first) </em>

<strong><em>Note that if you use any other LC-3 emulator than simpl, you will probably have to use the technique of this second example, with a separate .asm file for each routine/subroutine, since the emulator may not be able to handle multiple .orig pseudo-ops in a single file as simpl does. </em></strong>

<strong>3.2 Exercises </strong>

<em><u>NOTE:</u></em><u>​</u><em> No “ghost writing”! That gets an instant 0! </em>

<h1>Exercise 01</h1>

Recall that exercise 04 from last week created an array of the first 10 powers of 2 {2​<sup>0​</sup>, 2​<sup>1​</sup>, 2​<sup>2​</sup>, …, 2​<sup>9​</sup>}, and then printed out their respective 16-bit binary representations (e.g. b0000 0000 0000 0001) one per line.

You did this by pasting the print code directly inside the loop.

Rework this exercise by converting the code that prints out R2 in 16-bit binary into a proper subroutine (as above, including proper headers), and simply invoking it from inside the loop.

<h1>Exercise 02</h1>

Write the ​<em><u>inverse</u></em>​ of a binary printing subroutine. That is, write a binary <u>​reading​</u> subroutine:

First, the subroutine will prompt the user to enter a 16-bit 2’s complement binary number: the user will enter ‘b’ followed by exactly sixteen 1’s and 0’s: e.g. “​b0001001000110100​” ​<em>(no spaces on </em><u>​<em>input</em></u>​<em>)</em>

Your subroutine should do the following:

<ol>

 <li>The user enters a binary number as a sequence of 17 ascii characters: b0010010001101000​</li>

 <li>The result of (1) is transformed into a single 16-bit value, which is stored in R2.</li>

</ol>

Your “main” can now invoke the subroutine from Exercise 01 to print the value of R2 back out to the console to check your work.

<strong>“Binary read” Algorithm: </strong>

<table width="493">

 <tbody>

  <tr>

   <td width="96">total</td>

   <td width="397">&lt;–        0</td>

  </tr>

  <tr>

   <td width="96">counter</td>

   <td width="397">&lt;–        16</td>

  </tr>

  <tr>

   <td width="96">R0</td>

   <td width="397">&lt;–  get input from user (the initial ‘b’) and do nothing with it</td>

  </tr>

 </tbody>

</table>

do

{

; that’s your job &#x1f642;

; HINT: the 4-bit binary number b1011 is (b101 * #2 + 1) } while ( counter &gt; 0 );

<h1>Exercise 03</h1>

Enhance exercise 2 so that it now performs some <u>​<em>input validation</em></u>​:

<ul>

 <li>If the first character entered is not ‘b’, the program should output an error message and go back to the beginning</li>

 <li>After that, if a SPACE is ever entered, the program should​ ​<u>only echo it</u>​ ​and continue (i.e. spaces are accepted but ignored in the conversion algorithm).</li>

 <li>If ​<em><u>any</u></em>​ character other than ‘1’, ‘0’ or SPACE is entered after the initial ‘b’, the program should output an error message and ask for a valid character – i.e. it should keep everything received so far, and keep looping until it gets a valid ‘0’, ‘1’ or space.</li>

</ul>

<h1>Exercise 04: Just read this</h1>

From now on, <strong><em>all</em></strong>​ ​ of your programs will consist of a simple test harness invoking one or more subroutines in which you will implement the assigned task – so make sure you have completely mastered the art of dividing your program up into these “self-contained” modules.

Each subroutine should perform a single task, and have CLEAR and EXPLICIT comments describing what it does, what input is required (and in which registers), and what will be returned (in which register).

A subroutine may invoke another subroutine – but only if you are <em>very, very</em>​          ​ careful!!  We will let you know when we want you to do this.

Make sure you understand and always employ basic “register hygiene” – i.e. backup and restore <strong><em>ONLY</em></strong>​ those registers that are modified by the subroutine for internal purposes only (and remember R7 if your subroutine invokes nested subroutines and/or TRAPS!)