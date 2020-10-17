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
