**Section 4: Implementing Authentication using Passport.js**

In this section, we will set up the authentication using Passport.js with the username and password strategy.

**Step 4.1: Install Passport.js and required dependencies**

Run the following command to install Passport.js, the local strategy, and other required dependencies:

```bash
npm install passport passport-local bcryptjs express-session
```

**Step 4.2: Configure Passport.js**

Create a new file `config/passport.js` and add the following code to set up the local strategy:

```javascript
const LocalStrategy = require('passport-local').Strategy;
const bcrypt = require('bcryptjs');
const { User } = require('../models');

module.exports = (passport) => {
  passport.use(
    new LocalStrategy({ usernameField: 'username' }, async (username, password, done) => {
      try {
        const user = await User.findOne({ where: { username } });

        if (!user) {
          return done(null, false, { message: 'Incorrect username.' });
        }

        if (!bcrypt.compareSync(password, user.password)) {
          return done(null, false, { message: 'Incorrect password.' });
        }

        return done(null, user);
      } catch (err) {
        return done(err);
      }
    })
  );

  passport.serializeUser((user, done) => {
    done(null, user.id);
  });

  passport.deserializeUser(async (id, done) => {
    try {
      const user = await User.findByPk(id);
      done(null, user);
    } catch (err) {
      done(err);
    }
  });
};
```

**Step 4.3: Initialize and configure Passport.js in the Express app**

Open `app.js` and add the following code to set up the session, initialize Passport.js, and configure it:

```javascript

// after the logger and before the router imports
const session = require('express-session');
const passport = require('passport');
require('./config/passport')(passport);

// ...

// add this right after the middleware using `express.static()`
// just before the router middleware
app.use(
  session({
    secret: 'your_secret_key',
    resave: false,
    saveUninitialized: false,
  })
);
app.use(passport.initialize());
app.use(passport.session());

// ...
```

Replace `'your_secret_key'` with a suitable secret key for your application.

**Step 4.4: Create login and register routes**

Create a new file `routes/auth.js` and add the following code to set up the login and register routes:

```javascript
const express = require("express");
const router = express.Router();
const bcrypt = require("bcryptjs");
const passport = require("passport");
const { User } = require("../models");

router.post("/register", async (req, res) => {
  const { username, password } = req.body;
  if (!username || !password) {
    return res
      .status(400)
      .json({ message: "Username and password are both required." });
  }
  try {
    const hashedPassword = bcrypt.hashSync(password, 10);
    await User.create({ username, password: hashedPassword });
    res.sendStatus(201);
  } catch (err) {
    res.status(500).send(err);
  }
});

router.post("/login", (req, res, next) => {
  passport.authenticate("local", (err, user, info) => {
    if (err) {
      return res.status(500).json({
        message: "An error occurred during authentication",
        error: err,
      });
    }

    if (!user) {
      return res.status(401).json({ message: "There was an error with the user" });
    }

    req.login(user, (err) => {
      if (err) {
        return res
          .status(500)
          .json({ message: "An error occurred during login", error: err });
      }

      return res.status(200).json({ message: "Login successful" });
    });
  })(req, res, next);
});

router.post("/logout", (req, res) => {
  req.logout((error) => {
    if (error) return next(error);
  });
  res.status(200).json({ message: "Logout successful" });
});

module.exports = router;

```

Now, register the `auth` routes in your `app.js`:

```javascript
const authRouter = require('./routes/auth');

// ...

app.use('/auth', authRouter);

// ...
```

With this setup, we have implemented the authentication using Passport.js. Users can now register and log in using their username and password.

In the next section, we will create RESTful API endpoints for our resources.
