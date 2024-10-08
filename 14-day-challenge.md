# 14-Day Spring Boot Challenge - Daily Tasks

1. **Day 1:** Set up your first Spring Boot application using Spring Initializr
   - Task: Create a new Spring Boot project and run it successfully

2. **Day 2:** Create REST endpoints
   - Task: Implement GET, POST, PUT, and DELETE endpoints for a simple entity (e.g., "Book")

3. **Day 3:** Integrate Spring Data JPA
   - Task: Set up a database connection and implement basic CRUD operations using JPA repositories

4. **Day 4:** Implement data validation and error handling
   - Task: Add validation to your entity and create global exception handling

5. **Day 5:** Set up Swagger/OpenAPI documentation
   - Task: Configure Swagger and document your REST API

6. **Day 6:** Implement basic authentication using Spring Security
   - Task: Secure your endpoints with username/password authentication

7. **Day 7:** Write unit and integration tests
   - Task: Create tests for your service layer and REST endpoints

8. **Day 8:** Write unit and integration tests continued & Implement caching
   - Task: Use Spring's caching abstraction to cache responses from a slow operation

9. **Day 9:** Create scheduled tasks
   - Task: Implement a scheduled task that runs periodically (e.g., clean up old data)

10. **Day 10:** Set up logging and monitoring
    - Task: Configure logging using SLF4J and set up basic monitoring with Spring Boot Actuator

11. **Day 11:** Implement file upload and download
    - Task: Create endpoints to handle file upload and download operations

12. **Day 12:** Use Spring Profiles
    - Task: Set up different configurations for development and production environments

13. **Day 13:** Implement a simple message queue
    - Task: Use Spring AMQP to create a message producer and consumer

14. **Day 14:** Containerize your application
    - Task: Create a Dockerfile and containerize your Spring Boot application

Here's an expanded version of Day 1's challenge as an example of how to structure each day's post:

```markdown
Day 1 Challenge: Set Up Your First Spring Boot Application

Welcome to Day 1 of our 14-Day Spring Boot Challenge! Today, we're laying the foundation by creating our first Spring Boot application.

Your task:
1. Go to https://start.spring.io/
2. Set up a new Spring Boot project with the following details:
   - Project: Maven
   - Language: Java
   - Spring Boot: 2.7.x (latest stable version)
   - Group: com.challenge
   - Artifact: bootcamp
   - Dependencies: Spring Web
3. Generate the project and import it into your favorite IDE
4. Run the application and verify it starts without errors
5. Create a simple controller that returns "Hello, Spring Boot!" when accessed via browser

Bonus: Customize the start.spring.io experience by adding a few more dependencies you think might be useful (e.g., DevTools, Actuator).

Need help? Check out these resources:
- Spring Quickstart Guide: https://spring.io/quickstart
- Spring Initializr Guide: https://github.com/spring-io/initializr

Share a screenshot of your running application in the comments along with any thoughts or questions you have. Don't forget to use #14DaySpringBootChallenge!

Remember, the Spring Boot community is here to help. If you're stuck, don't hesitate to ask questions in the comments. Good luck, and happy coding!
```

### Day 2 Challenge: Create REST Endpoints

Welcome to Day 2 of our 14-Day Spring Boot Challenge! Today, we'll dive into creating RESTful endpoints for a simple entity.

#### Your Task:

1. **Create a New Entity:**
   - Define a new entity called `Book` with the following fields:
     - `id` (Long)
     - `title` (String)
     - `author` (String)
     - `isbn` (String)
     - `publishedDate` (LocalDate)

2. **Create a Book Controller:**
   - Implement the following REST endpoints in your controller:
     - `GET /books` - Retrieve a list of all books
     - `GET /books/{id}` - Retrieve a single book by its ID
     - `POST /books` - Create a new book
     - `PUT /books/{id}` - Update an existing book by its ID
     - `DELETE /books/{id}` - Delete a book by its ID

3. **Implement Basic Service Layer:**
   - Create a service layer that your controller will use to perform the necessary operations on the `Book` entity.
   - For now, you can use a simple `List<Book>` to store the books in memory.

4. **Test Your Endpoints:**
   - Use tools like Postman or curl to test your endpoints and ensure they are working correctly.

#### Example Code:

**Book Entity:**
```java
@Entity
public class Book {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String title;
    private String author;
    private String isbn;
    private LocalDate publishedDate;

    // Getters and setters
}
```

**Book Controller:**
```java
@RestController
@RequestMapping("/books")
public class BookController {
    private final BookService bookService;

    @Autowired
    public BookController(BookService bookService) {
        this.bookService = bookService;
    }

    @GetMapping
    public List<Book> getAllBooks() {
        return bookService.getAllBooks();
    }

    @GetMapping("/{id}")
    public Book getBookById(@PathVariable Long id) {
        return bookService.getBookById(id);
    }

    @PostMapping
    public Book createBook(@RequestBody Book book) {
        return bookService.createBook(book);
    }

    @PutMapping("/{id}")
    public Book updateBook(@PathVariable Long id, @RequestBody Book book) {
        return bookService.updateBook(id, book);
    }

    @DeleteMapping("/{id}")
    public void deleteBook(@PathVariable Long id) {
        bookService.deleteBook(id);
    }
}
```

**Book Service:**
```java
@Service
public class BookService {
    private final List<Book> books = new ArrayList<>();

    public List<Book> getAllBooks() {
        return books;
    }

    public Book getBookById(Long id) {
        return books.stream()
                .filter(book -> book.getId().equals(id))
                .findFirst()
                .orElseThrow(() -> new RuntimeException("Book not found"));
    }

    public Book createBook(Book book) {
        book.setId((long) (books.size() + 1));
        books.add(book);
        return book;
    }

    public Book updateBook(Long id, Book book) {
        Book existingBook = getBookById(id);
        existingBook.setTitle(book.getTitle());
        existingBook.setAuthor(book.getAuthor());
        existingBook.setIsbn(book.getIsbn());
        existingBook.setPublishedDate(book.getPublishedDate());
        return existingBook;
    }

    public void deleteBook(Long id) {
        books.removeIf(book -> book.getId().equals(id));
    }
}

Testing:
### 1. GET All Books

**Request:**

- **Method:** GET
- **URL:** `http://localhost:8080/books`

**Postman Configuration:**

1. Open Postman and create a new request.
2. Set the request method to `GET`.
3. Set the URL to `http://localhost:8080/books`.
4. Click `Send`.

### 2. GET a Book by ID

**Request:**

- **Method:** GET
- **URL:** `http://localhost:8080/books/{id}`

**Postman Configuration:**

1. Open Postman and create a new request.
2. Set the request method to `GET`.
3. Set the URL to `http://localhost:8080/books/{id}` (replace `{id}` with the actual ID of the book).
4. Click `Send`.

### 3. POST Create a New Book

**Request:**

- **Method:** POST
- **URL:** `http://localhost:8080/books`
- **Body Type:** raw
- **Body Content:**
  ```json
  {
    "title": "Spring Boot in Action",
    "author": "Craig Walls",
    "isbn": "9781617292545",
    "publishedDate": "2016-01-01"
  }
  ```

**Postman Configuration:**

1. Open Postman and create a new request.
2. Set the request method to `POST`.
3. Set the URL to `http://localhost:8080/books`.
4. Go to the `Body` tab.
5. Select `raw` and set the format to `JSON`.
6. Enter the JSON body content as shown above.
7. Click `Send`.

### 4. PUT Update an Existing Book

**Request:**

- **Method:** PUT
- **URL:** `http://localhost:8080/books/{id}`
- **Body Type:** raw
- **Body Content:**
  ```json
  {
    "title": "Spring Boot in Practice",
    "author": "John Doe",
    "isbn": "9781617292552",
    "publishedDate": "2022-01-01"
  }
  ```

**Postman Configuration:**

1. Open Postman and create a new request.
2. Set the request method to `PUT`.
3. Set the URL to `http://localhost:8080/books/{id}` (replace `{id}` with the actual ID of the book you want to update).
4. Go to the `Body` tab.
5. Select `raw` and set the format to `JSON`.
6. Enter the JSON body content as shown above.
7. Click `Send`.

### 5. DELETE a Book by ID

**Request:**

- **Method:** DELETE
- **URL:** `http://localhost:8080/books/{id}`

**Postman Configuration:**

1. Open Postman and create a new request.
2. Set the request method to `DELETE`.
3. Set the URL to `http://localhost:8080/books/{id}` (replace `{id}` with the actual ID of the book you want to delete).
4. Click `Send`.

### Summary of Endpoints:

- **GET** `http://localhost:8080/books`: Retrieve all books.
- **GET** `http://localhost:8080/books/{id}`: Retrieve a book by its ID.
- **POST** `http://localhost:8080/books`: Create a new book.
- **PUT** `http://localhost:8080/books/{id}`: Update an existing book by its ID.
- **DELETE** `http://localhost:8080/books/{id}`: Delete a book by its ID.
```

<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>demo</artifactId>
    <version>1.0-SNAPSHOT</version>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.5.4</version>
        <relativePath/>
    </parent>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-validation</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-api</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter-engine</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>

## Note few dependencies are not required can you identify them ?
Good luck and happy coding!

---

### Day 3

Day 3: Integrate Spring Data JPA

Task: a> Set up a database connection and implement basic CRUD operations using JPA repositories
      b> Create Mongodb account (cloud free tier). And connect to Mongodb.
+--------------------------------+
|       Spring Boot App          |
|                                |
|  +--------------------------+  |
|  |     Application Layer     |  |
|  |   (Controllers/Services)  |  |
|  +--------------------------+  |
|              |                 |
|  +--------------------------+  |
|  |    Data Access Layer      |  |
|  | (Repositories/Entities)   |  |
|  +--------------------------+  |
|              |                 |
|   +------------------------+   |   +-------------------------+
|   |    Spring Data JPA      |   |   |  Spring Data MongoDB    |
|   |      (H2 Database)      |   |   |    (MongoDB Database)   |
|   +------------------------+   |   +-------------------------+
|                                |
+--------------------------------+

     |                |
     |                |
     v                v

+------------------+       +--------------------------+
|    H2 Database   |       |     MongoDB Database     |
|  (In-Memory/Dev) |       | (Cloud/On-Prem/Prod)     |
+------------------+       +--------------------------+


### 1. `pom.xml`
This `pom.xml` file includes dependencies for Spring Boot, Spring Data JPA, H2 Database, and testing.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.example</groupId>
    <artifactId>bookstore</artifactId>
    <version>1.0-SNAPSHOT</version>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.7.4</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <dependencies>
        <!-- Spring Web Dependency -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <!-- Spring Data JPA Dependency -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>

        <!-- H2 Database Dependency -->
        <dependency>
            <groupId>com.h2database</groupId>
            <artifactId>h2</artifactId>
            <scope>runtime</scope>
        </dependency>

        <!-- Spring Boot Test Dependency -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
</project>
```

### 2. `application.properties`
This file configures the H2 in-memory database and enables the H2 console.

```properties
# H2 Database Configuration
spring.datasource.url=jdbc:h2:mem:testdb
spring.datasource.driverClassName=org.h2.Driver
spring.datasource.username=sa
spring.datasource.password=password
spring.jpa.database-platform=org.hibernate.dialect.H2Dialect

# Enable H2 Console
spring.h2.console.enabled=true
spring.h2.console.path=/h2-console
```

### 3. `Book.java`
This is the `Book` entity class that represents the `books` table in the database.

```java
package com.example.bookstore.model;

import javax.persistence.*;
import java.time.LocalDate;

@Entity
public class Book {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    private String title;
    private String author;
    private String isbn;
    private LocalDate publishedDate;

    // Getters and setters
    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getAuthor() {
        return author;
    }

    public void setAuthor(String author) {
        this.author = author;
    }

    public String getIsbn() {
        return isbn;
    }

    public void setIsbn(String isbn) {
        this.isbn = isbn;
    }

    public LocalDate getPublishedDate() {
        return publishedDate;
    }

    public void setPublishedDate(LocalDate publishedDate) {
        this.publishedDate = publishedDate;
    }
}
```

### 4. `BookRepository.java`
This is the JPA repository interface for the `Book` entity.

```java
package com.example.bookstore.repository;

import com.example.bookstore.model.Book;
import org.springframework.data.jpa.repository.JpaRepository;

public interface BookRepository extends JpaRepository<Book, Long> {
}
```

### 5. `BookService.java`
This is the service layer that handles business logic for managing books.

```java
package com.example.bookstore.service;

import com.example.bookstore.model.Book;
import com.example.bookstore.repository.BookRepository;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;

import java.util.List;

@Service
public class BookService {

    private final BookRepository bookRepository;

    @Autowired
    public BookService(BookRepository bookRepository) {
        this.bookRepository = bookRepository;
    }

    public List<Book> getAllBooks() {
        return bookRepository.findAll();
    }

    public Book getBookById(Long id) {
        return bookRepository.findById(id)
                .orElseThrow(() -> new RuntimeException("Book not found"));
    }

    public Book createBook(Book book) {
        return bookRepository.save(book);
    }

    public Book updateBook(Long id, Book book) {
        Book existingBook = getBookById(id);
        existingBook.setTitle(book.getTitle());
        existingBook.setAuthor(book.getAuthor());
        existingBook.setIsbn(book.getIsbn());
        existingBook.setPublishedDate(book.getPublishedDate());
        return bookRepository.save(existingBook);
    }

    public void deleteBook(Long id) {
        bookRepository.deleteById(id);
    }
}
```

### 6. `BookController.java`
This is the controller that exposes RESTful endpoints for managing books.

```java
package com.example.bookstore.controller;

import com.example.bookstore.model.Book;
import com.example.bookstore.service.BookService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/books")
public class BookController {

    private final BookService bookService;

    @Autowired
    public BookController(BookService bookService) {
        this.bookService = bookService;
    }

    @GetMapping
    public List<Book> getAllBooks() {
        return bookService.getAllBooks();
    }

    @GetMapping("/{id}")
    public Book getBookById(@PathVariable Long id) {
        return bookService.getBookById(id);
    }

    @PostMapping
    public Book createBook(@RequestBody Book book) {
        return bookService.createBook(book);
    }

    @PutMapping("/{id}")
    public Book updateBook(@PathVariable Long id, @RequestBody Book book) {
        return bookService.updateBook(id, book);
    }

    @DeleteMapping("/{id}")
    public void deleteBook(@PathVariable Long id) {
        bookService.deleteBook(id);
    }
}
```

### 7. `BookstoreApplication.java`
This is the main class to run your Spring Boot application.

```java
package com.example.bookstore;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class BookstoreApplication {

    public static void main(String[] args) {
        SpringApplication.run(BookstoreApplication.class, args);
    }
}
```

### Running the Application
1. **Run the Application:**
   - Use your IDE or the command line to run the Spring Boot application.
   - If using the command line, run `mvn spring-boot:run`.

2. **Access the H2 Console:**
   - Visit `http://localhost:8080/h2-console` to access the H2 database console.
   - Use the JDBC URL `jdbc:h2:mem:testdb`, username `sa`, and an empty password.

3. **Test Endpoints:**
   - You can use Postman or curl to interact with the REST API at `http://localhost:8080/books`.

This setup provides a basic Spring Boot application with JPA integration. You can expand on it by adding more features, custom queries, and handling edge cases.


##### Day#5 #####
To add Swagger API documentation to your Spring Boot application, you can use Springfox or Springdoc OpenAPI. I'll demonstrate how to do it with Springdoc OpenAPI, which is a popular choice and easy to set up.

### Step 1: Add the Springdoc OpenAPI Dependency

First, add the Springdoc OpenAPI dependency to your `pom.xml`:

```xml
<dependencies>
    <!-- Other dependencies -->

    <!-- Springdoc OpenAPI dependency -->
    <dependency>
        <groupId>org.springdoc</groupId>
        <artifactId>springdoc-openapi-ui</artifactId>
        <version>1.7.0</version>
    </dependency>
</dependencies>
```

### Step 2: Access the Swagger UI

Once you've added the dependency and started your Spring Boot application, the Swagger UI will be available at the following URL:

```
http://localhost:8080/swagger-ui.html
```

### Step 3: Customize the API Documentation (Optional)

You can customize the API documentation by using annotations. Here is an example with the provided `BookController.java`:

#### Annotating the BookController

```java
package com.example.demo.controller;

import com.example.demo.entity.Book;
import com.example.demo.service.BookService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;
import io.swagger.v3.oas.annotations.Operation;
import io.swagger.v3.oas.annotations.tags.Tag;

import java.util.List;

import javax.validation.Valid;

@RestController
@RequestMapping("/books")
@Tag(name = "Books", description = "API for managing books")
public class BookController {

    private final BookService bookService;

    @Autowired
    public BookController(BookService bookService) {
        this.bookService = bookService;
    }

    @Operation(summary = "Get all books")
    @GetMapping
    public List<Book> getAllBooks() {
        return bookService.getAllBooks();
    }

    @Operation(summary = "Get a book by its ID")
    @GetMapping("/{id}")
    public Book getBookById(@PathVariable Long id) {
        return bookService.getBookById(id);
    }

    @Operation(summary = "Create a new book")
    @PostMapping
    public ResponseEntity<Book> createBook(@Valid @RequestBody Book book) {
        Book createdBook = bookService.createBook(book);
        return new ResponseEntity<>(createdBook, HttpStatus.CREATED);
    }

    @Operation(summary = "Delete a book by its ID")
    @DeleteMapping("/{id}")
    public void deleteBook(@PathVariable Long id) {
        bookService.deleteBook(id);
    }
}
```

### Step 4: Configure OpenAPI (Optional)

You can customize the OpenAPI documentation in your `application.properties` or `application.yml`:

#### Example with `application.yml`:

```yaml
springdoc:
  api-docs:
    path: /v3/api-docs
  swagger-ui:
    path: /swagger-ui.html
```

### Final Step: Run Your Application

After these changes, run your Spring Boot application. You should be able to see the Swagger UI by navigating to `http://localhost:8080/swagger-ui.html`, where you can interact with your API and see the documentation.


### Day#6 ###
### Day 6: Implement Basic Authentication Using Spring Security

**Objective**: Secure your Spring Boot application's endpoints with username/password authentication using Spring Security.

---

### Step 1: Add Spring Security Dependency

1. Open your `pom.xml` file.
2. Add the Spring Security dependency as shown below:

```xml
<dependencies>
    <!-- Other dependencies -->
    
    <!-- Spring Security Dependency -->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-security</artifactId>
    </dependency>
    
    <!-- Other dependencies -->
</dependencies>
```

3. Save the `pom.xml` file.
4. Reload the Maven project to ensure the dependency is added.

---

### Step 2: Create a Security Configuration Class

1. In your project structure, create a new package called `com.example.demo.config` if it doesn't already exist.
2. Inside the `config` package, create a new class named `SecurityConfig`.

3. Add the following code to the `SecurityConfig` class:

```java
package com.example.demo.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
@EnableWebSecurity
public class SecurityConfig {

    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .authorizeRequests()
            .antMatchers("/books/**").authenticated()  // Secure all /books endpoints
            .anyRequest().permitAll()  // Allow all other endpoints
            .and()
            .httpBasic();  // Enable Basic Authentication

        return http.build();
    }

    @Bean
    public UserDetailsService userDetailsService() {
        InMemoryUserDetailsManager manager = new InMemoryUserDetailsManager();
        manager.createUser(User.withUsername("user")
            .password("{noop}password")  // {noop} is a password encoder that does nothing, just for testing
            .roles("USER")
            .build());
        return manager;
    }
}
```

4. Save the `SecurityConfig` class.

---

### Step 3: Restart the Application

1. Ensure the Spring Boot application is restarted to apply the new security configuration.
2. Use the following command if you're running the application from the command line:

```bash
mvn spring-boot:run
```

---

### Step 4: Test Authentication Using Postman

1. Open Postman.
2. Create a new request with the following details:
   - **Method**: GET
   - **URL**: `http://localhost:8080/books`

3. Under the **Authorization** tab:
   - Select **Basic Auth** as the type.
   - Enter the username as `user`.
   - Enter the password as `password`.

4. Click **Send** to make the request.

5. You should receive a `200 OK` response with the list of books. If the credentials are incorrect, you will receive a `401 Unauthorized` response.

---

### Step 5: Verify Unauthenticated Access to Other Endpoints

1. In Postman, change the URL to an endpoint that does not require authentication (e.g., `http://localhost:8080/`).
2. Remove the Authorization header by selecting **No Auth** under the **Authorization** tab.
3. Click **Send**.

4. You should receive a `200 OK` response if the endpoint is publicly accessible.

---

### Summary

- **Added Security Dependency**: Introduced Spring Security into the project.
- **Created Security Config**: Defined a security configuration to secure the `/books/**` endpoints.
- **Tested Basic Authentication**: Verified the security configuration using Postman.

---

With this setup, your application now has basic username/password authentication implemented, securing your specified endpoints.

Day#7 Article

# Understanding Caching: A Simple Guide with Technical Details

Hey there! Let's talk about caching. It's one of those computer science concepts that sounds complicated but is actually pretty simple when you break it down. And trust me, once you get it, you'll see it everywhere in tech.

## What the heck is caching anyway?

Okay, picture this: You've got a favorite book you're always reading. Instead of trekking to the library every time you want to read it, you keep a copy at home. That's basically caching in a nutshell.

In tech terms, it's like having a little storage area where you keep stuff you use a lot. This way, you can grab it quickly without having to go all the way back to where it originally came from.

## Why bother with caching?

Simple – it's all about speed! Going back to our book example, imagine if you had to run to the library every time you wanted to look up a favorite quote. Exhausting, right? 

In the digital world, caching does the same thing. It saves time by keeping frequently used data close at hand. This means your apps run faster because they're not constantly fetching the same info from slow sources like databases.

## Getting technical: How to actually do this stuff

Alright, now that we've got the basics down, let's get our hands dirty with some code. We'll look at how to set up caching in a Spring Boot app using two methods: RAM-based caching (the simple way) and Redis caching (the fancy way).

### RAM-based caching: The quick and dirty method

This is like keeping Post-it notes on your desk. It's fast and easy, but don't expect miracles for big projects.

1. First, add this to your `pom.xml`:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
```

2. Then, in your main application class, add this annotation:

```java
@EnableCaching
```

3. Now, in your service class, you can use these nifty annotations:

```java
@Cacheable("books")
public List<Book> getAllBooks() {
    // Your code here
}

@Cacheable(value = "books", key = "#id")
public Book getBookById(Long id) {
    // Your code here
}

@CachePut(value = "books", key = "#book.id")
public Book createBook(Book book) {
    // Your code here
}

@CacheEvict(value = "books", key = "#id")
public void deleteBook(Long id) {
    // Your code here
}
```

### Redis caching: The cool kid on the block

Redis is like having a super-smart assistant who remembers everything. It's more powerful than simple RAM caching and can even share info between different servers.

1. Add these to your `pom.xml`:

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-cache</artifactId>
</dependency>
```

2. Set up Redis in your `application.properties`:

```properties
spring.cache.type=redis
spring.redis.host=localhost
spring.redis.port=6379
```

3. Enable caching just like before:

```java
@EnableCaching
```

4. And your service class looks the same as with RAM caching. Spring's smart enough to figure out the rest.

## Wrapping up

So there you have it – caching in a nutshell. It's all about keeping frequently used stuff close by for quick access. Whether you're using simple RAM caching or fancy Redis caching, the idea's the same: store the stuff you use a lot where you can grab it quickly.

This trick helps your apps run faster and keeps your users happy. And let's face it, in today's world of short attention spans, every millisecond counts!

Now go forth and cache like a pro! And remember, when in doubt, just think of that book on your shelf at home. That's all caching really is – just a lot geekier.
