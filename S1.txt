package Vinominds.SelinumProject;

import java.time.Duration;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.Assert;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.Test;

public class Page_Oject_Model_BA_HO {

    WebDriver driver;
    LoginPage loginPage;
    HomePage homePage;

    @BeforeMethod(alwaysRun = true)
    public void setUp() {

        driver = new ChromeDriver();

        driver.manage().window().maximize();

        // Implicit wait using TimeUnit syntax
        driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(60));
        driver.get("https://www.saucedemo.com/");

        loginPage = new LoginPage(driver);

        homePage = new HomePage(driver);
    }

    @Test
    public void loginTest() {

        loginPage.login("standard_user", "secret_sauce");

        String actualTitle = homePage.getProductsTitle();

        Assert.assertEquals(actualTitle, "Products");
    }

    @AfterMethod(alwaysRun = true)
    public void tearDown() {

        if (driver != null) {
            driver.quit();
        }
    }
}