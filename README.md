# DUCTF-2022-Writeups

## Solves

### source provided 	
Category: rev 

Points: 50

In this challenge we were provided with an assembly file which i essentially translated to python in order to get the flag. 


### Legit App Not Ransomware 
Category: rev

Points: 50

In thi challenge we were provided with a .exe executable which I binwalked for a .NET executable which I then decompiled with dnSpy to find the flag.  


### Clicky

Category: rev

Points: 50

This was very similar in concept to legit app not ransomware with just a little bit of extra reading and decoding required. 

Writeup: https://github.com/etselect/DUCTF-2022-Writeups/blob/main/Clicky.md


### Land Down Under 	

Category: misc

Points: 329 

This challenge was written in aussie++ (which is written in rust). Most of the challenge was essentially turning the code upsid down so you could work with it. From 

there was a variable simply called "i_should_print_the_flag" which you can print with the function "GIMME". This prints the flag in base32 and lower case which was 

heavily hinted at by the code which prints  "Sixty four? So we divide that by two..". 


### login

Category: pwn

Points: 289

This was a funky heap overflow which I solved with gef and pwntools. Full writeup coming. 


### Slash Flag 	

Category: misc

Points: 471

This challenge was based around a storage bot in the DUCTF discord server. It pointed you to it's source code so it mostly came down to reading it thourougly. 
in the source code on GitHub you can look at how the bot processes commands and how it decides who an organiser is. I invited it to a server and made myself an 
organiser. The file name in the bot's create function didn’t get put through quote so you could use a little command expansion but it would make it all caps so you 
couldn’t use letters or it wouldn't be treated as a command. But it doesn’t change special characters or numbers so you can use octal and create a file named cat 
/flag/flag.txt > F or `$'\143\141\164' $'\57\146\154\141\147\57\146\154\141\147\56\164\170\164' > F`,  then open the F with the bot. 
