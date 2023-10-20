# immutable-passport

Integrating Immutable Passport into your application involves several steps. In this guide, we'll walk through each step in detail, including creating a simple application, registering it on the Immutable Developer Hub, installing and initializing the Passport client, logging in a user, displaying the tokens and user's nickname, logging out a user, and initiating a transaction from Passport. We'll use JavaScript and Node.js for this guide.

### Prerequisites:
Before you begin, make sure you have the following prerequisites:

1. Node.js and npm (Node Package Manager) installed on your system.
2. A code editor (e.g., Visual Studio Code).
3. An Immutable Developer Hub account.

Let's get started:

### Step 1: Create a Simple Application

You can either create a new application or clone a repository for a simple application. For this guide, let's create a new directory and initialize a new Node.js application:

```bash
mkdir passport-integration
cd passport-integration
npm init -y
```

### Step 2: Register Your Application on Immutable Developer Hub

1. Visit the Immutable Developer Hub (https://developer.immutable.com/).

2. Log in to your account or create one if you haven't already.

3. In the Developer Hub, create a new application.

4. Note down the `Client ID` and `Client Secret` provided by Immutable. You'll need these later.

### Step 3: Installing and Initializing the Passport Client

In your project directory, install the Immutable Passport client:

```bash
npm install @immutable/passport
```

Next, create a JavaScript file (e.g., `app.js`) and initialize the Passport client with your `Client ID`:

```javascript
// app.js

const passport = require('@immutable/passport');

const clientID = 'YOUR_CLIENT_ID';

passport.initialize({
  clientID,
  // ...other options if needed
});
```

### Step 4: Logging in a User with Passport

To log in a user, create a route in your application that redirects users to the Immutable Passport login page:

```javascript
// app.js

const express = require('express');
const app = express();

app.get('/login', (req, res) => {
  const loginURL = passport.getAuthorizationURL({ scope: 'openid profile email' });
  res.redirect(loginURL);
});

app.listen(3000, () => {
  console.log('Server started on http://localhost:3000');
});
```

When users visit `/login`, they will be redirected to the Immutable Passport login page.

### Step 5: Displaying Tokens and User's Nickname

After the user logs in and is redirected back to your application, you can obtain the ID token, access token, and user's nickname.

```javascript
// app.js

app.get('/callback', async (req, res) => {
  try {
    const tokens = await passport.authenticate(req.query);
    const { id_token, access_token } = tokens;

    // Use id_token, access_token, and user info
    const userInfo = await passport.getUserInfo(id_token);
    const nickname = userInfo.nickname;

    res.send(`ID Token: ${id_token}<br>Access Token: ${access_token}<br>Nickname: ${nickname}`);
  } catch (error) {
    res.status(500).send('Error: ' + error.message);
  }
});
```

### Step 6: Logging Out a User

To log out a user, you can create a route that clears the session and redirects to the Immutable Passport logout URL:

```javascript
// app.js

app.get('/logout', (req, res) => {
  req.session = null; // Clear session
  const logoutURL = passport.getLogoutURL();
  res.redirect(logoutURL);
});
```

### Step 7: Initiating a Transaction from Passport

To initiate a transaction, you can use the Passport client to send a transaction request:

```javascript
// app.js

app.get('/send-transaction', async (req, res) => {
  try {
    const transactionRequest = {
      /* Your transaction request parameters */
    };
    const transactionHash = await passport.sendTransaction(transactionRequest);
    res.send(`Transaction Hash: ${transactionHash}`);
  } catch (error) {
    res.status(500).send('Error: ' + error.message);
  }
});
```

Make sure to replace `/* Your transaction request parameters */` with the actual transaction details you want to send.

Now, your application is integrated with Immutable Passport, allowing users to log in, log out, and initiate transactions using Immutable Passport services. Remember to secure your application, handle errors gracefully, and follow best practices for production deployments.
