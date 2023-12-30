# Cucumber Basic
This is an sample project to demonstrate how to work with Selenium and cucumber for Java

:golf:There are still two branches with Cucumber 5 integration and Cucumber selenium with jenkins & Extent reporting also.


![alt text](https://github.com/venkywarriors619/Cucumber/blob/cucumberbasic/cucumber.jpg ":mag_right: Keep Exploring :mag:")

### Difference between dry run and strict in cucumber
<strong>Strict: </strong>if strict option is set to false then at execution time if cucumber encounters any undefined/pending steps then cucumber does not fail the execution and undefined steps are skipped and BUILD is SUCCESSFUL 
<br>and if Strict option is set to true then at execution time if cucumber encounters any undefined/pending steps then cucumber does fails the execution and undefined steps are marked as fail <br>
<strong>dryRun : </strong>It is used to verify that all steps of the feature file defined on step generator or glue code file or not. Syntax is : dryRun= true
One thing keep in mind that when dryRun=true then entire code should not run only it checks that all the methods matched with feature file or not.
<br> So in case any of the function is missed in the Step Definition for any Step in Feature File, it will give us the message. So If you writing scenarios first and then implementing step definitions then add dryRun = true.
### Data Tables in Cucumber
In this example, we will pass the test data using the data table and handle it using Raw() method.
```
Scenario: Successful Login with Valid Credentials
 Given User is on Home Page
 When User Navigate to LogIn Page
 And User enters Credentials to LogIn
    | testuser_1 | Test@153 |
 Then Message displayed Login Successfully
```
The implementation of the above step will belike this:
```
@When("^User enters Credentials to LogIn$")
 public void user_enters_testuser__and_Test(DataTable usercredentials) throws Throwable {
 
 //Write the code to handle Data Table
 List<List<String>> data = usercredentials.raw();
 
 //This is to get the first data of the set (First Row + First Column)
 driver.findElement(By.id("log")).sendKeys(data.get(0).get(0)); 
 
 //This is to get the first data of the set (First Row + Second Column)
     driver.findElement(By.id("pwd")).sendKeys(data.get(0).get(1));
 
     driver.findElement(By.id("login")).click();
 }
```
#### Maps in Data Tables
```
Scenario: Successful Login with Valid Credentials
 Given User is on Home Page
 When User Navigate to LogIn Page
 And User enters Credentials to LogIn
 | Username   | Password |
    | testuser_1 | Test@153 |
    | testuser_2 | Test@154 |
 Then Message displayed Login Successfully
```
```
@When("^User enters Credentials to LogIn$")
 public void user_enters_testuser_and_Test(DataTable usercredentials) throws Throwable {
 
 //Write the code to handle Data Table
 for (Map<String, String> data : usercredentials.asMaps(String.class, String.class)) {
 driver.findElement(By.id("log")).sendKeys(data.get("Username")); 
     driver.findElement(By.id("pwd")).sendKeys(data.get("Password"));
     driver.findElement(By.id("login")).click();
 }
 
 }
```
### Parameterization without Example Keyword
```
Feature: Login Action
 
Scenario: Successful Login with Valid Credentials
 Given User is on Home Page
 When User Navigate to LogIn Page
 And User enters "testuser_1" and "Test@123"
 Then Message displayed Login Successfully
```
```
 @When("^User enters \"(.*)\" and \"(.*)\"$")
 public void user_enters_UserName_and_Password(String username, String password) throws Throwable {
 driver.findElement(By.id("log")).sendKeys(username); 
     driver.findElement(By.id("pwd")).sendKeys(password);
     driver.findElement(By.id("login")).click();
 }
```
### Data-Driven Testing Using Examples Keyword
```
Feature: Login Action
 
Scenario Outline: Successful Login with Valid Credentials
 Given User is on Home Page
 When User Navigate to LogIn Page
 And User enters "<username>" and "<password>"
 Then Message displayed Login Successfully
Examples:
    | username   | password |
    | testuser_1 | Test@153 |
    | testuser_2 | Test@153 |
```
```
@When("^User enters \"(.*)\" and \"(.*)\"$")
 public void user_enters_UserName_and_Password(String username, String password) throws Throwable {
 driver.findElement(By.id("log")).sendKeys(username); 
     driver.findElement(By.id("pwd")).sendKeys(password);
 }
```
### TestNG with Cucumber
CucumberRunner class extends AbstractTestNGCucumberTests and CucumberRunner class is specified in testNG.xml file.
<br><a href="https://www.lambdatest.com/blog/automation-testing-with-selenium-cucumber-testng/">Cucumber basics</a>
<br><a href="http://www.amitrawat.tech/post/cucumber-jvm-with-testng/">Integrating cucumber-JVM with TestNG</a>
```
@CucumberOptions(
        format={"pretty","json:path/to/json_repot.json"},
        features = "Path_to_features_file",
        glue="com.sri.stepDefinition",
        tags={"@smoke,@regression")
        )

public class TestRunner extends AbstractTestNGCucumberTests{}
```
### Cucumber Extent Report
<a href="https://www.toolsqa.com/selenium-cucumber-framework/cucumber-extent-report/">Cucumber extent report</a>
<br><a href="https://medium.com/@praveendavidmathew/creating-cucumber-extent-report-the-right-way-3298a247e545">Cucumber with Extent report</a>
### How To Get Feature Name And Scenario Name Using Cucumber-JVM
```
Scenario scenario
"Feature Name : " +  scenario.getId().split(";")[0]
"Scenario Name : " + scenario.getName()
```
### Running Cucumber tests in groups using TestNG XML and Cucumber tags 
<a href="https://www.toolsqa.com/cucumber/cucumber-tags/">Cucumber tags</a><br>
Tag the scenarios in your feature files with the desired tags. For example
```
@smoke
Scenario: Verify login functionality
  Given user navigates to the login page
  When user enters valid credentials
  Then user should be logged in successfully
```
Create a TestNG XML file where you can specify the tags you want to run. Here's an example
```
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" >
<suite name="CucumberSuite" parallel="methods" thread-count="2">

    <test name="SmokeTest">
        <classes>
            <class name="path.to.TestNGCucumberRunner">
                <methods>
                    <include name="^.*@smoke.*$"/>
                </methods>
            </class>
        </classes>
    </test>

</suite>
```
### How to run specific feaure file in cucumber via testng.xml file
Tag your feature files with unique identifiers. For example, you can use the @Feature tag in your feature files:
```
@FeatureLogin
Scenario: Verify login functionality
  Given user navigates to the login page
  When user enters valid credentials
  Then user should be logged in successfully

```
```
<!DOCTYPE suite SYSTEM "http://testng.org/testng-1.0.dtd" >
<suite name="CucumberSuite" parallel="methods" thread-count="2">

    <test name="LoginFeature">
        <classes>
            <class name="path.to.TestNGCucumberRunner">
                <methods>
                    <include name="Login.feature"/>
                </methods>
            </class>
        </classes>
    </test>

</suite>
```
