# Project
Project Work Repository
package Account;

import org.junit.Assert;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.testng.annotations.Test;
import config.PropertiesFile;

public class LoginPage extends PropertiesFile 
{	
	public static void login(WebDriver driver,String username,String password,boolean rememberMe) throws InterruptedException
	   {    
		 driver.findElement(By.xpath("//input[@id='email_input']")).sendKeys(username);
	     driver.findElement(By.xpath("//input[@id='password_input']")).sendKeys(password);
	     driver.findElement(By.xpath("//button[contains(text(),'Login')]")).click();
	     Thread.sleep(5000);
	   }
	
	@Test(dataProvider="LoginData")
	public void TestLogin(String browser) throws InterruptedException 
	{	
		String username = "roohi@envirya.in";
    	String password = "Yaali@1421";
    	boolean rememberMe = false;
		WebDriver driver = createBrowser(browser);
		login(driver,username,password, rememberMe);
		System.out.println("Successfully Logged In");
		Thread.sleep(3000);
		driver.findElement(By.xpath(p.getProperty("logout"))).click();
		driver.quit();
	}
	@Test(dataProvider="LoginData")
	public void TestInvalidEmail(String browser) throws InterruptedException
	{	
		String username = "manav@envirya.in@com";
    	String password = "12345@qwert";
    	boolean rememberMe = false;
		WebDriver driver = createBrowser(browser);
		login(driver,username,password, rememberMe);
		Assert.assertEquals("Invalid email or password.", driver.findElement(By.xpath("//div[contains(text(),'Invalid email or password.')]")).getText());
		System.out.println(driver.findElement(By.xpath("//div[contains(text(),'Invalid email or password.')]")).getText());
		driver.quit();
	}  
    @Test(dataProvider="LoginData")
	public void TestInvalidPassword(String browser) throws InterruptedException 
	{	
    	String username = "roohi@envirya.in";
    	String password = "1234@";
    	boolean rememberMe = false;
		WebDriver driver = createBrowser(browser);
		login(driver,username,password, rememberMe); 
		Assert.assertEquals("Invalid email/password",driver.findElement(By.xpath("//div[@class='Toastify__toast-body']")).getText());
		System.out.println(driver.findElement(By.xpath("//div[@class='Toastify__toast-body']")).getText());
		driver.quit();
	}	
    @Test(dataProvider="LoginData")
	public void TestNoEmail(String browser) throws InterruptedException 
	{
    	String username = "";
    	String password = "Yaali@1421";
    	boolean rememberMe = false;
		WebDriver driver = createBrowser(browser);
		login(driver,username,password, rememberMe); 
    	String Expected_tooltip ="Please fill out this field.";
    	Assert.assertEquals("Please fill out this field.", Expected_tooltip);
    	System.out.println("Test Passed-Please fill out this field");
    	driver.quit();
    }	
    @Test(dataProvider="LoginData")	
	public void TestNoPassword(String browser) throws InterruptedException 
	{
    	String username = "manav@envirya.in";
    	String password = "";
    	boolean rememberMe = false;
    	WebDriver driver = createBrowser(browser);
    	login(driver,username,password, rememberMe);
		String Expected_tooltip ="Please fill out this field.";
		Assert.assertEquals("Please fill out this field.", Expected_tooltip);
		System.out.println("Test Passed-Please fill out this field");
		driver.quit();
	}
	  
}
