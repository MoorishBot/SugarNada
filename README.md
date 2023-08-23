# SugarNada
Introducing a groundbreaking project that combines cutting-edge technology to enhance health monitoring and improve daily lives. My project focuses on seamlessly integrating glucose level monitoring with the power of Amazon's Alexa, creating a comprehensive solution for individuals managing diabetes. Leveraging the capabilities of the Continuous Glucose Monitor (CGM) and Amazon Echo Dot/Alexa (All types), I have developed a system that continuously reads glucose levels and provides timely notifications through Alexa when readings fall below a specified threshold.
The true magic of my project unfolds with the integration of Amazon's Alexa. I do not need to use voice commands, to query Alexa for the current glucose levels, in fact, Alexa proactively notifies me, offering a gentle reminder to take necessary actions, such as consuming a snack or checking blood sugar more thoroughly, (Alexa message is 100% customizable),
This project redefines the way health technologies collaborate, fostering a more connected and responsive approach to personal well-being.

# What is needed?

1-	Google Sheet (Free Google Apps)

2-	Free Sugarmate.io account (Free)

3-	 VoiceMonkey.io $2.99/M

4-	Any Amazon's Echo Device will work.

# How to:
# SugarMate
- Create a free account at https://www.sugarmate.io/
- write down the user name and the password, we will need them for Google Apps to make the API calls.
- Now Let's open Chrome, and login into Sugarmate.io, 
How to Add a Script to Google Sheets
1. Open your Google Sheets workbook
2. Go to Extensions > Apps Script
![image](https://github.com/MoorishBot/SugarNada/assets/89058912/7c097bf2-b46c-4912-a6e5-6accccd0db86)
3. Apps Script is a tool created by Google that allows you to run code scripts in your spreadsheet.There are several things we can use Apps Script for such as creating new functions, adding custom menus or sidebars, building add-ons, integrating with other Google Workspace applications and a lot more.
 ![image](https://github.com/MoorishBot/SugarNada/assets/89058912/9a4eb1ee-ed6b-44a7-b580-3b1789cf9912)
   
4. In the Script Editor, paste the script below

function SugarMate() {

// Create a request options object.

var requestOptions = {

method: 'POST',

redirect: 'follow'

};

// Make the request to the SugarMate API.

var response = UrlFetchApp.fetch("https://sugarmate.io/oauth/web?email=sugarmateuserID&password=sugarmatePassword", requestOptions);

// Get the response text.

var result = response.getContentText();

// Print the result to the console.

//Logger.log(result);

var responseJSON = JSON.parse(response.getContentText());

var accessToken = responseJSON["access_token"];

//Logger.log(accessToken);

 var date = new Date();
 
var nonce = Math.floor((date.getTime()/1000)).toString();

var url = 'https://sugarmate.io/api/v2/accounts/PUT_ACCOUNT_ID_HERE/readings?feed_last_updated=' + nonce;

var headers = {

'Authorization': 'Bearer ' + accessToken

};

var response = UrlFetchApp.fetch(url, {

'method': 'GET',

'headers': headers

});

if (response.getResponseCode() == 200) {

var readings = JSON.parse(response.getContentText());

var responseJSON = JSON.parse(response.getContentText());

const length = responseJSON.readings.length;

Logger.log(length);

var val = responseJSON['readings'][length-1]['val'];

Logger.log(val);

if (val < 70) 

{

var urlWEB = 'VOICE_MONKEY_WEBHOOK_HERE';

var response = UrlFetchApp.fetch(urlWEB, {'method': 'POST'});

}  else {

    // Do nothing
    
}

Logger.log(responseJSON);

Logger.log(nonce);

}}





