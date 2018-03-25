
This challenge was part of the HareKaze 2018 CTF. It is a little tricky and I didn't manage to solve it during the CTF
but I figured it out afterwards. 

To start with we are given a binary file, so let us run the basic commands on it. 

Here is the output of file

![header](header.PNG)

and the output of rabin2

![properties](properties.PNG)

Hmm nothing looks strange so far. Lets run the binary and see if we can get more information. 

![testtry](testtry.PNG)

As you can see, entering random words doesn't do anything and it doesn't seem to react to a buffer overflow. At this point
I realize that the binary will give me more information but before we start debugging let's try and run strings to see if
that will provide some answers. 

![strings](strings.PNG)

Hmm, looks like some animals are listed and their associated sounds are produced. Isoroku looks interesting as it's sound hints to
the flag. Let's try some of these strings. 

![isoroku](isoroku.PNG)

Damn, I was hoping it would be this easy. Ah well, looks like we have to dive into the binary to get more information. 
Loading the binary up in Radare2 and using the command 'VV' to enter visualization mode, we can start to understand the program.

Here I have shown the start of the main program. 

![three_animals](three_animals.PNG)

Well it looks like three animals have to be added before we can go to the parade part where the animal noises are printed as 
shown with the jump less than or equal to and the comparison to 2.  

Here is the animal noises being printed. As you can see, entering isoroku will output the flag. 

![flag_isoroku](flag_isoroku.PNG)

Now we gotta figure out why it isn't printing the flag. Let's see what happens when we are inputting the animals.

![input](input.PNG)

Hmm this answer's some of our questions. First of all, the read_chk is a secure read that protects against buffer overflows 
which explains why our buffer overflow didn't do anything to our program. Furthermore we see that it is using string compare
(strcmp) to compare our strings against strings with animals values such as "cow". 

![noisoroku](noisoroku.PNG)

Scrolling down further into the program, we see that there is no comparison for "isoroku" and if the string we entered doesn't match one of the values then the program will ignore it. Seems like in order to solve this issue we need to take advantage 
of a strcmp quality, mainly that the comparision will stop reading after it encounters a null byte. We might be able to 
append isoroku onto a legit value and the program will add it to stack and then when the parade begins, the flag will be read.

Right so, so far we have an idea of our payload looking like this:

payload = "hen" + "\x00" + "isoroku"















