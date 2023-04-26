**Section 3: Creating and Running Migrations and Seeders**

In this section, we will create and run migrations for our models, and then create seeders to generate sample data for our application.

**Step 3.1: Create migrations**

Run the following commands to generate the required migrations for our join tables:

```bash
npx sequelize-cli migration:generate --name create-basket-items
npx sequelize-cli migration:generate --name create-order-baskets
```

Now, open the newly created migration files for the join tables in the `migrations` directory and define the appropriate up and down methods:

In `migrations/YYYYMMDDHHmmss-create-basket-items.js`, add:

```javascript
'use strict';
module.exports = {
  up: async (queryInterface, Sequelize) => {
    await queryInterface.createTable('BasketItems', {
      basket_id: {
        type: Sequelize.INTEGER,
        allowNull: false,
        references: {
          model: 'Baskets',
          key: 'id',
        },
      },
      item_id: {
        type: Sequelize.INTEGER,
        allowNull: false,
        references: {
          model: 'Items',
          key: 'id',
        },
      },
    });
  },
  down: async (queryInterface, Sequelize) => {
    await queryInterface.dropTable('BasketItems');
  },
};
```

In `migrations/YYYYMMDDHHmmss-create-order-baskets.js`, add:

```javascript
'use strict';
module.exports = {
  up: async (queryInterface, Sequelize) => {
    await queryInterface.createTable('OrderBaskets', {
      order_id: {
        type: Sequelize.INTEGER,
        allowNull: false,
        references: {
          model: 'Orders',
          key: 'id',
        },
      },
      basket_id: {
        type: Sequelize.INTEGER,
        allowNull: false,
        references: {
          model: 'Baskets',
          key: 'id',
        },
      },
    });
  },
  down: async (queryInterface, Sequelize) => {
    await queryInterface.dropTable('OrderBaskets');
  },
};
```

**Step 3.2: Run migrations**

If you haven't created the database yet, you can do so either using `psql`:

```bash
createdb database_name
```

OR

You can let the `sequelize-cli` create it for you:

```bash
npx sequelize-cli db:create
```

Run the following command to apply the migrations to your PostgreSQL database:

```bash
npx sequelize-cli db:migrate
```


**Step 3.3: Create seeders**

- _This step is optional. You may opt to create all of your application data via the API using the POST routes you will soon develop._

Run the following commands to generate the seeders for our models:

```bash
npx sequelize-cli seed:generate --name demo-items
npx sequelize-cli seed:generate --name demo-baskets
npx sequelize-cli seed:generate --name demo-users
npx sequelize-cli seed:generate --name demo-orders
```

Open the newly created seeder files in the `seeders` directory and add the appropriate sample data:

In `seeders/YYYYMMDDHHmmss-demo-items.js`, add:

```javascript
'use strict';

module.exports = {
  up: async (queryInterface, Sequelize) => {
    await queryInterface.bulkInsert('Items', [
      { name: 'Carrot', price: 0.99, createdAt: new Date(), updatedAt: new Date() },
      { name: 'Tomato', price: 1.50, createdAt: new Date(), updatedAt: new Date() },
      { name: 'Spinach', price: 3.00, createdAt: new Date(), updatedAt: new Date() },
      { name: 'Kale', price: 2.50, createdAt: new Date(), updatedAt: new Date() },
      { name: 'Apple', price: 1.20, createdAt: new Date(), updatedAt: new Date() },
      { name: 'Orange', price: 1.00, createdAt: new Date(), updatedAt: new Date() },
      { name: 'Grapes', price: 2.00, createdAt: new Date(), updatedAt: new Date() },
      { name: 'Strawberry', price: 2.50, createdAt: new Date(), updatedAt: new Date() },
      { name: 'Basil', price: 1.00, createdAt: new Date(), updatedAt: new Date() },
      { name: 'Mint', price: 1.00, createdAt: new Date(), updatedAt: new Date() },
      { name: 'Parsley', price: 1.00, createdAt: new Date(), updatedAt: new Date() },
      { name: 'Lettuce', price: 1.50, createdAt: new Date(), updatedAt: new Date() },
      { name: 'Cucumber', price: 1.00, createdAt: new Date(), updatedAt: new Date() },
      { name: 'Broccoli', price: 2.00, createdAt: new Date(), updatedAt: new Date() },
      { name: 'Cauliflower', price: 2.50, createdAt: new Date(), updatedAt: new Date() },
    ]);
  },

  down: async (queryInterface, Sequelize) => {
    await queryInterface.bulkDelete('Items', null, {});
  },
};
```

In `seeders/YYYYMMDDHHmmss-demo-baskets.js`, add:

```javascript
'use strict';

module.exports = {
  up: async (queryInterface, Sequelize) => {
    await queryInterface.bulkInsert('Baskets', [
      { name: 'Vitality Boost', price: 10.00, createdAt: new Date(), updatedAt: new Date() },
      { name: 'Energy Pack', price: 12.00, createdAt: new Date(), updatedAt: new Date() },
      { name: 'Immunity Boost', price: 15.00, createdAt: new Date(), updatedAt: new Date() },
      { name: 'Mindful Munchies', price: 9.00, createdAt: new Date(), updatedAt: new Date() },
      { name: 'Green Goodness', price: 14.00, createdAt: new Date(), updatedAt: new Date() },
    ]);
  },

  down: async (queryInterface, Sequelize) => {
    await queryInterface.bulkDelete('Baskets', null, {});
  },
};
```

In `seeders/YYYYMMDDHHmmss-demo-users.js`, add:

```javascript
'use strict';

const bcrypt = require('bcryptjs');

module.exports = {
  up: async (queryInterface, Sequelize) => {
    await queryInterface.bulkInsert('Users', [
      { username: 'SpongeBob', password: bcrypt.hashSync('pineapple123', 10), createdAt: new Date(), updatedAt: new Date() },
      { username: 'Arnold', password: bcrypt.hashSync('heyarnold123', 10), createdAt: new Date(), updatedAt: new Date() },
      { username: 'TommyPickles', password: bcrypt.hashSync('rugrats123', 10), createdAt: new Date(), updatedAt: new Date() },
      { username: 'FoxyBrown', password: bcrypt.hashSync('foxybrown123', 10), createdAt: new Date(), updatedAt: new Date() },
      { username: 'Shaft', password: bcrypt.hashSync('shaft123', 10), createdAt: new Date(), updatedAt: new Date() },
      { username: 'SuperFly', password: bcrypt.hashSync('superfly123', 10), createdAt: new Date(), updatedAt: new Date() },
    ]);
  },

  down: async (queryInterface, Sequelize) => {
    await queryInterface.bulkDelete('Users', null, {});
  },
};
```

In `seeders/YYYYMMDDHHmmss-demo-orders.js`, add:

```javascript
'use strict';

module.exports = {
  up: async (queryInterface, Sequelize) => {
    await queryInterface.bulkInsert('Orders', [
      { user_id: 1, ordered_date: new Date(), createdAt: new Date(), updatedAt: new Date() },
      { user_id: 2, ordered_date: new Date(), createdAt: new Date(), updatedAt: new Date() },
      { user_id: 3, ordered_date: new Date(), createdAt: new Date(), updatedAt: new Date() },
      { user_id: 4, ordered_date: new Date(), createdAt: new Date(), updatedAt: new Date() },
      { user_id: 5, ordered_date: new Date(), createdAt: new Date(), updatedAt: new Date() },
      { user_id: 6, ordered_date: new Date(), createdAt: new Date(), updatedAt: new Date() },
    ]);
  },

  down: async (queryInterface, Sequelize) => {
    await queryInterface.bulkDelete('Orders', null, {});
  },
};
```

**Step 3.4: Run seeders**

Run the following command to seed your PostgreSQL database with the sample data:

```bash
npx sequelize-cli db:seed:all
```

**Step 3.5: Create seeders for join tables**

Run the following commands to generate the required seeders for the join tables:

```bash
npx sequelize-cli seed:generate --name demo-basket-items
npx sequelize-cli seed:generate --name demo-order-baskets
```

Open the newly created seeder files in the `seeders` directory and add the appropriate sample data:

In `seeders/YYYYMMDDHHmmss-demo-basket-items.js`, add:

```javascript
'use strict';

module.exports = {
  up: async (queryInterface, Sequelize) => {
    await queryInterface.bulkInsert('BasketItems', [
      { basket_id: 1, item_id: 1 },
      { basket_id: 1, item_id: 2 },
      { basket_id: 1, item_id: 3 },
      { basket_id: 2, item_id: 4 },
      { basket_id: 2, item_id: 5 },
      { basket_id: 2, item_id: 6 },
      { basket_id: 3, item_id: 7 },
      { basket_id: 3, item_id: 8 },
      { basket_id: 3, item_id: 9 },
      { basket_id: 4, item_id: 10 },
      { basket_id: 4, item_id: 11 },
      { basket_id: 4, item_id: 12 },
      { basket_id: 5, item_id: 13 },
      { basket_id: 5, item_id: 14 },
      { basket_id: 5, item_id: 15 },
    ]);
  },

  down: async (queryInterface, Sequelize) => {
    await queryInterface.bulkDelete('BasketItems', null, {});
  },
};
```

In `seeders/YYYYMMDDHHmmss-demo-order-baskets.js`, add:

```javascript
'use strict';

module.exports = {
  up: async (queryInterface, Sequelize) => {
    await queryInterface.bulkInsert('OrderBaskets', [
      { order_id: 1, basket_id: 1 },
      { order_id: 2, basket_id: 2 },
      { order_id: 3, basket_id: 3 },
      { order_id: 4, basket_id: 4 },
      { order_id: 5, basket_id: 5 },
      { order_id: 6, basket_id: 1 },
    ]);
  },

  down: async (queryInterface, Sequelize) => {
    await queryInterface.bulkDelete('OrderBaskets', null, {});
  },
};
```

**Step 3.6: Run seeders for join tables**

Run the following command to seed your PostgreSQL database with the sample data for the join tables (you will need to use the file path for _your_ seed files for the last two tables):


```bash
npx sequelize-cli db:seed --seed seeders/YYYYMMDDHHmmss-demo-basket-items.js
npx sequelize-cli db:seed --seed seeders/YYYYMMDDHHmmss-demo-order-baskets.js
```


With the migrations applied and the seed data in place, we can now proceed to implement the authentication and expose the RESTful API endpoints for our resources.

If you haven't already, you should take a break now.

[Next Section](04-implementing-authentication.md)