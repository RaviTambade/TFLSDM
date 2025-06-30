Absolutely — let’s do a **hands-on mentor-guided exercise**: we’ll build and run a Node.js app connected to a MySQL database, all inside Docker containers, using Docker Compose.

This exercise will show you how to containerize both **your Node.js app** and a **MySQL database**, wire them together, and make them talk — just like they would in production.

---

## 👨‍🏫 **📝 Prerequisites**

✅ Node.js project folder (e.g., `my-node-app`)
✅ A simple Node.js app that connects to MySQL (or you’ll use the sample I’ll provide)
✅ Docker and Docker Compose installed

---

## 📁 **1) Sample Node.js App**

In a new folder (e.g., `my-node-app`), create the following files:

**package.json**

```json
{
  "name": "node-mysql-docker",
  "version": "1.0.0",
  "main": "app.js",
  "scripts": {
    "start": "node app.js"
  },
  "dependencies": {
    "express": "^4.18.2",
    "mysql2": "^3.2.0"
  }
}
```

**app.js**

```js
const express = require('express');
const mysql = require('mysql2');

const app = express();

// Use environment variables for connection info
const connection = mysql.createConnection({
  host: process.env.DB_HOST || 'localhost',
  user: process.env.DB_USER || 'root',
  password: process.env.DB_PASSWORD || '',
  database: process.env.DB_NAME || 'testdb'
});

app.get('/', (req, res) => {
  connection.query('SELECT NOW() as now', (err, results) => {
    if (err) {
      console.error('DB error:', err);
      res.status(500).send('Database error');
    } else {
      res.send(`Connected to MySQL! Server time: ${results[0].now}`);
    }
  });
});

app.listen(3000, () => console.log('Server started on port 3000'));
```

---

## 📦 **2) Create a Dockerfile**

In the same folder:

```dockerfile
# Use official Node.js image
FROM node:18

# Set working directory
WORKDIR /usr/src/app

# Copy package.json and install dependencies
COPY package*.json ./
RUN npm install

# Copy the rest of your app
COPY . .

# Expose port
EXPOSE 3000

# Start the app
CMD ["npm", "start"]
```

---

## 🗂️ **3) Create docker-compose.yml**

In the same folder, create a **docker-compose.yml** file:

```yaml
version: '3.8'

services:
  db:
    image: mysql:8
    environment:
      MYSQL_ROOT_PASSWORD: root
      MYSQL_DATABASE: testdb
    ports:
      - "3306:3306"
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-h", "localhost"]
      interval: 10s
      retries: 5

  app:
    build: .
    ports:
      - "3000:3000"
    environment:
      DB_HOST: db
      DB_USER: root
      DB_PASSWORD: root
      DB_NAME: testdb
    depends_on:
      db:
        condition: service_healthy
```

Here’s what’s happening:

* **db service** uses the official MySQL 8 image and sets up a database called `testdb` with root password `root`.
* **app service** builds your Dockerfile, exposes port 3000, and injects environment variables to connect to the `db` service.
* `depends_on` ensures `app` waits until MySQL is healthy.

---

## 🚀 **4) Build & Run the Containers**

From the project directory, run:

```bash
docker-compose up --build
```

---

## 🔎 **5) Test Your App**

Once both services are running, open your browser or use curl:

```
http://localhost:3000/
```

You should see:

```
Connected to MySQL! Server time: 2025-06-29T10:42:00.000Z
```

This proves your Node.js app is talking to your MySQL database, all inside containers!

---

## 🛑 **6) Stop & Clean Up**

When you’re done, stop your services with:

```bash
docker-compose down
```

---

## 🔥 **What You Learned**

✅ Built a Docker image for your Node.js app
✅ Spun up MySQL in a separate container
✅ Used Docker Compose to orchestrate multiple services
✅ Connected your app to a MySQL database container
✅ Verified end-to-end communication

---

## 📝 **Bonus Challenge: Seed Your Database**

Try adding an **init.sql** file to your project:

```sql
CREATE TABLE IF NOT EXISTS messages (id INT AUTO_INCREMENT PRIMARY KEY, content VARCHAR(255));
INSERT INTO messages (content) VALUES ('Hello from Docker!');
```

Then mount it to MySQL in `docker-compose.yml`:

```yaml
db:
  image: mysql:8
  ...
  volumes:
    - ./init.sql:/docker-entrypoint-initdb.d/init.sql
```

Now your MySQL container will initialize with a table and data!

---

👨‍🏫 *Remember*: Containers let you test your entire stack locally exactly as it will run in production — a core skill for any modern developer.


