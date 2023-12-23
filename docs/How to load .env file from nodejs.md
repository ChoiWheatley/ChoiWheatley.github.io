---
aliases: 
tags: 
description:
title: How to load .env file from nodejs
created: 2023-12-23T21:49:30
updated: 2023-12-23T21:49:48
---
- [[0018 Javascript ☕️]]
___

## AI Response

To load a `.env` file in a Node.js application, you can use the `dotenv` package. Here's a step-by-step guide:

1. Install the `dotenv` package using npm or yarn:

   ```bash
   npm install dotenv
   ```

   or

   ```bash
   yarn add dotenv
   ```

2. Create a `.env` file in the root directory of your project and define your environment variables:

   ```env
   DB_HOST=localhost
   DB_USER=myuser
   DB_PASSWORD=mypassword
   ```

3. In your Node.js application entry point (typically `index.js` or `app.js`), require and configure `dotenv` at the very beginning:

   ```javascript
   require('dotenv').config();
   ```

4. Now, you can access your environment variables using `process.env`:

   ```javascript
   const dbHost = process.env.DB_HOST;
   const dbUser = process.env.DB_USER;
   const dbPassword = process.env.DB_PASSWORD;

   console.log(`Database Host: ${dbHost}`);
   console.log(`Database User: ${dbUser}`);
   console.log(`Database Password: ${dbPassword}`);
   ```

Ensure that you do not commit your `.env` file to version control systems, as it often contains sensitive information.

By following these steps, your Node.js application will load the environment variables from the `.env` file during runtime.
