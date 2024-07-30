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

8. **Day 8:** Implement caching
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
```
Good luck and happy coding!

---

Feel free to customize and expand the assignment as needed.
