---
layout: post
title:  "Setting up the Gmail API and XOAUTH2 with PHPMailer"
date:   2017-07-14 16:44:50 +0000
categories: PHP PHPMailer SMTP GmailAPI XOAUTH2
img: "/assets/gmailapi.jpg"
---

After getting into a pickle trying to set up the Gmail API with PHPMailer AGAIN I thought I’d better document the process, just in case there’s a next time.

First of all, you need to configure the OAuth2 app in your Google account by taking the following steps.


Log in to your Google account and head to the [Developer Console.](https://console.developers.google.com/ "Google Developer Console")


Click on the Select a project dropdown.

![Google Developer Console GoogleAPIs](assets/gmailapi1.jpg)


Click the + button on the pop up to create a new project.

![GoogleAPIs navigate to New Project page](assets/gmailapi2.jpg)


Give the project a name (like 'My Website Contact Form') and click create.

![GoogleAPIs name and create a new project](assets/gmailapi3.jpg)


When the project is created you are redirected to the API Manager Dashboard. Click ENABLE API.

![GoogleAPIs navigate to API library](assets/gmailapi4.jpg)


On the API library page, search for or click on the Gmail API link.

![GoogleAPIs choose Gmail API in API Library](assets/gmailapi5.jpg)


On the Gmail API page, click ENABLE.

![GoogleAPIs enable Gmail API](assets/gmailapi6.jpg)


Click on the Credentials Menu option on the left, NOT on the Create Credentials button that appears after you've enabled the API.


Click on the OAuth consent screen tab, choose the email address you'll use to authenticate and then give your product a name and save.

![GoogleAPIs OAuth consent screen](assets/gmailapi7.jpg)


Click on the Credentials tab, then click the Create Credentials button and choose OAuth Client ID from the drop down menu.

![GoogleAPIs credentials](assets/gmailapi8.jpg)


On the Create client ID page, choose the Web application option, give your app a name, give the path to PHPMailer's get_oauth_token.php file in your project directory as an Authorized redirect URI and then click on the Create button.

![GoogleAPIs create client ID](assets/gmailapi9.jpg)


When your credentials are generated, a pop up with your client ID and client secret will appear. You can also find these later by clicking the pencil icon beside your project listing on the Credentials page.

![GoogleAPIs OAuth 2.0 client ID credentials](assets/gmailapi10.jpg)


Now you need to configure the PHPMailer files.


In your project files, update the values in phpmailer/get_oauth_token.php with those from your API credentials: $redirectUri, $clientId and $clientSecret. (Yes, $redirectUri does point back to the same file you're editing).

![PHPMailer file update with Gmail API credentials](assets/gmailapi11.jpg)


NOTE: You may need to download additional packages through Composer so that the necessary classes are available to PHPMailer. Additionally, PHP >=5.6.0 is required. In Terminal, cd into the project root then run 
* php composer.phar require league/oauth2-client
* php composer.phar require league/oauth2-google


Now you need to fetch the Refresh Token.


Paste the path to the phpmailer/get_oauth_token.php into your browser and hit return. 


If you have more then one Google ID, you'll now be asked to select which one you'd like to use.


You'll then be asked to give permission for your app to access your Gmail account.


Once you click the Accept button, you'll be redirected to a page with your Refresh Token. This is the value you need to add to your PHPMailer script in order to perform XOAUTH2 authentication when connecting to Gmail's SMTP server.

![Set PHPMailer object variables with GMail API values](assets/gmailapi12.jpg)


NOTE: You may have to join the Google group 'Allow Risky Access Permissions by Unreviewed Apps', or fill out the OAuth Developer Verification Form, in order to use the API (and get the $oauthRefreshToken attribute needed as above).



