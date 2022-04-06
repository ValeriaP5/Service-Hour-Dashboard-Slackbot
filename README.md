# Service-Hour-Dashboard-Slackbot
A slackbot which can display a volunteer's service hour record in Slack
//talk to the google form
//get the IDs of the form components
//https://docs.google.com/forms/d/17UTAsTuMKP8ivuUxcc9d75lO7DfQX0PUQmyX2t8o66s/edit
const FORM_ID = '17UTAsTuMKP8ivuUxcc9d75lO7DfQX0PUQmyX2t8o66s';

//Add custome menu to sheet
function onOpen(){ automate();}

function automate(){
  var ui =SpreadsheetApp.getUi().createMenu("Send Email");
  ui.addItem("Send Email", "getInfo");
  ui.addToUi();
}

function getFormIDs () {
  const form = FormApp.openById(FORM_ID);
  const formItems = form.getItems(); //array of form items

//loop over array
//print out form items

formItems.forEach(item => console.log(item.getTitle()+ ' ' + item.getId()));
}


function getInfo (){

//get the spreadsheet information
const ss = SpreadsheetApp.getActiveSpreadsheet();
const responseSheet = ss.getSheetByName ('Form Responses 1');
const data = responseSheet.getDataRange().getValues();
//console.log(data);

//remove the header
data.shift();
console.log(data);

//loop over the rows
data.forEach((item,i) => {

//Identy ones I haven´t reply to
if(item[6] === '') {

//get the email address
const email = item[1];
console.log(email);
const name = item[2]
console.log(name);

//write the email
const subject = 'Thank you for responding to the Apps Script Questionnaire ' + name;
let body = 'Here is your dashboard of weekly service hours: (dashboard will come here)' ;
console.log(body)

//send email
GmailApp.sendEmail(email,subject,body);

}

} );

}
