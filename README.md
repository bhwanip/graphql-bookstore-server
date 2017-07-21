Bookstore GraphQL API
=====================
Most tutorials that teach GraphQL are code-centric - they start with a simple example, such as a to-do list, and show the code to implement it. In this project, I will take a domain-centric approach, starting with a simple domain consisting of different entities and relationships and showing how such a domain could be implemented using GraphQL.

The domain we have selected is that of a bookstore. The bookstore sells books. A book can have one or more authors and a single publisher. However, an author could have written several books and a publisher could have published many books too. More formally, book-to-author is a many-to-many relationship, whereas book-to-publisher is a many-to-one relationship. This is depicted in the domain model below. In this project we show how such a domain can be implemented using GraphQL.

![Domain Model](assets/bookstore-domain-model.png)

Getting started (Dev Mode)
--------------------------
To run the application in development mode:
```bash
$ npm install
$ npm run start-db
$ npm run watch  <--- run this in a separate shell
```

- `npm install` installs the required node libraries under `node_modules`. This needs to be run only once.
- `npm run start-db` starts a simple JSON based database server. It is implemented using [JSON Server](https://github.com/typicode/json-server) which provides a simple persistence solution with a full REST API. The JSON file that stores the data is located at /data/db.json. It is seeded with four books and associated authors and publishers.
- `npm run watch` starts the application. It is designed for an efficient development process. As you make changes to the code, the application will restart to reflect the changes immediately. Also, Node.js is started with the `--inspect` flag so that debugging is turned on.


Then point your browser to [http://localhost:8080/graphiql](http://localhost:8080/graphiql). You should now be able to perform queries and mutations as defined in the sections below.

Building for production
-----------------------
When you want to deploy the application into production, run the following command:

```bash
$ npm run build
$ npm start
```

- `npm run build` compiles the ES6 code into the dist directory. This needs to be run only once.
- `npm start` runs the application from the dist directory.

Queries
-------
### Get a list of all authors
```
{
  authors {
    name
  }
}
```

### Get an author along with their books
```
query Author($id: ID!) {
  author(id: $id) {
    name,
    books {
      name
    }
  }
}
```

with the following variables
```
{
  "id": "martin-fowler"
}
```

### Get a list of all publishers
```
{
  publishers {
    name
  }
}
```

### Get a publisher along with their books
```
query Publisher($id: ID!) {
  publisher(id: $id) {
    name,
    books {
      name
    }
  }
}
```

with the following variables
```
{
  "id": "addison-wesley"
}
```

### Get a list of all books
```
{
  books {
    name
  }
}
```

### Get a book along with its publisher and authors
```
query Book($id: ID!) {
  book(id: $id) {
    name,
    publisher {
      name
    },
    authors {
      name
    }
  }
}
```

with the following variables
```
{
  "id": "design-patterns"
}
```

Mutations
---------
### Create an author
```
mutation CreateAuthor($id: ID!, $name: String!) {
  createAuthor(id: $id, name: $name) {
    id,
    name
  }
}
```

with the following variables
```
{
  "id": "robert-martin",
  "name": "Robert C. Martin"
}
```

### Create a publisher
```
mutation CreatePublisher($id: ID!, $name: String!) {
  createPublisher(id: $id, name: $name) {
    id,
    name
  }
}
```

with the following variables
```
{
  "id": "prentice-hall",
  "name": "Prentice Hall"
}
```

### Create a book
```
mutation CreateBook($id: ID!, $name: String!, $publisherId: ID!, $authorIds: [ID!]!) {
  createBook(id: $id, name: $name, publisherId: $publisherId, authorIds: $authorIds) {
    id,
    name,
    publisher {
      name
    },
    authors {
      name
    }
  }
}
```

with the following variables
```
{
  "id": "clean-code",
  "name": "Clean Code - A Handbook of Agile Software Craftsmanship",
  "publisherId": "prentice-hall",
  "authorIds": [
    "robert-martin"
  ]
}
```
