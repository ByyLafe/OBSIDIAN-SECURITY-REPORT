# Localisation : https://obsidian-website-seven.vercel.app/ 

## Flag : {3V3RYTH1NG_W4S_PL4N} 

Description: The javascript source code being obfuscated, we can at first think that we cannot easily find any flaws.  

Outside, a fatal error was made during development.  

Achievement:  

    Using a scrapping tool, to have a local text file 

and complete with the site, not having to move every part of the code, or possibly losing it in it. And also keep a local copy,  

of it, in case its developers patch the source code, or if the site is deleted.  

    Analysis of the code. Even encoded, some patterns emerge, notably qNNX. 

Which seems to give access to the other values: qNNX.GNNS(9)&&cSkE===qNNX.CKfT(10)  

PoC:  

Open the development tools (generally via F12) on the login page.  

From the Console tab, enter the following commands to query the global object:  

console.log("Expected login: " + qNNX.GNNS(9));  

console.log("Expected password: " + qNNX.CKfT(10));  

The system returns immediately:  

Login: Jordan  

Password: cherry  

Using these credentials allows you to bypass the security barrier and display the flag.  

We could also have found the password and the identifier at the website via the email present in the hard binary.  

Via a script, for example, that tests different possibilities, but it remains less reliable.  

Patch:  

Never trust the client.  

Even if the data is encrypted, if it is decoded on the client side, this encryption was useless because it is possible to display the result of the calculation.  

The credentials must never be present in the source code, even if they are encrypted. 