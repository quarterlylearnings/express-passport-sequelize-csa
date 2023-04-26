**Introduction: Building a Community-Supported Agriculture API with Express, Passport, and Sequelize**

In this lesson, you will learn how to build a RESTful API for a community-supported agriculture application using Express.js, Passport.js, and Sequelize.js. The application will serve `baskets`, `items`, `users`, and `orders` resources, allowing clients to manage and interact with them following the REST paradigm.

Throughout the lesson, you will:

1. Set up an Express.js application using the `express-generator`.
2. Configure a Postgres database and integrate Sequelize.js as the ORM.
3. Utilize `sequelize-cli` commands for creating, migrating, and seeding the database.
4. Create models and associations for `baskets`, `items`, `users`, and `orders`.
5. Implement authentication using Passport.js and the [username and password strategy](https://www.passportjs.org/concepts/authentication/password/).
6. Create RESTful API endpoints for each resource and protect them with authentication.

By the end of this lesson, you will have a fully functional API for managing community-supported agriculture resources. You will gain a deeper understanding of how to build a secure and scalable API using Express.js, Passport.js, and Sequelize.js with a Postgres database.

This lesson assumes you have a basic understanding of JavaScript, Express.js, and SQL databases. Familiarity with Passport.js and Sequelize.js is helpful, but not required.

Ready? Okay.

[Get Started](01-setup.md)