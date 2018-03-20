LSE Challenge Link: https://ctf.lse.epita.fr/ex/55/

This challenge was relatively easy to complete but provided a good review of the fundamentals. 

To start off we check the file headers on the binary,

![header](header.PNG)

We can see that the file isn't stripped which makes this challenge less cumbersome. 
Next we check the properties of the binary. 

![rabin2](rabin2.PNG)

Great, no NX or ALSR. Now that we have an idea of the challenge, lets run the binary.

![password](password.PNG)

Ah we need a username,well before we dive into the assembly, lets see if we can get the username and password
an easier way. By running strings. 

![strings](strings.PNG)

Huh, admin and T6OBSh2i look interesting. Let's try those.

![login](login.PNG)

Great we are in. Lets see what this binary can do.

![choice1](choice1.PNG)

![choice2](choice2.PNG)

Hmmmm, seems like we can't execute commands. We should definently check that in the binary later. 
But first, since this challenge is only 50 points. Lets see if it has any common vunerablities. 

![overflow](overflow.PNG)

Awesome, now we have a direction for this challenge. The input has a buffer overflow vunerablity and we can 
redirect it to a function in choice1 to execute our command. So let's go into the binary to do that. 






