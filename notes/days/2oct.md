## 🔹 End to End Testing:

*"Team, imagine we are climbing a mountain. On the way up, we don’t just test one step of the climb. We check if our shoes grip well (unit test), if our rope holds (API test), if our vision is clear (UI test). But the true success is only when we reach the top — from the base camp all the way to the summit — safely. That complete journey is what we call **End-to-End Testing**."*

### 🔹 Connecting to Our Java Project

Think about the **shopping portal** we are building:

1. **Backend**: We have a **Product API** (`/products`, `/products/{id}`) created using Spring Boot.
2. **Frontend**: We have `index.html` that fetches products using **JavaScript** and displays them in a list.
3. **User’s Journey**: A customer opens the browser → visits the site → sees the product list.

So what does **End-to-End Testing** mean here?
👉 It means **testing the entire flow as if a real user is interacting**:

* The **browser is opened automatically**.
* The **UI calls the API**.
* The API **fetches data from backend services**.
* The UI **renders the list correctly**.

No shortcuts, no mocks, no isolation. The system is tested **as a whole** — from UI → API → Database → back to UI.

### 🔹 Example: End-to-End Test for Our Portal

With **Selenium + Rest-Assured + TestNG**, our E2E test would look like:

```java
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.annotations.*;
import static io.restassured.RestAssured.*;
import static org.hamcrest.Matchers.*;

public class EndToEndTest {
    WebDriver driver;

    @BeforeClass
    public void setup() {
        driver = new ChromeDriver();
    }

    @Test
    public void testFullUserJourney() {
        // Step 1: API Test - Backend responds correctly
        given()
        .when().get("http://localhost:8080/products")
        .then().statusCode(200)
        .body("size()", greaterThan(0));

        // Step 2: UI Test - User opens the browser and sees products
        driver.get("http://localhost:8080/index.html");
        String header = driver.findElement(By.tagName("h2")).getText();
        assert header.equals("Available Products");

        // Step 3: UI actually renders list from API response
        int itemCount = driver.findElements(By.tagName("li")).size();
        assert itemCount > 0 : "No products displayed!";
    }

    @AfterClass
    public void teardown() {
        driver.quit();
    }
}
```


*"Students, notice the difference:*

* **Unit Test** → Checks a single method (`ProductController.getById()`).
* **API Test** → Checks an endpoint (`/products`).
* **UI Test** → Checks the web page loads (`index.html`).
* **End-to-End Test** → Simulates **the complete user journey** from browser → API → backend → response → UI.

E2E Testing is like **watching the entire movie** instead of just checking one scene. 🎥
It assures that **all pieces of the puzzle work together**, not just individually.

That’s why companies invest in E2E testing before they ship products — because users don’t care about individual components, they care about the experience end-to-end." 🌍
Alright, let’s break this down mentor-style and make it a story you can visualize, step by step. Imagine this is happening in your project lab with a real application and a team of testers and developers.

### **Scene: The Testing Lab**

You just finished coding your Spring Boot e-commerce project. It has **Products**, **Orders**, **Shopping Cart**, and **Payment** controllers. Everything looks good, your code compiles, and the Tomcat server is running. Now it’s time to **test it**.

### **Manual Testing: The Old Way**

Before automation, we testers used **Postman**:

1. Open Postman → enter the API URL `/api/products` → choose GET/POST/PUT/DELETE.
2. For **POST**, you manually enter JSON:

   ```json
   {
     "id": 66,
     "name": "Headphone",
     "price": 3500
   }
   ```
3. Click **Send**, check response → maybe `201 Created`.
4. For **GET**, check all products.
5. For **PUT**, modify an existing product, say laptop → update name to `Wireless Laptop`, price to `2200`.
6. For **DELETE**, choose product ID, click Send → expect `204 No Content`.

**Pain Point:** Every time you add new features, you repeat these steps for **every API**. Imagine having **50+ API methods**—that’s a lot of clicks, copy-paste, and manual checks.


### **Automation: Enter REST Assured + Selenium**

Now, instead of manual testing, you write **automated test scripts**:

* **Given**: Precondition setup → URL, headers, query params.
* **When**: Action → GET, POST, PUT, DELETE.
* **Then**: Verification → status code, response body, headers.

Example:

```java
given()
    .contentType(ContentType.JSON)
    .body("{\"id\":66,\"name\":\"Headphone\",\"price\":3500}")
.when()
    .post("/api/products")
.then()
    .statusCode(201);
```

✅ This is **exactly what you do in Postman**, but now it’s **scripted**, repeatable, and instant.


### **Running Tests**

1. You run **all test scripts**.

2. Out of 6 tests, 4 failed, 2 passed.

   * Why? Maybe your **POST API** didn’t create the product → `201 expected` but something else returned.
   * Maybe server wasn’t running → URL or port mismatch.

3. You restart the server:

   ```bash
   mvn spring-boot:run
   ```

   Then re-run tests → now they start passing.

4. You **update a product**:

   ```java
   given().contentType(JSON)
          .body("{\"id\":1,\"name\":\"Headphone\",\"price\":2200}")
   .when().put("/api/products/1")
   .then().statusCode(200);
   ```

   Result → `Laptop` replaced by `Headphone`, price updated. ✅

5. You **delete a product**:

   ```java
   when().delete("/api/products/3")
   .then().statusCode(204);
   ```

   Product removed, response verified automatically.



### **Why Automation Rocks**

* **No manual testers required** for routine API checks.
* **Regression Testing**: Every Sprint, you modify the code, but you run the **same tests** to make sure nothing breaks.
* **Integration Testing**: Your **frontend (Ajax/HTML)** calls API → backend (Spring Boot) → database (MySQL). REST Assured can test backend independently; Selenium can test frontend.
* **End-to-End Testing**: Frontend + backend + database → everything verified automatically.

### **Key Learning Points**

1. **REST Assured Methods:**

   * `given()` → setup headers, query params, body (preconditions)
   * `when()` → call the API method (GET/POST/PUT/DELETE)
   * `then()` → assertions (status codes, body content)

2. **Test Execution Flow:**

   * Write test once → run hundreds of times → instant feedback → save huge time.

3. **Selenium for Frontend:**

   * `findElement()`, `getTitle()`, `getCurrentUrl()`, `click()`, `sendKeys()`
   * Automates browser actions like a user, combined with REST Assured for backend.

---

### **End Scene: The Moral**

* Manual testing is **slow and repetitive**.
* Automation using **REST Assured + TestNG + Selenium** is **fast, reliable, and repeatable**.
* You now have a **living suite of tests** that ensures every Sprint, your project works perfectly—your regression testing, integration testing, and end-to-end testing are all **covered in one go**.
