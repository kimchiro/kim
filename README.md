# kim
const http = require("http");
const { DataSource } = require("typeorm");
const express = require("express");

const app = express();

const dotenv = require("dotenv");
dotenv.config();


const myDataSource = new DataSource({
  type: process.env.TYPEORM_CONNECTION,
  host: process.env.TYPEORM_HOST,
  port: process.env.TYPEORM_PORT,
  username: process.env.TYPEORM_USERNAME,
  password: process.env.TYPEORM_PASSWORD,
  database: process.env.TYPEORM_DATABASE,
});


app.use(express.json()); // for parsing application/json

const users = [
  {
    id: 1,
    name: "Rebekah Johnson",
    email: "Glover12345@gmail.com",
    password: "123qwe",
  },
  {
    id: 2,
    name: "Fabian Predovic",
    email: "Connell29@gmail.com",
    password: "password",
  },
];

app.get("/", async (req, res) => {
  try {
    return res.status(200).json({ message: "Welcome to Soheon's server!" });
  } catch (err) {
    console.log(err);
  }
});

//1. API 로 users 화면에 보여주기
app.get("/users", async (req, res) => {
  try {
    const userData = await myDataSource.query(
      "SELECT id, name, email FROM USERS"
    );
    return res.status(200).json({
      users: users,
    });
  } catch (error) {
    console.log(error);
  }
});
//2. users 생성

app.post("/createUsers", async (req, res) => {
  try {
    const me = req.body;

    console.log("ME: ", me);

    const name2 = me.name;
    const password2 = me.password;
    const email2 = me.email;

    const userData = await myDataSource.query(`
      INSERT INTO users (
        name, 
        password,
        email
      )
      VALUES (
        '${name2}',
        '${password2}', 
        '${email2}'
      )
    `);

    console.log("iserted user id", userData, inserId);

    users.push(me);

    console.log("USERS: ", users);

    return res.status(201).json({
      users: users,
    });
  } catch (err) {
    console.log(err);
  }
});

// 과제 3 DELETE
// 가장 마지막 user를 삭제하는 엔드포인트
app.delete("/deleteUsers", async (req, res) => {
  try {
    users.pop();
    return res.status(204).json({ Delete: "Last User" });
  } catch (err) {
    console.log(err);
  }
});

// 과제 4 UPDATE
// 1번 user의 이름을 'Code Kim'으로 바꾸어 보세요.

app.put("/putUser/", async (req, res) => {
  try {
    const newName = req.body;

    users[0].name = newName.name;
    // users[0].name = newName
    return res.status(201).json({ "slice name": "Kim Code" });
  } catch (err) {
    console.log(err);
  }
});

const server = http.createServer(app); // express app 으로 서버를 만듭니다.

const start = async () => {
  // 서버를 시작하는 함수입니다.
  try {
    server.listen(8080, () => console.log(`Server is listening on 8000`));
  } catch (err) {
    console.error(err);
  }
};

myDataSource.initialize().then(() => {
  console.log("Data Source has been initialized!");
});

start();
# Readme
# Readme
