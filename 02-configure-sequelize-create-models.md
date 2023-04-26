**Section 2: Configuring Sequelize and Creating Models**

In this section, we will configure Sequelize to use PostgreSQL as our database and create the required models for our application.

**Step 2.1: Initialize Sequelize**

Run the following command to initialize Sequelize and create the necessary configuration files:

```bash
npx sequelize-cli init
```

**Step 2.2: Update the Sequelize configuration**

Change the `config/config.json` filename to `config/config.js` file and update the configuration for your PostgreSQL database. 
Install `dotenv` so that you can use environment variables in the `config.js` file.

```bash
npm install dotenv
```

Replace the `username`, `password`, `database`, `host`, and `dialect` with the appropriate environment variables for your system. You also need to require `dotenv` as well as export this configuration object.

```javascript
require('dotenv').config()

module.exports = {
  "development": {
    "username": process.env.DEVELOPMENT_DATABASE_USER,
    "password": process.env.DEVELOPMENT_DATABASE_PASSWORD, //probably null
    "database": process.env.DEVELOPMENT_DATABASE_NAME,
    "host": "127.0.0.1",
    "dialect": "postgres"
  },
//   ...
}
```

In order for these environment variables to get assigned, you need to create a file named `.env` at the root of your application with values for the above variable

```
DEVELOPMENT_DATABASE_USER="username"
DEVELOPMENT_DATABASE_PASSWORD="password"
DEVELOPMENT_DATABASE_NAME="database_name"
```

**Step 2.3: Create the models**

We will now create the models for `baskets`, `items`, `users`, and `orders`. Run the following commands to generate the required models:

```bash
npx sequelize-cli model:generate --name Basket --attributes name:string,price:float
npx sequelize-cli model:generate --name Item --attributes name:string,price:float
npx sequelize-cli model:generate --name User --attributes username:string,password:string
npx sequelize-cli model:generate --name Order --attributes user_id:integer,ordered_date:date
```

**Step 2.4: Define associations**

Open each model file in the `models` directory and define the appropriate associations between the models:

In `models/basket.js`, add the following associations:

```javascript
class Basket extends Model {
  static associate(models) {
    Basket.belongsToMany(models.Item, {
      through: 'BasketItems',
      foreignKey: 'basket_id',
    });
  }
}
```

In `models/item.js`, add the following associations:

```javascript
class Item extends Model {
  static associate(models) {
    Item.belongsToMany(models.Basket, {
      through: 'BasketItems',
      foreignKey: 'item_id',
    });
  }
}
```

In `models/user.js`, add the following associations:

```javascript
class User extends Model {
  static associate(models) {
    User.hasMany(models.Order, {
      foreignKey: 'user_id',
    });
  }
}
```

In `models/order.js`, add the following associations:

```javascript
class Order extends Model {
  static associate(models) {
    Order.belongsTo(models.User, {
      foreignKey: 'user_id',
    });
    Order.belongsToMany(models.Basket, {
      through: 'OrderBaskets',
      foreignKey: 'order_id',
    });
  }
}
```

By configuring the models with associations we have used JavaScript to create a mapping for the database schema. In other words, we are bringing the ERD to life through code. 

Our next step is running migrations to create the database schema which will actually modify the database. Then we will seed the database with data.

[Next Section](03-migrations-and-seeders.md)