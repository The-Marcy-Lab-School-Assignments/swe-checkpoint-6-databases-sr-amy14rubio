# Short Response: Databases Checkpoint

Answer each question below in complete sentences. Aim for 3–5 sentences per answer — enough to show that you understand the concept, not just that you memorized a definition. Use the exact terms and concepts from the lessons, but write in your own words. Specific examples and analogies are encouraged.

---

## Question 1

What is the difference between **authentication** and **authorization**? Give a concrete example of how a user would encounter each in the context of a fullstack web application.

**Your answer:**
The difference between **authentication and authorization** is that _authentication_ is when the server ensures that **you have a stored cookie session**, while _authorization_ is when the server verifies if **the user is allowed to perform a CRUD action on an account**. An example for _authentication_ would be if I, an un-logged in user on Instagram were to try to comment on a post. The server would not allow me to comment on the post because **I am not logged into an Instagram account and do not have a stored session cookie**, meaning that I am not authenticated. An example for _authorization_ would be if I were logged into an Instagram account but I were to try to delete someone else's post. I would not be authorized to perform that action as, while I can delete my own posts, **I am not allowed to delete another user's post**.

---

## Question 2

Why should passwords **never** be stored as plaintext in a database? Explain what hashing is and its key properties that allow a server to verify a password without ever storing the original?

**Your answer:**
Passwords should never be stored as plaintext in a database because any savvy developer could **hack into the database and have access to all users' passwords**. Hashing is the conversion of a plaintext string to a hash, an irreversible fixed-length string of characters. As hashing is **pure and deterministic**, it always produces the same output for a given plaintext string. Every time a user tries to verify their password, the plaintext string is hashed and **is checked with the stored hashed version** of the password. This way the password and the attempted version of the password is **never stored in the database**.

---

## Question 3

Explain what it means when we say that "HTTP is stateless"? Explain why cookies are necessary in order to keep users logged-in across multiple sessions and how a server and a client work together to achieve this functionality.

**Your answer:**
When we say "HTTP is stateless", we mean that the server has **no memory of previous HTTP requests**. Whether a user logs into an account, or deletes or updates information, the server has no memory of the event when a new HTTP request comes in. Cookies solve this problem by **storing this information in the browser** instead of having the server keep track of the data in-memory. Every time an HTTP request is made, the server **checks the browser for a session cookie**, if found, the server allows the user to stay logged in and when the user logs out, the **session cookie is deleted from the browser**. To prevent malicious users from accessing the publicly available cookie, the cookie is _signed_ with a private 'secret' string that allows the server to detect if the cookie has been tampered with.

---

## Question 4

A frontend can hide a "Delete Account" button from users who aren't logged in. Why isn't that enough to protect the `DELETE /api/users/:id` route on the server? What are the two layers of protection that the backend implements to protect against this?

**Your answer:**
This isn't enough to protect the `DELETE /api/users/:id` route on the server because a savvy developer can use **curl commands to access the API endpoint** and manipulate data from other users' accounts. The two layers of protection that the backend implements are **authentication and authorization**. If the endpoint is trying to be accessed to a user who is not logged in, or a user who is logged but is not allowed to access said endpoint, **an error message is returned and the data is protected**.

---

## Question 5

What is **SQL injection**? Explain what makes the code below unsafe, then describe how parameterized queries fix the problem.

```js
// Unsafe — never do this!
pool.query(`SELECT * FROM users WHERE username = '${username}'`);
```

**Your answer:**
SQL injection is when a malicious user uses **SQL string interpolation values to write SQL queries** and manipulate data, such as dropping the database. Parameterized queries fix this problem by having a placeholder for the value, this placeholder is **passed in as an array** which the `pg` library sends to Postgres, separate from the SQL query. Parameterized queries are treated as **data entries, not SQL commands**, and prevents SQL injections.

---

## Question 6

What problem does the **`/api/auth/me`** endpoint pattern solve? When does the frontend call it and what does it return?

**Your answer:**
The problem that the endpoint solves is that it **checks if there is a session cookie stored** in the browser and **looks up a user** by their `user_id`. The frontend calls this in the model part of our code, which stores the `user_id` to then be called on by our controllers. The controller returns either a **401 error** if the user is not authenticated or, a **200 success status code along with the user's data** if the user was found.
