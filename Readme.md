## What is an API?

Think of an API (Application Programming Interface) as a messenger that delivers your requests to a system and returns the response back to you. It's like ordering food at a restaurant. You tell the waiter what you want (your request), and they bring you the food (the response).

In the digital world, APIs are used by applications to communicate with each other. For example, a weather app uses an API to fetch weather data from a weather service.

#### Key Points:

- APIs act as intermediaries between different software applications.
- They define how applications can interact with each other.
- They simplify software development by providing pre-built functionalities.


## RESTful Architecture: The Blueprint for APIs

### What is REST?

<b><i>REST</i></b> stands for <b><i>Representational State Transfer</i></b>. 
<br>It's a set of architectural constraints that guide how web-based systems should interact. In simpler terms, it's a style of designing APIs that makes them easy to use, scalable, and efficient.

It relies on a stateless, client-server communication protocol, typically HTTP.

In a RESTful API, resources are identified by URLs (Uniform Resource Locators), and interactions with these resources are done using standard HTTP methods.


API (Application Programming Interface) allows different software applications to communicate with each other. <BR>REST APIs use HTTP requests to perform CRUD (Create, Read, Update, Delete) operations on resources.


#### Key Principles of REST:

- <B>Client-Server Architecture</B>: The API (server) and the application using it (client) are separate entities.

- <B>Statelessness</B>: Each request from the client contains all the information needed to understand and process it. The server doesn't store session information.

- <B>Cacheable</B>: Responses can be cached to improve performance.

- <B>Uniform Interface</B>: A consistent set of operations (GET, POST, PUT, DELETE) is used for accessing and manipulating resources.

- <B> Layered System</B>: Multiple layers of servers can be used to improve scalability and security.

#### Example:

Imagine a RESTful API for a blog. 
You might have resources like posts, comments, and users. Each resource would have its own URL (e.g., /posts, /comments, /users). You would use HTTP methods to create, read, update, and delete these resources.

## Comparison with Other APIs

- <B>SOAP (Simple Object Access Protocol)</B>: A protocol for exchanging structured information in web services, often over HTTP or SMTP. SOAP uses XML for messages and has a more rigid structure compared to REST.

 - <B>GraphQL </B>: A query language for APIs that allows clients to request exactly the data they need. It provides more flexibility than REST but can be more complex to implement.


## HTTP Methods: The Verbs of the Web

HTTP methods are the actions you can perform on a resource through a RESTful API. They define the operation you want to execute.

### Common HTTP Methods:

* **GET:** Retrieves data from a specified resource.
  * Example: `GET /users` to fetch a list of users.
* **POST:** Creates a new resource.
  * Example: `POST /users` to create a new user.
* **PUT:** Updates an existing resource.
  * Example: `PUT /users/123` to update user with ID 123.
* **DELETE:** Deletes a resource.
  * Example: `DELETE /users/123` to delete user with ID 123.

* **HEAD:** Similar to GET, but only returns headers.
* **OPTIONS:** Used to determine the supported methods for a resource.
* **PATCH:** Applies partial modifications to a resource.


##  Building a REST API

### Structuring Endpoints

Endpoints are the URLs through which clients interact with the resources of an API. A well-structured endpoint makes the API intuitive and easy to use.

```bash
/users                # Collection of users
/users/{id}           # Specific user
/users/{id}/posts     # Posts by a specific user
/posts                # Collection of posts
/posts/{id}           # Specific post

```

### CRUD Operations

#### 1. Create (POST):

- Adds a new resource to the server.
- Endpoint: /users

- Request:
```bash
{
  "name": "John Doe",
  "email": "john@example.com"
}
```
- Response:
```bash
{
  "id": 1,
  "name": "John Doe",
  "email": "john@example.com"
}

```

#### 2. Read (GET):

- Retrieves one or more resources from the server.
- Endpoint: /users (for a list of users), /users/{id} (for a specific user)
- Request: GET /users

- response:
```json
[
  {
    "id": 1,
    "name": "John Doe",
    "email": "john@example.com"
  },
  {
    "id": 2,
    "name": "Jane Doe",
    "email": "jane@example.com"
  }
]

```

#### 3. Update (PUT/PATCH):

- Modifies an existing resource.
- Endpoint: /users/{id}

#### 4. Delete (DELETE):

- Removes a resource from the server.
- Endpoint: /users/{id}
- Request: DELETE /users/1
- Response: 204 No Content


## Example REST API using Node.js and Express


```javascript
const express = require('express');
const app = express();
const port = 3000;

app.use(express.json());

// Sample data
let users = [
  { id: 1, name: 'John Doe', email: 'john@example.com' },
  { id: 2, name: 'Jane Doe', email: 'jane@example.com' }
];

// CRUD operations

// Create a new user
app.post('/users', (req, res) => {
  const newUser = { id: users.length + 1, ...req.body };
  users.push(newUser);
  res.status(201).json(newUser);
});

// Read all users
app.get('/users', (req, res) => {
  res.json(users);
});

// Read a specific user
app.get('/users/:id', (req, res) => {
  const user = users.find(u => u.id === parseInt(req.params.id));
  if (!user) return res.status(404).send('User not found');
  res.json(user);
});

// Update a user
app.put('/users/:id', (req, res) => {
  const user = users.find(u => u.id === parseInt(req.params.id));
  if (!user) return res.status(404).send('User not found');

  user.name = req.body.name;
  user.email = req.body.email;
  res.json(user);
});

// Delete a user
app.delete('/users/:id', (req, res) => {
  const userIndex = users.findIndex(u => u.id === parseInt(req.params.id));
  if (userIndex === -1) return res.status(404).send('User not found');

  users.splice(userIndex, 1);
  res.status(204).send();
});

// Start the server
app.listen(port, () => {
  console.log(`Server running on port ${port}`);
});

```

###  Data Formats

When interacting with REST APIs, data is typically exchanged between the client and server in specific formats. The two most common data formats are JSON and XML. Let's dive into each of these formats.

#### JSON (JavaScript Object Notation)

**JSON** is a lightweight data interchange format that is easy for humans to read and write, and easy for machines to parse and generate. It is based on a subset of JavaScript, but it is language-independent, making it a widely used format for data exchange.

**Example JSON:**
```json
{
  "id": 1,
  "name": "John Doe",
  "email": "john@example.com",
  "isActive": true,
  "roles": ["admin", "user"]
}
```

**Advantages of JSON:**
- Readable and easy to understand.
- Lightweight and efficient.
- Widely supported by most programming languages.
- Easy to parse and generate.

#### XML (Extensible Markup Language)

**XML** is a markup language that defines a set of rules for encoding documents in a format that is both human-readable and machine-readable. It is more verbose than JSON and is used less frequently in modern web APIs, but it is still used in some legacy systems.

**Example XML:**
```xml
<user>
  <id>1</id>
  <name>John Doe</name>
  <email>john@example.com</email>
  <isActive>true</isActive>
  <roles>
    <role>admin</role>
    <role>user</role>
  </roles>
</user>
```

**Advantages of XML:**
- Flexible and can represent complex data structures.
- Supports namespaces to avoid element name conflicts.
- Can be validated against a schema (XSD) to ensure data integrity.
- Widely used in enterprise systems and some legacy systems.

#### JSON vs. XML

| Feature         | JSON                            | XML                            |
|-----------------|---------------------------------|--------------------------------|
| Readability     | Easy to read                    | More verbose                   |
| Data Size       | Smaller                         | Larger                         |
| Parsing         | Faster                          | Slower                         |
| Supported Types | Limited (string, number, array) | More comprehensive (elements, attributes) |
| Schema Support  | JSON Schema (optional)          | XSD (built-in)                 |
| Usage           | Modern web APIs                 | Legacy systems, enterprise systems |

#### Example: Converting JSON to XML

Let's look at an example of converting a JSON object to XML.

**JSON:**
```json
{
  "id": 1,
  "name": "John Doe",
  "email": "john@example.com",
  "isActive": true,
  "roles": ["admin", "user"]
}
```

**Equivalent XML:**
```xml
<user>
  <id>1</id>
  <name>John Doe</name>
  <email>john@example.com</email>
  <isActive>true</isActive>
  <roles>
    <role>admin</role>
    <role>user</role>
  </roles>
</user>
```

#### Example REST API Endpoint with JSON Response

Here's an example of a REST API endpoint that returns JSON data.

**Endpoint:**
```plaintext
GET /users/1
```

**Response:**
```json
{
  "id": 1,
  "name": "John Doe",
  "email": "john@example.com",
  "isActive": true,
  "roles": ["admin", "user"]
}
```



