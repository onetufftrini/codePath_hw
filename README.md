# Project 8 - Pentesting Live Targets

Time spent: **X** hours spent in total

> Objective: Identify vulnerabilities in three different versions of the Globitek website: blue, green, and red.

The six possible exploits are:

* Username Enumeration
* Insecure Direct Object Reference (IDOR)
* SQL Injection (SQLi)
* Cross-Site Scripting (XSS)
* Cross-Site Request Forgery (CSRF)
* Session Hijacking/Fixation

Each color is vulnerable to only 2 of the 6 possible exploits. First discover which color has the specific vulnerability, then write a short description of how to exploit it, and finally demonstrate it using screenshots compiled into a GIF.

## Blue

Vulnerability #1: __Session Hijacking/Fixation________________

Description:

<img src="blue-vuln1.gif">

Vulnerability #2: ___SQL Injection_______________

Description:

<img src="blue-vuln2.gif">

## Green

Vulnerability #1: _______Username Enumeration___________

Description:

<img src="green-vuln1.gif">

Vulnerability #2: ________Cross-Site Scripting (XSS)__________

Description:

<img src="green-vuln2.gif">


## Red

Vulnerability #1: ______Insecure Direct Object Reference (IDOR)____________

Description:

<img src="red-vuln1.gif">

Vulnerability #2: _______Cross-Site Request Forgery (CSRF)___________

Description:

<img src="red-vuln2.gif">


## Notes

Describe any challenges encountered while doing the work



Blue Exploit 1 - I performed session hijacking to verify this vulnerability. In one browser (firefox), I logged in normally but then copied the current PHPSESSIONID using a tool provided by Codepath. We then went to another browser (Chrome) which was NOT logged in and changed the PHPSESSIONID in this browser (using the same tool) to match the one we retrieved from the normally logged in browser. The result is that we were automatically logged into the system in the second browser without the need for a username or password.

Blue Exploit 2 - I performed a SQL injection at the public/staff/salespeople/show.php?id=5 endpoint. The 5 was replaced with the following SQL syntax: ' OR SLEEP(5)=0--' (which becomes %27%20OR%20SLEEP(5)=0--%27 when encoded in a URL). This snippet forced the SQL query (and thus our entire request) to pause for 5 seconds, thus indicating a successful injection.

Green Exploit 1 -  In the login screen of the green application, if the username is correct, but the password is wrong, the Log in was unsuccessful message is bolded. However, if the username does not exist, this message is NOT bolded. In the exploit, pperson, jmunroe99, and lbtables are the only valid usernames.

Green Exploit 2 - I input some Javascript in between a pair of <script> tags in the "Feedback" box of the "Contact Us" form. So that when an admin goes to his/her panel to check for any submitted forms, this Javascript is automatically executed.
  
Red Exploit 1 - An IDOR vulnerability exists where I can see any account, including those accounts that have been effectively deactivated. I used the public/salesperson.php?id=?? endpoint where ?? is the ID of the salesperson. In this exploit, I put in the ID (11) of "Lazy Lazyman," who was apparently fired for stealing. This attack is made even easier by the fact that the ID numbers of all salespeople can be guessed since they are low numbers and are in numerical order. Thus, I simply guessed ID numbers until they reach a deactivated account.

Red Exploit 2 -To perform a CSRF attack, we can trick an admin into clicking a link to a "blank" page that contains a hidden form. The page's source looks like the following:

<html>
  <head>
    <title>A Totally Blank Page</title>
  </head>
  <body onload="document.CSRF.submit()">
	<form action="https://xx.xxx.xxx.xx/red/public/staff/salespeople/edit.php?id=5" method="post" style="display: none;" name='CSRF' target="res">
	    <input type="text" name="first_name" value="Ken Barker" />
      	<input type="text" name="last_name" value="WAS HACKED" />
      	<input type="text" name="phone" value="555-352-9654" />
      	<input type="text" name="email" value="kbarker@HACKED.com" />
	</form>
    <iframe name="res" style="display: none;"></iframe>
  </body>
</html>

It contains all of the information necessary to modify Ken Barker's salesperson profile (id=5) and will be successfully submitted as soon as a logged in user loads the page.
