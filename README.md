# DDD


```javascript


const express = require('express');
const mysql = require('mysql');
const bodyParser = require('body-parser');

const app = express();
app.use(bodyParser.json());

// Создание подключения к базе данных
const db = mysql.createConnection({
  host: 'localhost',
  user: 'your_username',
  password: 'your_password',
  database: 'your_database'
});

db.connect((err) => {
  if (err) {
    throw err;
  }
  console.log('База данных подключена!');
});

// Получение всех пользователей
app.get('/users', (req, res) => {
  const sql = 'SELECT * FROM users';
  db.query(sql, (err, result) => {
    if (err) {
      res.send({ error: err.message });
    } else {
      res.send(result);
    }
  });
});

// Добавление нового пользователя
app.post('/users', (req, res) => {
  const { name, email, age } = req.body;
  const sql = 'INSERT INTO users SET ?';
  db.query(sql, { name, email, age }, (err, result) => {
    if (err) {
      res.send({ error: err.message });
    } else {
      res.send('Пользователь успешно добавлен');
    }
  });
});

// Обновление пользователя
app.put('/users/:id', (req, res) => {
  const { name, email, age } = req.body;
  const sql = 'UPDATE users SET ? WHERE id = ?';
  db.query(sql, [req.body, req.params.id], (err, result) => {
    if (err) {
      res.send({ error: err.message });
    } else {
      res.send('Пользователь успешно обновлен');
    }
  });
});

// Удаление пользователя
app.delete('/users/:id', (req, res) => {
  const sql = 'DELETE FROM users WHERE id = ?';
  db.query(sql, req.params.id, (err, result) => {
    if (err) {
      res.send({ error: err.message });
    } else {
      res.send('Пользователь успешно удален');
    }
  });
});

const port = 3000;
app.listen(port, () => console.log(`Сервер запущен на порту ${port}`));

//////////////////////////////


const express = require('express');
const mysql = require('mysql');
const cors = require('cors');
const multer = require('multer');
const upload = multer();
const app = express();
const port = 3000;

const corsOptions = {
  origin: 'http://localhost:3000',
  methods: 'GET,HEAD,PUT,PATCH,POST,DELETE',
  credentials: true,
  optionsSuccessStatus: 204
};

app.use(cors(corsOptions));

// Используем multer для обработки FormData
app.use(upload.array());
app.use(express.static('public'));

const connection = mysql.createConnection({
    host: 'localhost',
    user: 'admin',
    password: '123',
    database: 'testexam',
});

connection.connect((err) => {
    if(err) throw err;
    console.log("Успешно!")
});

app.get('/api/users', (req, res) => {
    connection.query("SELECT * FROM users", (err, results) => {
        if(err) throw err;
        res.json(results)
    })
});

app.post('/register', (req, res) => {
    const username = req.body.name;
    const password = req.body.password;
    console.log('Received data:', username, password);

    // Обработка данных и сохранение в базе данных
    // ...

    res.json({ message: 'Data received' });
});

app.listen(port, () => {
    console.log(`Server is running on http://localhost:${port}/`);
});



регистрация 

const express = require('express');
const mysql = require('mysql');
const multer = require('multer');
const bodyParser = require('body-parser');
const cors = require('cors');

const app = express();
const upload = multer();

app.use(cors());
app.use(bodyParser.json());
app.use(upload.array());

const db = mysql.createConnection({
  host: 'localhost',
  user: 'your_username',
  password: 'your_password',
  database: 'your_database'
});

db.connect((err) => {
  if (err) {
    throw err;
  }
  console.log('Connected to the database.');
});

app.post('/register', (req, res) => {
  const { name, email, password } = req.body;

  if (!name || !email || !password) {
    return res.status(400).json({ message: 'All fields are required.' });
  }

  const sql = 'INSERT INTO users (name, email, password) VALUES (?, ?, ?)';
  const passwordHash = bcrypt.hashSync(password, 10); // Предполагается, что у вас установлена библиотека bcrypt

  db.query(sql, [name, email, passwordHash], (err, result) => {
    if (err) {
      return res.status(500).json({ message: 'Error registering user.' });
    }
    res.status(201).json({ message: 'User registered successfully.' });
  });
});

const port = 3000;
app.listen(port, () => console.log(`Server running on port ${port}`));


////////////////////////////////


app.post('/login', (req, res) => {
  const { email, password } = req.body;

  if (!email || !password) {
    return res.status(400).json({ message: 'Email and password are required.' });
  }

  const sql = 'SELECT * FROM users WHERE email = ?';
  db.query(sql, [email], (err, results) => {
    if (err) {
      return res.status(500).json({ message: 'Error checking credentials.' });
    }

    if (results.length === 0) {
      return res.status(401).json({ message: 'Invalid email or password.' });
    }

    const user = results[0];
    if (!bcrypt.compareSync(password, user.password)) {
      return res.status(401).json({ message: 'Invalid email or password.' });
    }

    // Возвращаем сообщение об успешной авторизации
    res.json({ message: 'Logged in successfully.', userId: user.id, redirectUrl: '/applications' });
  });
});
```
