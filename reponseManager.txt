/*
Author: BAL KRISHN SINGH
Created: 15th March 2018
exection steps: (1) open command prompt (2) goto directory where file "reponseManager.js" exists (3)run "node reponseManager.js"

*/

 /* display response */
const answerArray=[
		{'1' : 'My name is bob'},
		{'2' : 'I am here to help you with your work related queries'},
		{'3' : 'To apply leave you need to login to your HR portal, click "apply Leave" and follow the instructions'},
		{'4' : 'You can claim your bills by contacting your accountant'},
		{'5' : 'Assignment of work will be done everyday by 11am.'}  ,
		{'defualtAnswer': 'Sorry I can not answer you at the moment.'}
	];
 /* display response */
const slots={
		 action:["do", "help", "apply", "cliam", "assign"],
		 subject:["name", "work", "leave", "bill", "time"]
	};
 /* display response */
const answerMap = [
		{ key:['name'], value:1},		
		{ key:['do'], value:2},
		{ key:['help'], value:2},
		{ key:['work'], value:2},			
		{ key:['cliam','bill'], value:4},	
		{ key:['apply','leave'], value:3},		
		{ key:['assign','work'], value:5},
		{ key:['assign','work','time'], value:5},       //  it element will never match
	];
	
/* find if an array include an element */
function include(arr, searchKey) {	
	return arr?(arr.indexOf(searchKey) != -1): false;
}

/* check which answerMap element is matching with input slots first */
function checkAnswerKey(anAnswerMap){
	var flag = true;	
	if(!anAnswerMap || !anAnswerMap.key || (!inputSlots.action && (!inputSlots.subject || !inputSlots.subject.length) ) || ( inputSlots.subject && typeof inputSlots.subject !== 'object'))
		return false;

	if(flag && inputSlots.action)
		flag = flag && include(slots.action, inputSlots.action) && include(anAnswerMap.key, inputSlots.action);

	if(flag && inputSlots.subject && inputSlots.subject.length)
		flag = flag && inputSlots.subject.every((aKey) => 	!aKey || (include(slots.subject, aKey) &&  include(anAnswerMap.key, aKey)));

	if(flag)	
			flag = flag && anAnswerMap.key.every((aKey) => { return aKey == inputSlots.action ||  include(inputSlots.subject, aKey)}); 
		
	return flag;
}


function getResponse(){
	 /* find the first matching element in answerMap */
	 var answerKey = answerMap.find(checkAnswerKey);
	 
	 /* find the response */
	 var output = answerKey? answerArray[answerArray.findIndex(x=> Object.keys(x) == answerKey.value.toString())][answerMap.find(checkAnswerKey).value.toString()]  :answerArray[5]['defualtAnswer'];
	  /* display response */
	 console.log("Response ::",output); 
 }
 
/* give input slots here */
 var inputSlots = { "subject":["work", "time"], "action" : "assign"};
 
 /* give hint to user inputSlots.subject  should be an array. */
 if( inputSlots.subject && typeof inputSlots.subject !== 'object')
	console.log("inputSlots.subject should be an array.");

getResponse();
