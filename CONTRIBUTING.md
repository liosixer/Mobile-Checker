# Contributing to Mobile Checker

Looking to contribute to Mobile Checker? There is many ways to get involved in its developement. 
Please take few minutes to read this document to understand how you can help in Mobile Checker developement.

## Add a new check

It is pretty simple to add a new check to the mobile checker. Create a new check require only an experience in a client side JavaScript developement.

A new check is composed by 2 composants:
* the script itself, who will be executed on the tested web page.
* the issue template, sent to the user.
* testing.

### script
All check scripts are located in the [lib/checks](https://github.com/w3c/Mobile-Checker/tree/master/lib/checks) directory. You just have to add your own script file in the correct category. Of course, feel free to create a new category if no category match with your check. Ready? Let's create your first check!

This is the basic template of check file. Save it in the lib directory:

````javascript
var self = this;
exports.name = "name-of-the-check"; //write here the name of your check. Have to match with the file's name.
exports.category = "name-of-the-category"; //write here the name. Have to match with a category's directory name.
exports.check = function(checker, browser) {
	/*
	 * here your code.
	 */
};
```

The ```check(checker, browser)``` function, receive ```checker``` and ```browser``` as parameters.
* Use ```browser``` to test the web page.
* Use ```checker``` to report the result.

N.B: The ```browser``` object is based on the [mobile-web-browser-emulator API](https://github.com/w3c/mobile-web-browser-emulator) which is based on [selenium WebDriverJS API](https://code.google.com/p/selenium/wiki/WebDriverJs). That allow you to use all the functions provided by these APIs.


#### access to mobile-web-browser-emulator API: 
````javascript
exports.check = function(checker, browser) {
	// get a browser object which can be use with mobile-web-browser-emulator API
};
```
example: 
````javascript
exports.check = function(checker, browser) {
	browser.takeScreenshot(__dirname + "/../screenshot.png"); //mobile-web-browser-emulator method
};
```

#### access to selenium WebDriverJS API: 
````javascript
	browser.do(function(driver) { //take a function in parameter
		// get a driver object which can be use with selenium webdriver 
	}); 
```
example:
````javascript
exports.check = function(checker, browser) {
	browser.do(function(driver) {
		driver.getTitle().then(function(title) { //selenium webdriver method
			console.log(title);
		});
	});
};
```
N.B: WebDriverJS uses "promises" to track the state of each operation. [learn more](https://code.google.com/p/selenium/wiki/WebDriverJs#Promises).

#### run your own script:
````javascript
exports.check = function(checker, browser) {
	browser.do(function(driver) {
		return driver.executeScript(function() {
			//write your own script and return what you need.
        });
	});
};
```

#### report a result:
to report a result, use the report function of the checker object.
````javascript
checker.report(key, name, category, status, data)
```
* **key**: name of the file issue to display in the report
* **name**:	self.name
* **category**: self.category
* **status**: error, warning or info
* **data**: data to display in the issue reported (optional)


### issue template
All check issues are located in the [lib/issues](https://github.com/w3c/Mobile-Checker/tree/master/lib/issues) directory. You just have to add your own issue file in the correct path as ```category/name-of-your-check/your-file```. You can create all issues you need. That allow you to use more than once the ```checker.report(key, name, category, status, data)``` function in your script.

The Mobile Checker use [EJS](http://www.embeddedjs.com/) to write the issues sent to the client side. That allow you to insert data in the reported issue.

template:
````html
<div class="col-md-12 issue">
    <div class="col-md-12 content-issue">
    	<h2 class="title-issue page-header">
    		<span class="glyphicon glyphicon-exclamation-sign" aria-hidden="true"></span> 
    		<!-- title of your issue -->
    		<small><%=category%></small>
    	</h2>
        <p> <!-- write here what's going on --> </p>
        <p class='fixit'><strong>Fix it:</strong> <!-- explain here how to fix it --> </p>
    </div>
</div>
```
