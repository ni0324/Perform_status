package Vinominds.SelinumProject;

import java.util.Set;

import java.time.Duration;

import org.openqa.selenium.By;
import org.openqa.selenium.JavascriptExecutor;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.Test;

import io.github.bonigarcia.wdm.WebDriverManager;

public class Multiple_Window {

    WebDriver driver;

    @BeforeMethod
    public void setup() {
        
        WebDriverManager.chromedriver().setup();

        driver = new ChromeDriver();
        driver.manage().window().maximize();
        driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(60));

        driver.get("https://www.saucedemo.com/");
    }

    @Test
    public void multipleWindow() {
        
        driver.findElement(By.id("user-name")).sendKeys("standard_user");
        driver.findElement(By.id("password")).sendKeys("secret_sauce");
        driver.findElement(By.id("login-button")).click();

        
        ((JavascriptExecutor) driver).executeScript("window.open('https://www.flipkart.com/','_blank');");

        String parent = driver.getWindowHandle();
        Set<String> windows = driver.getWindowHandles();

        for (String window : windows) {
            if (!window.equals(parent)) {
                driver.switchTo().window(window);
                System.out.println("Child Window: " + driver.getTitle());
                driver.close();
            }
        }

        driver.switchTo().window(parent);
        System.out.println("Parent Window: " + driver.getTitle());
    }

    @AfterMethod
    public void tearDown() {
        if (driver != null) {
            driver.quit();
        }
    }
}