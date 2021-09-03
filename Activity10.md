Practical Activity - Enumerating Web Application vulnerabilities

 Here is the link TryHackMe | OWASP Juice Shop
This room uses the Juice Shop vulnerable web application to learn how to identify and exploit common web application vulnerabilities.
OWASP Juice Shop is probably the most modern and sophisticated insecure web application! … Juice Shop encompasses vulnerabilities from the entire OWASP Top Ten along with many other security flaws found in real-world applications!
Resources Used
•	Kali Linux
•	Burp Suite (Community Edition)
•	OWASP Juice Shop
Question. Come up with a well explained walkthrough report of the room and attach the screenshots with answers and well description of how you have achieved it.
# Task 1   Open for business!
i.	Deploy the VM attached to this task to get started! You can access this machine by using your browser-based machine, or if you're connected through OpenVPN
 
ii.	Once the machine has loaded, access it by copying and pasting its IP into your browser; if you're using the browser-based machine, paste the machines IP into a browser on that machine.
 
For this task, I started with setting up my machine. I was not able to totally use my attack box because of the limited time. To do that I installed the owasp juice shop on my local machine.
Steps to install owasp juice shop from terminal
-	sudo apt-get install nodejs
-	sudo apt-get install npm
-	git clone https://github.com/bkimminich/juice-shop.git
-	cd juice-shop/
-	npm install
-	npm start

And we see its running on localhost:3000
To configure proxy
-	From the right side of the browser, click on Add on
-	Search for “Foxy Proxy” and download it
-	Click on import proxy lists and type in ” 127.0.0.1:8080” and click on import setting.
-	Go to Preferences
-	Navigate to Network settings and select Manual proxy configuration
-	Enter “127.0.0.1” as HTTP proxy and “8080” as the port and click on OK
-	Start burpsuite.

# Task 2  Let's go on an adventure!
i.	What's the Administrator's email address?
       Answer: Administrator’s email address is admin@juice-sh.op
The reviews show each user's email address. Which, by clicking on the Apple Juice product, shows us the Admin email!
        
ii.	 What parameter is used for searching? 
-	Click on the magnifying glass in the top right of the application will pop out a search bar.

-	We can then input some text and by pressing Enter will search for the text which was just inputted.

-	Now pay attention to the URL which will now update with the text we just entered.

-	We can now see the search parameter after the /#/search? the letter q
         
iii.	 What show does Jim reference in his review? 
Answer: In jim’s review, the show which he reference was Star Trek

-	Jim did a review on the Green Smoothie product. We can see that he mentions a replicator.

-	If we google "replicator" we will get the results indicating that it is from a TV show called Star Trek

     

 
# Task 3  Inject the juice
i.	Log into the administrator account!
Answer: #flag-32a5e0f21372bcc1000a6088b93b458e41f0e02a
-	After we navigate to the login page, enter some data into the email and password fields.

-	Before clicking submit, make sure Intercept mode is on.This will allow us to see the data been sent to the server!

-	We will now change the "a" next to the email to: ' or 1=1--and forward it to the server.
-	
 
 
 
 
 


ii.	 Log into the Bender account!
Answer: #flag-fb364762a3c102b2db932069c0e6b78e738d4066
-	Similar to what we did in Question #1, we will now log into Bender's account! Capture the login request again, but this time we will put: bender@juice-sh.op'-- as the email. 
 
 
 
# Task 4  Who broke my lock?!
i.	 Bruteforce the Administrator account's password!
Answer: c2110d06dc6f81c67cd8099ff0ba601241f1ac0e
-	We have used SQL Injection to log into the Administrator account but we still don't know the password. Let's try a brute-force attack! We will once again capture a login request, but instead of sending it through the proxy, we will send it to Intruder.

-	Go to Positions and then select the Clear § button. In the password field place two § inside the quotes. To clarify, the § § is not two sperate inputs but rather Burp's implementation of quotations e.g. "". The request should look like the image below. 

-	For the payload, we will be using the best1050.txt from Seclists. (Which can be installed via: apt-get install seclists


-	You can load the list from /usr/share/seclists/Passwords/Common-Credentials/best1050.txt
-	A failed request will receive a 401 Unauthorized 
-	Whereas a successful request will return a 200 OK.  





    
ii.	Reset Jim's password!
Answer: 094fbc9b48e525150ba97d05b942bbf114987257
-	Looking through the wiki page we find that he has a brother.
 
-	Looks like his brother's middle name is Samuel

-	Inputting that into the Forgot Password page allows you to successfully change his password.You can change it to anything you want!
 
    
# Task 5 AH! Don't look!
i.	 Access the Confidential Document!
Answer: edf9281222395a1c5fee9b89e32175f1ccf50c5b
-	Navigate to the About Us page, and hover over the "Check out our terms of use".
-	You will see that it links to  http://localhost:3000/ftp/legal.md. Navigating to that /ftp/ directory reveals that it is exposed to the public!
-	
-	We will download the acquisitions.md and save it. It looks like there are other files of interest here as well.

-	After downloading it, navigate to the home page to receive the flag!

 
ii.	 Log into MC SafeSearch's account!
Answer: 66bdcffad9e698fd534003fbb3cc7e2b7b55d7f0
-	After watching the video there are certain parts of the song that stand out.

-	He notes that his password is "Mr. Noodles" but he has replaced some "vowels into zeros", meaning that he just replaced the o's into 0's

-	We now know the password to the mc.safesearch@juice-sh.op account is "Mr. N00dles"
   
iii.	Download the Backup file!
Answer: bfc1e6b4a16579e85e06fee4c36ff8c02fb13795
-	We will now go back to the  http://localhost:3000/ftp/ folder and try to download package.json.bak. But it seems we are met with a 403 which says that only .md and .pdf files can be downloaded. 

-	To get around this, we will use a character bypass called "Poison Null Byte". A Poison Null Byte looks like this: %00. 
-	Note that we can download it using the url, so we will encode this into a url encoded format.

-	The Poison Null Byte will now look like this: %2500. Adding this and then a .md to the end will bypass the 403 error!
   
# Task6 Who's flying this thing?
i.	Access the administration page!
Answer: 946a799363226a24822008503f5d1324536629a0
-	First, we are going to open the Debugger on Firefox. This can be done by navigating to it in the Web Developers menu. 

-	We are then going to refresh the page and look for a javascript file for main-es2015.js

-	We will then go to that page at: localhost:3000/main-es2015.js

-	To get this into a format we can read, click the { } button at the bottom

-	Now search for the term "admin" 

-	You will come across a couple of different words containing "admin" but the one we are looking for is "path: administration"


-	This hints towards a page called "/#/administration" as can be seen by the about path a couple lines below, but going there while not logged in doesn't work. 

-	As this is an Administrator page, it makes sense that we need to be in the Admin account in order to view it.

-	A good way to stop users from accessing this is to only load parts of the application that need to be used by them. This stops sensitive information such as an admin page from been leaked or viewed.
   
ii.	View another user's shopping basket!
Answer: 41b997a36cc33fbe4f0ba018474e19ae5ce52121
-	Login to the Admin account and click on 'Your Basket'. Make sure Burp is running so you can capture the request!

-	Forward each request until you see: GET /rest/basket/1 HTTP/1.1

-	Now, we are going to change the number 1 after /basket/ to 2
-It will now show you the basket of UserID 2. You can do this for other UserIDs as well, provided that they have one!
   
iii.	Remove all 5-star reviews!
Answer: 50c97bcce0b895e446d61c83a21df371ac2266ef
-	Navigate to the  localhost:/#/administration page again and click the bin icon next to the review with 5 stars!

 
# Task 7  Where did that come from?
i.	Perform a DOM XSS!
Answer: 9aaf4bbea5c30d00a1f5bbcfce4db6d4b0efe0bf
-	We will be using the iframe element with a javascript alert tag: <iframe src="javascript:alert(`xss`)"> 

-	Inputting this into the search bar will trigger the alert.

 
ii.	Perform a persistent XSS!
Answer: 149aa8ce13d7a4a8a931472308e269c94dc5f156
-	First, login to the admin account.

-	We are going to navigate to the "Last Login IP" page for this attack.


-	It should say the last IP Address is 0.0.0.0

-	Make sure that Burp intercept is on, so it will catch the logout request.

-	We will then head over to the Headers tab where we will add a new header:

-	Name: True-Client-I

-	Value: <iframe src="javascript:alert(`xss`)">

-	Then forward the request to the server!

-	When signing back into the admin account and navigating to the Last Login IP page again, we will see the XSS alert!
	

   
iii.	Perform a reflected XSS!
Answer: 23cefee1527bde039295b2616eeb29e1edc660a0
-	First, we are going to need to be on the right page to perform the reflected XSS!

-	Login into the admin account and navigate to the 'Order History' page. 

-	From there you will see a "Truck" icon, clicking on that will bring you to the track result page. You will also see that there is an id paired with the order.   

-	We will use the iframe XSS, <iframe src="javascript:alert(`xss`)">, in the place of the 5267-f73dcd000abcc353

-	After submitting the URL, refresh the page and you will then get an alert saying XSS!

  

# Task 8
i.	Access the /#/score-board/ page
Answer: 7efd3174f9dd5baa03a7882027f2824d2f72d86e
To access this page, just type the URL: localhost:3000/#/score-board
 
