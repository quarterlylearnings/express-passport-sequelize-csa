**Section 5: Creating RESTful API Endpoints**

In this section, we will create the RESTful API endpoints for each of our resources: `baskets`, `items`, `users`, and `orders`.

**Step 5.1: Create API routes for each resource**

Create new route files for each resource in the `routes` directory:

- `routes/baskets.js`
- `routes/items.js`
- `routes/users.js`
- `routes/orders.js`

For each file, add the basic structure for an Express router:

```javascript
const express = require("express");
const router = express.Router();

// Add your resource-specific routes here

module.exports = router;
```

**Step 5.2: Add CRUD operations for each resource**

Add the necessary CRUD operations to each router file, using the Sequelize models to interact with the database.

For example, in `routes/baskets.js`, add the following CRUD operations:

```javascript
const { Basket, BasketItem, Item } = require("../models");

// Create a new basket
router.post("/", async (req, res) => {
  try {
    const newBasket = {
      name: req.body.name,
      price: req.body.price,
    };
    const basket = await Basket.create(newBasket);
    res.status(201).json(basket);
  } catch (error) {
    res.status(500).json({ message: "Error creating basket", error });
  }
});

// Get all baskets, including associated items
router.get("/", async (req, res) => {
  try {
    const baskets = await Basket.findAll(); // how can we include the ITEMS associated with the baskets in this response?
    res.json(baskets);
  } catch (error) {
    res.status(500).json({ message: "Error retrieving baskets", error });
  }
});

// Get a specific basket by ID, including associated items
router.get("/:id", async (req, res) => {
  try {
    const basket = await Basket.findByPk(req.params.id); // how can we include the ITEMS associated with the baskets in this response?

    if (!basket) {
      res.status(404).json({ message: "Basket not found" });
    } else {
      res.json(basket);
    }
  } catch (error) {
    res.status(500).json({ message: "Error retrieving basket", error });
  }
});

// Update a basket by ID
router.put("/:id", async (req, res) => {
  const { name, price } = req.body;

  try {
    const newBasket = {};
    if (name !== undefined) {
      newBasket.name = name;
    }
    if (price !== undefined) {
      newBasket.price = price;
    }
    const [updated] = await Basket.update(newBasket, {
      where: { id: req.params.id },
    });

    if (updated) {
      const updatedBasket = await Basket.findByPk(req.params.id);
      res.json(updatedBasket);
    } else {
      res.status(404).json({ message: "Basket not found" });
    }
  } catch (error) {
    res.status(500).json({ message: "Error updating basket", error });
  }
});

// Delete a basket by ID
router.delete("/:id", async (req, res) => {
  try {
    const deleted = await Basket.destroy({
      where: { id: req.params.id },
    });

    if (deleted) {
      res.status(204).json({ message: "Basket deleted" });
    } else {
      res.status(404).json({ message: "Basket not found" });
    }
  } catch (error) {
    res.status(500).json({ message: "Error deleting basket", error });
  }
});
```

For the rest of the routes you will have to implement different methods to be able to perform CRUD operations using Sequelize following the pattern of the `/baskets` route above.

- There is a known bug in Sequelize where snake_cased field names create bugs when trying to do associations. See if you can work through this bug in making associations work for your routes.

**Step 5.3: Register resource routes in `app.js`**

Register the new route files for each resource in your `app.js`:

```javascript
const basketsRouter = require("./routes/baskets");
const itemsRouter = require("./routes/items");
const usersRouter = require("./routes/users");
const ordersRouter = require("./routes/orders");

// ...

app.use("/baskets", basketsRouter);
app.use("/items", itemsRouter);
app.use("/users", usersRouter);
app.use("/orders", ordersRouter);
```

**Step 5.4: Protect API routes with authentication**

To protect your API routes and ensure only authenticated users can access them, you can add a middleware function to check if the user is authenticated. If not, return a `401 Unauthorized` status.

Create a new directory called `middlewares/` and add a file `auth.js` to it and add the following code:

`middlewares/auth.js`

```javascript
const passport = require("passport");

function authenticate(req, res, next) {
  passport.authenticate("local", (error, user) => {
    if (error) {
      return next(error);
    }

    if (!user) {
      return res
        .status(401)
        .json({ message: "Incorrect username or password." });
    }

    req.logIn(user, (error) => {
      if (error) {
        return next(error);
      }
      next();
    });
  })(req, res, next);
}

module.exports = { authenticate };
```

Now, import the `authenticate` middleware in your resource route files and use it to protect the routes:

```javascript
const { authenticate } = require("../middlewares/auth");

// ...

router.post("/", authenticate, async (req, res) => {
  // Route implementation
});

// Apply the middleware to other routes as needed
```

Apply this middleware to the routes that require authentication in the `baskets`, `items`, `users`, and `orders` route files.

You have now created RESTful API endpoints for each resource, allowing clients to perform CRUD operations on `baskets`, `items`, `users`, and `orders`. Additionally, you have protected the routes with authentication, ensuring only authorized users can access them.

At this point, you should have a fully functional API for our Community-Supported Agriculture application. Test the API using Postman or curl to ensure everything is working as expected.

🤜🏾 🤛🏾 Big up yourself!

[Run it back!](01-setup.md)
