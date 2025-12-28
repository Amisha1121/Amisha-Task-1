# Amisha-Task-1

package test;

import java.time.LocalTime;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;

public class OrderFlowTest {

    public static void main(String[] args) {

        // -------- Time Check (6 PM - 7 PM) --------
        LocalTime currentTime = LocalTime.now();
        LocalTime startTime = LocalTime.of(18, 0);
        LocalTime endTime = LocalTime.of(19, 0);

        if (currentTime.isBefore(startTime) || currentTime.isAfter(endTime)) {
            System.out.println("❌ Test can run only between 6 PM and 7 PM");
            return;
        }

        System.out.println("✅ Time condition satisfied. Starting test...");

        // -------- WebDriver Setup --------
        System.setProperty("webdriver.chrome.driver",
                "C:\\chromedriver\\chromedriver.exe");

        WebDriver driver = new ChromeDriver();
        driver.manage().window().maximize();

        try {
            // -------- Open Website --------
            driver.get("https://example-ecommerce.com");
            System.out.println("🌐 Website opened");

            // -------- Search Product --------
            driver.findElement(By.id("searchBox")).sendKeys("Headphones");
            driver.findElement(By.id("searchBtn")).click();
            System.out.println("🔍 Product searched");

            // -------- Select Product --------
            driver.findElement(By.xpath("//div[@class='product'][1]")).click();
            System.out.println("📦 Product selected");

            // -------- Add to Cart --------
            driver.findElement(By.id("addToCart")).click();
            System.out.println("🛒 Product added to cart");

            // -------- Get Total Amount --------
            WebElement amountElement = driver.findElement(By.id("totalAmount"));
            String amountText = amountElement.getText().replace("₹", "").trim();
            int amount = Integer.parseInt(amountText);

            System.out.println("💰 Cart Amount: ₹" + amount);

            // -------- Amount Validation --------
            if (amount <= 500) {
                System.out.println("❌ Payment amount must be greater than ₹500");
                driver.quit();
                return;
            }

            System.out.println("✅ Amount condition satisfied");

            // -------- Proceed to Payment --------
            driver.findElement(By.id("checkout")).click();
            driver.findElement(By.id("payNow")).click();
            System.out.println("💳 Payment successful");

            // -------- Order Confirmation --------
            WebElement confirmation =
                    driver.findElement(By.id("orderConfirmation"));

            System.out.println("🎉 Order Confirmed: "
                    + confirmation.getText());

        } catch (Exception e) {
            System.out.println("⚠️ Test Failed: " + e.getMessage());
        } finally {
            driver.quit();
            System.out.println("🔚 Browser closed");
        }
    }
}
