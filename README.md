# study-graphQL

学习GraphQL

1. GraphQL查询总是能准确获得你想要的数据，不多不少，所以返回的结果是可预测的， 不再像你使用 REST 那样过度获取信息。

2. 它为我们提供了同一个端，对于同一个 API，没有版本2或版本3。给 GraphQL API 添加字段和类型而无需影响现有查询，老旧字段可以废弃，从工具中隐藏。

3. GraphQL是强类型的，通过它，可以在执行之前验证 GraphQL 类型系统中的查询， 它帮助我们构建更强大的 Api。

## 快速开始

```
npm  init -y
npm i graphpack -D

# package.json

"scripts": {
    "dev": "graphpack",
    "build": "graphpack build"
}

# src/shema.graphpql

type Query {
  hello: String
}

# src/resolve.js

const resolvers = {
  Query: {
    hello: () => "Hello World!"
  }
};

export default resolvers;


npm run dev

# 浏览器输入

# Write your query or mutation here
query {
  hello
}
```
## 定义User,query

```
# db.js

export let users = [
  { id: 1, name: "John Doe", email: "john@gmail.com", age: 22 },
  { id: 2, name: "Jane Doe", email: "jane@gmail.com", age: 23 }
];
# resolve.js

import { users } from "./db";

const resolvers = {
  Query: {
    user: (parent, { id }, context, info) => {
      return users.find(user => user.id == id);
    },
    users: (parent, args, context, info) => {
      return users;
    }
  }
};

export default resolvers;
# schema.graphql
type User {
  id: ID!
  name: String!
  email: String!
  age: Int
}

type Query {
  users: [User!]!
  user(id: ID!): User!
}
# 浏览器输入
query {
  users {
  id
  age
  name
  email
 }
}

# or

query {
 	user(id:1) {
    age
    name
    email
  }
}
```
## mutation

```
type Mutation {
  createUser(id: ID!, name: String!, email: String!, age: Int): User!
  updateUser(id: ID!, name: String, email: String, age: Int): User!
  deleteUser(id: ID!): User!
}

Mutation: {
    createUser: (parent, { id, name, email, age }, context, info) => {
      const newUser = { id, name, email, age };

      users.push(newUser);

      return newUser;
    },
    updateUser: (parent, { id, name, email, age }, context, info) => {
      let newUser = users.find(user => user.id === id);

      newUser.name = name;
      newUser.email = email;
      newUser.age = age;

      return newUser;
    },
    deleteUser: (parent, { id }, context, info) => {
      const userIndex = users.findIndex(user => user.id === id);

      if (userIndex === -1) throw new Error("User not found.");

      const deletedUsers = users.splice(userIndex, 1);

      return deletedUsers[0];
    }
  }
```
