# mern-template
quick build of MERN template

### Setting a Project folder (MERN)
```bash
mkdir MERN
```

### Setting the Frontend (REACT)
```bash
# inside MERN
npx create-react-app client
```

### Setting the Backend (Express)
```bash
# inside MERN
mkdir server
cd server & npm init -y
npm install mongdb express cors dotenv nodemon

touch index.js
```

### Setting server, routes, database in the Backend
```js
// MERN/server/index.js
const express = require("express");
const app = express();
const cors = require("cors");
require("dotenv").config({ path: "./config.env" });
const dbo = require("./db/conn");
const port = process.env.PORT;

app.use(express.urlencoded({ extended: true }));
app.use(express.json());
app.use(cors());

const indexRouter = require('./routes/index');
const usersRouter = require('./routes/users');

app.use('/api', indexRouter);
app.use('/api/users', usersRouter);

app.listen(port, () => {
  dbo.connectDB(function (err) {
    if (err) console.error(err);
 
  });
  console.log(`Server is running on port: ${port}`);
});
```

### Enviroment variable setup (PORT & DB)
Create a DB at https://www.mongodb.com/atlas/database
```bash
# MERN/server/config.env
PORT=4000
ATLAS_URI=mongodb+srv://<username>:<password>@sandbox.jadwj.mongodb.net/<db-name>?retryWrites=true&w=majority
```

### Databaes Connection
```js
// MERN/routes/db-connect
const { MongoClient } = require("mongodb");
const Db = process.env.ATLAS_URI;
const client = new MongoClient(Db, {
  useNewUrlParser: true,
  useUnifiedTopology: true,
});
 
var _db;
 
module.exports = {
  connectToServer: function (callback) {
    client.connect(function (err, db) {
      // Verify we got a good "db" object
      if (db)
      {
        _db = db.db("employees");
        console.log("Successfully connected to MongoDB."); 
      }
      return callback(err);
         });
  },
 
  getDb: function () {
    return _db;
  },
};
```


### Setting User Management Route and Endpoints
// MERN/server/routes/users.js
See code
