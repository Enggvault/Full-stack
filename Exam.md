# Full Stack Development Exam

**Total Marks:** 60
- 20 MCQs (1 marks each) = 20 marks
- 5 Short Questions (2 marks each) = 10 marks
- 1 Long Question (10 marks) = 10 marks

---

## Part 1: Multiple Choice Questions (2 marks each) (do any 20)

### Section A: Very Easy
1. What is the command used to initialize a new local Git repository?
A) `git start`
B) `git new`
C) `git create`
D) `git init`

2. What is the primary purpose of a "commit" in Git?
A) To upload files directly to GitHub
B) To save a permanent snapshot of the project's current state
C) To undo the last change made to a file
D) To create a new branch

3. Which HTML tag is used for the most important heading on a page?
   A) <title>
   B) <heading>
   C) <h1>
   D) <h6>

4. Which CSS property is used to change the background color of an element?
   A) color
   B) background-color
   C) bg-color
   D) color-background

5. In JavaScript, which keyword is used to declare a block-scoped variable that cannot be reassigned?
   A) var
   B) let
   C) const
   D) static

6. What is the common default port for local Node.js development?
   A) 80
   B) 443
   C) 5432
   D) 3000

7. Which HTTP method is typically used to retrieve data from a server without modifying it?
   A) POST
   B) GET
   C) PUT
   D) DELETE

8. In JSON format, string keys must be enclosed in:
   A) Single quotes ('')
   B) Double quotes ("")
   C) Backticks (``)
   D) No quotes

9. What JavaScript engine powers Node.js under the hood?
   A) SpiderMonkey
   B) JavaScriptCore
   C) V8 Engine
   D) Chakra

10. Which of the following is considered a NoSQL (Document) database?
    A) PostgreSQL
    B) MySQL
    C) SQLite
    D) MongoDB

11. What does CSS stand for?
    A) Cascading Style Sheets
    B) Creative Style Sheets
    C) Computer Style Sheets
    D) Colorful Style Sheets

12. In Express.js, what does the `req` object represent in a route handler?
    A) The response sent to the client
    B) The incoming HTTP request
    C) The database connection
    D) The error handler

### Section B: Tricky & Hard
13. Which of the following CSS selectors has the highest specificity score?
    A) div p::before
    B) .class:hover
    C) #header
    D) [type="email"]

14. What is the output of the following JavaScript code due to hoisting?
    ```javascript
    console.log(x);
    var x = 10;
    ```
    A) 10
    B) ReferenceError
    C) null
    D) undefined

15. Which of the following HTTP methods is idempotent but NOT safe?
    A) GET
    B) HEAD
    C) PUT
    D) POST

16. In the Node.js event loop, which queue has the highest execution priority and will run before the others?
    A) Timers phase
    B) I/O callbacks
    C) process.nextTick() queue
    D) Promise microtask queue

17. In REST API design, which method is strictly best suited for partially updating a resource (e.g., changing only a user's email address)?
    A) PUT
    B) POST
    C) PATCH
    D) UPDATE

18. Which ACID database property ensures that a transaction is "all or nothing"?
    A) Consistency
    B) Isolation
    C) Durability
    D) Atomicity

19. What happens in modern Node.js if a JavaScript Promise is rejected but there is no `.catch()` handler attached?
    A) The program silently ignores it
    B) It resolves to undefined
    C) It throws an unhandled promise rejection error
    D) The Event Loop crashes immediately

20. In a distributed database system, the CAP theorem states you can only pick two out of three guarantees. Which of the following is NOT one of the CAP guarantees?
    A) Consistency
    B) Availability
    C) Partition Tolerance
    D) Performance
    
21. What is the main difference between `git merge` and `git rebase` when integrating changes?
A) `merge` deletes the original branch, while `rebase` keeps it.
B) `merge` preserves the exact history of both branches, while `rebase` rewrites history to create a linear progression of commits.
C) `merge` can only be done locally, while `rebase` is required when pushing to a remote server.
D) There is no functional difference; they are aliases for the same operation.

22. What happens when you execute the command `git reset --hard HEAD~1`?
A) It unstages all files but keeps your working directory changes intact.
B) It creates a new commit that reverses the changes of the previous commit.
C) It completely removes the last commit and permanently deletes any file changes made in that commit from your working directory.
D) It moves HEAD to the previous commit but leaves the modifications in the staging area.

---

## Part 2: Short Questions (2 marks each) (do any 5)

### Section A: Easy

23. What is the fundamental difference between Git and GitHub?
24. What is the main difference between `margin` and `padding` in the CSS Box Model?
25. State the primary difference between the `let` and `const` keywords in JavaScript.
26. What is the purpose of the `alt` attribute in an HTML `<img>` tag, and why is it crucial for web accessibility?

### Section B: Very Hard
27. Explain the difference between `PUT` and `PATCH` in RESTful API design. Provide a specific scenario demonstrating why using `PUT` instead of `PATCH` for a partial update could result in unintended data loss.
28. Describe how the JavaScript Event Loop handles Promises (microtasks) versus `setTimeout` (timers) callbacks. If both are queued at the exact same time with a 0ms delay, which executes first and why?

---

## Part 3: Long Question (10 marks)  (do any 1)

### Very Hard & Tricky
29. You are tasked with designing the backend architecture for a high-traffic e-commerce platform that must handle 1 million concurrent users during a flash sale.
   a) **(3 marks)** Detail the N-Tier architecture you would implement. Explain the specific roles of the Load Balancer, API Gateway, Application Servers (e.g., Node.js), and the Database tier.
   b) **(4 marks)** The database has become a severe bottleneck. Explain how you would optimize read and write operations using Database Replication (Primary/Read Replicas) and a Caching Layer (like Redis).
   c) **(3 marks)** A user attempts to purchase a limited-edition item, but due to high concurrency, the item sells out microseconds before their request is processed by the database. Using RESTful design principles, what exact HTTP status code should your API return to indicate this state conflict, and how should the server structure the JSON response body to handle the error gracefully?
   
   
   //[ 29. (Points to look for)
    a) Load Balancer distributes incoming traffic across servers. API Gateway handles routing, rate limiting, and auth. App servers process business logic statelessly. Database tier stores persistent data.
    b) Read operations (product listings) are routed to Redis cache or Read Replicas. Write operations (orders) go directly to the Primary DB, which asynchronously updates the replicas.
    c) 409 Conflict. The JSON body should include an error structure: `{ "success": false, "error": { "code": "OUT_OF_STOCK", "message": "The item sold out during checkout." } }`.]

30. **Scenario:** You are collaborating on a team project using Git and GitHub. Your team lead assigns you to implement a new feature. Describe the complete, step-by-step workflow you would follow, starting from ensuring you have the latest code, to finally getting your new feature ready for the team lead to review and merge. Provide the necessary Git commands for each step.

*(Expected Answer [2 marks for each logical step with its command, total 10 marks]):*
1. **Sync with Remote Main (2 marks):** First, switch to the main branch (`git checkout main`) and pull the latest changes from the remote repository to ensure you are up to date (`git pull origin main`).
2. **Create a Feature Branch (2 marks):** Create and switch to a new isolated branch for your work to avoid messing up the main codebase (`git checkout -b feature-branch-name`).
3. **Stage and Commit Changes (2 marks):** After writing your code, add the modified files to the staging area (`git add .`) and save a snapshot with a descriptive message (`git commit -m "Add new feature"`).
4. **Push the Branch to GitHub (2 marks):** Upload your local feature branch to the remote repository on GitHub (`git push -u origin feature-branch-name`).
5. **Open a Pull Request (PR) (2 marks):** Navigate to the repository on GitHub and open a Pull Request from your feature branch into the main branch so the team lead can review, approve, and merge the code.


---
---

## Answer Key

### Part 1: MCQs
1. D, 2. B, 3. C, 4. B, 5. C, 6. D, 7. B, 8. B, 9. C, 10. D, 11. A, 12. B, 13. C, 14. D, 15. C, 16. C, 17. C, 18. D, 19. C, 20. D, 21 B, 22 C

### Part 2: Short Questions
21. Margin is the space outside the border of an element (pushing other elements away), whereas padding is the space inside the border (between the border and the content itself).
22. `let` allows a variable to be reassigned a new value later, whereas `const` prevents the variable identifier from being reassigned after its initial declaration.
23. The `alt` attribute provides alternative text if an image fails to load and is read aloud by screen readers to visually impaired users, making the image content accessible.
24. `PUT` replaces the entire resource, whereas `PATCH` only updates the specified fields. If you use `PUT` and only send the `email` field for a user, all other fields (like `name` and `role`) will be overwritten as null/deleted.
25. Promises go to the microtask queue, while `setTimeout` goes to the timers queue (macrotasks). The event loop processes the entire microtask queue before moving to the next phase, so the Promise will always execute before the `setTimeout` callback.

### Part 3: Long Question (Points to look for)
26. a) Load Balancer distributes incoming traffic across servers. API Gateway handles routing, rate limiting, and auth. App servers process business logic statelessly. Database tier stores persistent data.
    b) Read operations (product listings) are routed to Redis cache or Read Replicas. Write operations (orders) go directly to the Primary DB, which asynchronously updates the replicas.
    c) 409 Conflict. The JSON body should include an error structure: `{ "success": false, "error": { "code": "OUT_OF_STOCK", "message": "The item sold out during checkout." } }`.

