https://disk.yandex.ru/d/pLNbPawSQbGQ7A
https://drive.google.com/drive/folders/19QAXoy0q--JnLhEnVBR9pG0WC_56HDdl?usp=sharing

33. Atbash (PHP)
🔹 Код:
<?php
function atbash($text) {
    $result = "";
    for ($i = 0; $i < strlen($text); $i++) {
        $char = $text[$i];
        if (ctype_alpha($char)) {
            $result .= chr(ord('Z') - (ord($char) - ord('A')));
        } else {
            $result .= $char;
        }
    }
    return $result;
}

echo atbash("HELLOWORLD") . "<br>";
echo atbash("VALIDATOR") . "<br>";
echo atbash("CYBERNETICS");
?>
🔹 Результат:
SVOOLDLIOW
EZORWZGLI
XBYVIMVGRXH
✅ 34. Atbash (PHP)
🔹 Код:

(тот же)

echo atbash("CRYPTOANALYSIS") . "<br>";
echo atbash("ENCODING") . "<br>";
echo atbash("REPLICATION");
🔹 Результат:
XIBKGLZMZOBHRH
VMXLWRMT
IVKORXZGRLM
✅ 35. Caesar (PHP, сдвиг 3)
🔹 Код:
<?php
function caesar($text, $shift) {
    $result = "";
    for ($i = 0; $i < strlen($text); $i++) {
        $char = $text[$i];
        if (ctype_alpha($char)) {
            $result .= chr((ord($char) - ord('A') + $shift) % 26 + ord('A'));
        } else {
            $result .= $char;
        }
    }
    return $result;
}

echo caesar("HELLOWORLD", 3) . "<br>";
echo caesar("VALIDATOR", 3) . "<br>";
echo caesar("CYBERNETICS", 3);
?>
🔹 Результат:
KHOORZRUOG
YDOLGDWRU
FBEHUQHWLFV
✅ 36. Caesar (PHP, сдвиг 2)
🔹 Код:
echo caesar("CRYPTOANALYSIS", 2) . "<br>";
echo caesar("ENCODING", 2) . "<br>";
echo caesar("REPLICATION", 2);
🔹 Результат:
ETARVQCPANCAUKU
GPEQFKPI
TGRNKECVKQP
✅ 37. ROT13 (JavaScript)
🔹 Код:
function rot13(str) {
    return str.replace(/[A-Z]/g, function(c) {
        return String.fromCharCode((c.charCodeAt(0) - 65 + 13) % 26 + 65);
    });
}

console.log(rot13("HELLOWORLD"));
console.log(rot13("VALIDATOR"));
console.log(rot13("CYBERNETICS"));
🔹 Результат:
URYYBJBEYQ
INYVQNGBE
PLOREARGVPF
✅ 38. ROT13 (JavaScript)
🔹 Код:
console.log(rot13("CRYPTOANALYSIS"));
console.log(rot13("ENCODING"));
console.log(rot13("REPLICATION"));
🔹 Результат:
PELCGBNANYLFVF
RAPBQVAT
ERCYVPNGVBA
✅ 39. XOR (PHP)
🔹 Код:
<?php
function xorCipher($text, $key) {
    $result = "";
    for ($i = 0; $i < strlen($text); $i++) {
        $result .= chr(ord($text[$i]) ^ ord($key[$i % strlen($key)]));
    }
    return base64_encode($result); // чтобы читалось
}

echo xorCipher("Hello, World!", "secret") . "<br>";
echo xorCipher("XOR cypher", "key123") . "<br>";
echo xorCipher("Check 2024", "move");
?>
🔹 Результат (пример):
(будет base64 строка)

💬 На экзамене:
👉 «Результат закодирован в base64, так как XOR даёт бинарные данные»

✅ 40. AES (PHP)
🔹 Код:
<?php
$key = "1234567890123456";

function encryptAES($text, $key) {
    return openssl_encrypt($text, "AES-128-ECB", $key);
}

function decryptAES($text, $key) {
    return openssl_decrypt($text, "AES-128-ECB", $key);
}

$enc1 = encryptAES("HELLOWORLD", $key);
$enc2 = encryptAES("VALIDATOR", $key);
$enc3 = encryptAES("CYBERNETICS", $key);

echo $enc1 . "<br>";
echo $enc2 . "<br>";
echo $enc3 . "<br>";

echo decryptAES($enc1, $key);
?>
🔹 Результат:
(зашифрованные строки)
HELLOWORLD
✅ 41. AES (PHP)
🔹 Код:
$enc1 = encryptAES("CRYPTOANALYSIS", $key);
$enc2 = encryptAES("ENCODING", $key);
$enc3 = encryptAES("REPLICATION", $key);

echo $enc1 . "<br>";
echo $enc2 . "<br>";
echo $enc3 . "<br>";

echo decryptAES($enc1, $key);
💡 ВАЖНО (что говорить):
XOR:
побитовое шифрование
результат бинарный → base64
AES:
симметричный алгоритм
используется OpenSSL
ключ 16 байт (AES-128)


42. Аутентификация (PDO + пароль)
🔹 1. SQL (создание таблицы + данные)
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50),
    email VARCHAR(100),
    password VARCHAR(255)
);

👉 Добавляем пользователя (пример):

<?php
$pdo = new PDO("mysql:host=localhost;dbname=test", "root", "");

$hash = password_hash("12345", PASSWORD_DEFAULT);

$pdo->prepare("INSERT INTO users (username, email, password) VALUES (?, ?, ?)")
    ->execute(["admin", "admin@mail.com", $hash]);
?>
🔹 2. login.php (форма + логика)
<?php
$pdo = new PDO("mysql:host=localhost;dbname=test", "root", "");
?>

<form method="POST">
    Login: <input type="text" name="username"><br>
    Password: <input type="password" name="password"><br>
    <button type="submit">Login</button>
</form>

<?php
if ($_POST) {
    $stmt = $pdo->prepare("SELECT * FROM users WHERE username = ?");
    $stmt->execute([$_POST['username']]);

    $user = $stmt->fetch();

    if ($user && password_verify($_POST['password'], $user['password'])) {
        echo "Успешный вход!";
    } else {
        echo "Неверный логин или пароль";
    }
}
?>
🔹 ✔ Результат:
ввод admin / 12345 → Успешный вход
неверный пароль → ошибка
✅ 43. Регистрация (PDO + фильтрация)
🔹 SQL
CREATE TABLE users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    user_name VARCHAR(50),
    user_email VARCHAR(100)
);
🔹 register.php
<?php
$pdo = new PDO("mysql:host=localhost;dbname=test", "root", "");
?>

<form method="POST">
    Name: <input type="text" name="name"><br>
    Email: <input type="text" name="email"><br>
    <button>Register</button>
</form>

<?php
if ($_POST) {
    $name = htmlspecialchars($_POST['name']);
    $email = filter_var($_POST['email'], FILTER_VALIDATE_EMAIL);

    if ($email) {
        $stmt = $pdo->prepare("INSERT INTO users (user_name, user_email) VALUES (?, ?)");
        $stmt->execute([$name, $email]);

        header("Location: index.php");
        exit;
    } else {
        echo "Неверный email";
    }
}
?>
🔹 ✔ Результат:
корректные данные → переход на index.php
плохой email → ошибка
✅ 44. Регистрация (regex проверки)
🔹 Условия:
имя ≥ 6 символов, только буквы
email валидный
🔹 Код:
<?php
$pdo = new PDO("mysql:host=localhost;dbname=test", "root", "");
?>

<form method="POST">
    Name: <input name="name"><br>
    Email: <input name="email"><br>
    <button>Register</button>
</form>

<?php
if ($_POST) {
    $name = $_POST['name'];
    $email = $_POST['email'];

    if (!preg_match("/^[a-zA-Z]{6,}$/", $name)) {
        echo "Имя должно быть только из букв и не менее 6 символов";
        exit;
    }

    if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
        echo "Неверный email";
        exit;
    }

    $stmt = $pdo->prepare("INSERT INTO users (user_name, user_email) VALUES (?, ?)");
    $stmt->execute([$name, $email]);

    header("Location: index.php");
}
?>
🔹 ✔ Результат:
Ivanov → ок
123 → ошибка
✅ 45. Регистрация (сложный пароль)
🔹 Условия:
пароль 8–16
1 заглавная
1 цифра
_ допустим
username ≥5
🔹 SQL:
CREATE TABLE users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    user_name VARCHAR(50),
    user_password VARCHAR(255)
);
🔹 Код:
<?php
$pdo = new PDO("mysql:host=localhost;dbname=test", "root", "");
?>

<form method="POST">
    Name: <input name="name"><br>
    Password: <input name="password"><br>
    <button>Register</button>
</form>

<?php
if ($_POST) {
    $name = $_POST['name'];
    $password = $_POST['password'];

    if (!preg_match("/^[a-zA-Z0-9]{5,}$/", $name)) {
        echo "Имя должно быть не менее 5 символов";
        exit;
    }

    if (!preg_match("/^(?=.*[A-Z])(?=.*\d)[A-Za-z\d_]{8,16}$/", $password)) {
        echo "Пароль не соответствует требованиям";
        exit;
    }

    $hash = password_hash($password, PASSWORD_DEFAULT);

    $stmt = $pdo->prepare("INSERT INTO users (user_name, user_password) VALUES (?, ?)");
    $stmt->execute([$name, $hash]);

    header("Location: index.php");
}
?>
🔹 ✔ Результат:
Pass1234 → ок
pass → ошибка
✅ 46. Проверка возраста
🔹 Код:
<form method="POST">
    Дата рождения (DD:MM:YYYY):
    <input name="date">
    <button>Проверить</button>
</form>

<?php
if ($_POST) {
    $date = $_POST['date'];

    if (!preg_match("/^\d{2}:\d{2}:\d{4}$/", $date)) {
        echo "Неверный формат";
        exit;
    }

    list($d, $m, $y) = explode(":", $date);
    $age = date("Y") - $y;

    if ($age >= 18) {
        echo "Уже можно!";
    } else {
        echo "Извините, вам еще нужно подрасти!";
    }
}
?>
🔹 ✔ Результат:
01:01:2000 → Уже можно
01:01:2010 → подрасти
✅ 47. Регистрация (email + пароль + повтор)
🔹 SQL:
CREATE TABLE users (
    user_id INT AUTO_INCREMENT PRIMARY KEY,
    user_email VARCHAR(100),
    user_password VARCHAR(255)
);
🔹 Код:
<?php
$pdo = new PDO("mysql:host=localhost;dbname=test", "root", "");
?>

<form method="POST">
    Email: <input name="email"><br>
    Password: <input name="password"><br>
    Repeat: <input name="repeat"><br>
    <button>Register</button>
</form>

<?php
if ($_POST) {
    $email = $_POST['email'];
    $password = $_POST['password'];
    $repeat = $_POST['repeat'];

    if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
        echo "Неверный email";
        exit;
    }

    if (strlen($password) < 8 || !preg_match("/[0-9]/", $password)) {
        echo "Пароль должен быть не менее 8 символов и содержать цифру";
        exit;
    }

    if ($password !== $repeat) {
        echo "Пароли не совпадают";
        exit;
    }

    $hash = password_hash($password, PASSWORD_DEFAULT);

    $stmt = $pdo->prepare("INSERT INTO users (user_email, user_password) VALUES (?, ?)");
    $stmt->execute([$email, $hash]);

    echo "Поздравляю, вы зарегистрированы!";
}
?>

48. Поиск товаров (фильтрация)
🔹 SQL
CREATE TABLE products (
    product_id INT AUTO_INCREMENT PRIMARY KEY,
    product_price DECIMAL(10,2),
    product_instock BOOLEAN,
    product_category VARCHAR(50)
);

INSERT INTO products (product_price, product_instock, product_category) VALUES
(100, 1, 'tech'),
(200, 0, 'tech'),
(50, 1, 'food'),
(300, 1, 'tech'),
(20, 1, 'food'),
(500, 0, 'lux'),
(700, 1, 'lux'),
(150, 1, 'tech'),
(80, 0, 'food'),
(60, 1, 'food');
🔹 products.php
<?php
$pdo = new PDO("mysql:host=localhost;dbname=test", "root", "");

$min = $_GET['min'] ?? 0;
$max = $_GET['max'] ?? 1000;
$instock = $_GET['instock'] ?? 1;
$category = $_GET['category'] ?? '';

$sql = "SELECT * FROM products WHERE product_price BETWEEN ? AND ?";
$params = [$min, $max];

if ($category) {
    $sql .= " AND product_category = ?";
    $params[] = $category;
}

$sql .= " AND product_instock = ?";
$params[] = $instock;

$stmt = $pdo->prepare($sql);
$stmt->execute($params);

echo "<table border=1>";
while ($row = $stmt->fetch()) {
    echo "<tr>
        <td>{$row['product_price']}</td>
        <td>{$row['product_category']}</td>
        <td>{$row['product_instock']}</td>
    </tr>";
}
echo "</table>";
?>
🔹 ✔ Результат:

таблица с фильтрованными товарами

✅ 49. Вывод пользователей
🔹 SQL
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50),
    email VARCHAR(100),
    password VARCHAR(255),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
🔹 PHP
<?php
$pdo = new PDO("mysql:host=localhost;dbname=test", "root", "");

$stmt = $pdo->prepare("SELECT username, email FROM users");
$stmt->execute();

echo "<table border=1>";
while ($row = $stmt->fetch()) {
    echo "<tr>
        <td>{$row['username']}</td>
        <td>{$row['email']}</td>
    </tr>";
}
echo "</table>";
?>
🔹 ✔ Результат:

таблица пользователей и email

✅ 50. Вывод пользователей (телефон)
🔹 SQL
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    login VARCHAR(50),
    phone VARCHAR(20),
    password VARCHAR(255),
    age INT
);
🔹 PHP
$stmt = $pdo->prepare("SELECT login, phone FROM users");
$stmt->execute();

echo "<table border=1>";
while ($row = $stmt->fetch()) {
    echo "<tr>
        <td>{$row['login']}</td>
        <td>{$row['phone']}</td>
    </tr>";
}
echo "</table>";
🔹 ✔ Результат:

логин + телефон

✅ 51. Заказы за неделю
🔹 SQL
CREATE TABLE orders (
    order_id INT AUTO_INCREMENT PRIMARY KEY,
    customer_name VARCHAR(50),
    product_name VARCHAR(50),
    quantity INT,
    order_date DATE
);
🔹 PHP
$stmt = $pdo->prepare("
    SELECT * FROM orders 
    WHERE order_date >= DATE_SUB(NOW(), INTERVAL 7 DAY)
");
$stmt->execute();

echo "<table border=1>";
while ($row = $stmt->fetch()) {
    echo "<tr>
        <td>{$row['customer_name']}</td>
        <td>{$row['product_name']}</td>
    </tr>";
}
echo "</table>";
🔹 ✔ Результат:

заказы за последние 7 дней

✅ 52. Имена + товары
🔹 PHP
$stmt = $pdo->prepare("SELECT customer_name, product_name FROM orders");
$stmt->execute();

echo "<table border=1>";
while ($row = $stmt->fetch()) {
    echo "<tr>
        <td>{$row['customer_name']}</td>
        <td>{$row['product_name']}</td>
    </tr>";
}
echo "</table>";
🔹 ✔ Результат:

таблица покупателей и товаров

✅ 53. Книги по дате (новые → старые)
🔹 SQL
CREATE TABLE books (
    book_id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(100),
    author VARCHAR(100),
    price DECIMAL(10,2),
    published_date DATE
);
🔹 PHP
$stmt = $pdo->prepare("
    SELECT * FROM books 
    ORDER BY published_date DESC
");
$stmt->execute();

echo "<table border=1>";
while ($row = $stmt->fetch()) {
    echo "<tr>
        <td>{$row['title']}</td>
        <td>{$row['published_date']}</td>
    </tr>";
}
echo "</table>";
🔹 ✔ Результат:

книги от новых к старым

✅ 54. Книги по автору
🔹 PHP
$stmt = $pdo->prepare("
    SELECT title, author, price 
    FROM books 
    ORDER BY author ASC
");
$stmt->execute();

echo "<table border=1>";
while ($row = $stmt->fetch()) {
    echo "<tr>
        <td>{$row['author']}</td>
        <td>{$row['title']}</td>
        <td>{$row['price']}</td>
    </tr>";
}
echo "</table>";
🔹 ✔ Результат:

отсортировано по автору

✅ 55. Сотрудники с зарплатой > 50000
🔹 SQL
CREATE TABLE employees (
    emp_id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50),
    position VARCHAR(50),
    salary DECIMAL(10,2),
    hire_date DATE
);
🔹 PHP
$stmt = $pdo->prepare("
    SELECT name, salary 
    FROM employees 
    WHERE salary > ?
");
$stmt->execute([50000]);

echo "<table border=1>";
while ($row = $stmt->fetch()) {
    echo "<tr>
        <td>{$row['name']}</td>
        <td>{$row['salary']}</td>
    </tr>";
}
echo "</table>";
🔹 ✔ Результат:

сотрудники с зарплатой выше 50000

✅ 56. Все сотрудники
🔹 PHP
$stmt = $pdo->prepare("SELECT name, position FROM employees");
$stmt->execute();

echo "<table border=1>";
while ($row = $stmt->fetch()) {
    echo "<tr>
        <td>{$row['name']}</td>
        <td>{$row['position']}</td>
    </tr>";
}
echo "</table>";
🔹 ✔ Результат:

все сотрудники

✅ 57. JOIN + добавление товара
🔹 SQL
CREATE TABLE categories (
    category_id INT AUTO_INCREMENT PRIMARY KEY,
    category_name VARCHAR(50)
);

CREATE TABLE products (
    product_id INT AUTO_INCREMENT PRIMARY KEY,
    product_name VARCHAR(50),
    price DECIMAL(10,2),
    category_id INT
);
🔹 PHP (JOIN + INSERT)
<?php
$pdo = new PDO("mysql:host=localhost;dbname=test", "root", "");

// вывод
$stmt = $pdo->prepare("
    SELECT p.product_name, c.category_name
    FROM products p
    JOIN categories c ON p.category_id = c.category_id
");
$stmt->execute();

echo "<table border=1>";
while ($row = $stmt->fetch()) {
    echo "<tr>
        <td>{$row['product_name']}</td>
        <td>{$row['category_name']}</td>
    </tr>";
}
echo "</table>";

// добавление
$pdo->prepare("INSERT INTO products (product_name, price, category_id) VALUES (?, ?, ?)")
    ->execute(["New Product", 100, 1]);
?>
🔹 ✔ Результат:
таблица товаров с категориями
новый товар добавлен
✅ 58. Средний балл (GROUP BY)
🔹 SQL
CREATE TABLE students (
    student_id INT AUTO_INCREMENT PRIMARY KEY,
    student_name VARCHAR(50),
    group_name VARCHAR(50)
);

CREATE TABLE grades (
    grade_id INT AUTO_INCREMENT PRIMARY KEY,
    student_id INT,
    subject VARCHAR(50),
    grade INT
);
🔹 PHP
<?php
$pdo = new PDO("mysql:host=localhost;dbname=test", "root", "");

// средний балл
$stmt = $pdo->prepare("
    SELECT s.student_name, AVG(g.grade) as avg_grade
    FROM students s
    JOIN grades g ON s.student_id = g.student_id
    GROUP BY s.student_id
");
$stmt->execute();

echo "<table border=1>";
while ($row = $stmt->fetch()) {
    echo "<tr>
        <td>{$row['student_name']}</td>
        <td>{$row['avg_grade']}</td>
    </tr>";
}
echo "</table>";

// добавление оценки
$pdo->prepare("INSERT INTO grades (student_id, subject, grade) VALUES (?, ?, ?)")
    ->execute([1, "Math", 5]);
?>
🔹 ✔ Результат:
средний балл студентов
новая оценка добавляется
🔥 ИТОГ (что ты должен понимать)

👉 SELECT + WHERE → фильтрация
👉 ORDER BY → сортировка
👉 JOIN → объединение таблиц
👉 GROUP BY → агрегаты (AVG)
👉 PDO → защита от SQL-инъекций



59. CSRF защита (PHP)
🔹 config.php (генерация + проверка токена)
<?php
session_start();

function generateToken() {
    if (empty($_SESSION['csrf_token'])) {
        $_SESSION['csrf_token'] = bin2hex(random_bytes(32));
    }
    return $_SESSION['csrf_token'];
}

function checkToken($token) {
    return isset($_SESSION['csrf_token']) && hash_equals($_SESSION['csrf_token'], $token);
}

// обновление токена
function refreshToken() {
    $_SESSION['csrf_token'] = bin2hex(random_bytes(32));
}
?>
🔹 form.php
<?php
require 'config.php';
?>

<form method="POST">
    Name: <input name="name"><br>
    Comment: <input name="comment"><br>

    <input type="hidden" name="csrf_token" value="<?php echo generateToken(); ?>">
    <button>Send</button>
</form>

<?php
if ($_POST) {
    if (!checkToken($_POST['csrf_token'])) {
        die("CSRF атака!");
    }

    echo "Данные приняты: " . htmlspecialchars($_POST['name']);

    refreshToken(); // обновление
}
?>
🔹 ✔ Результат:
с правильным токеном → работает
без токена → CSRF атака!
✅ 60. Сессии (безопасность)
🔹 config.php
<?php
session_start();

// защита cookies
session_set_cookie_params([
    'httponly' => true,
    'secure' => false,
    'samesite' => 'Strict'
]);

// регенерация
if (!isset($_SESSION['initiated'])) {
    session_regenerate_id(true);
    $_SESSION['initiated'] = true;
}
?>
🔹 index.php
<?php require 'config.php'; ?>

<form method="POST">
    Name: <input name="name">
    <button>Send</button>
</form>

<?php
if ($_POST) {
    $_SESSION['name'] = $_POST['name'];
    header("Location: hello.php");
}
?>
🔹 hello.php
<?php require 'config.php'; ?>

<?php
if (isset($_SESSION['name'])) {
    echo "Привет, " . htmlspecialchars($_SESSION['name']) . "!";
} else {
    echo "Нет имени";
}
?>
🔹 ✔ Результат:
ввод имени → переход
вывод: Привет, Имя!
✅ 61. Блокировка после 5 попыток
🔹 SQL
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50),
    password VARCHAR(255),
    failed_attempts INT DEFAULT 0,
    last_failed_login TIMESTAMP NULL DEFAULT NULL,
    is_locked BOOLEAN DEFAULT FALSE
);
🔹 login.php
<?php
$pdo = new PDO("mysql:host=localhost;dbname=test", "root", "");

if ($_POST) {
    $username = $_POST['username'];
    $password = $_POST['password'];

    $stmt = $pdo->prepare("SELECT * FROM users WHERE username=?");
    $stmt->execute([$username]);
    $user = $stmt->fetch();

    if ($user['is_locked']) {
        // проверка 10 минут
        if (strtotime($user['last_failed_login']) + 600 < time()) {
            $pdo->prepare("UPDATE users SET is_locked=0, failed_attempts=0 WHERE id=?")
                ->execute([$user['id']]);
        } else {
            die("Аккаунт заблокирован");
        }
    }

    if ($user && password_verify($password, $user['password'])) {
        echo "Успешный вход";

        $pdo->prepare("UPDATE users SET failed_attempts=0 WHERE id=?")
            ->execute([$user['id']]);
    } else {
        $attempts = $user['failed_attempts'] + 1;

        $locked = $attempts >= 5 ? 1 : 0;

        $pdo->prepare("UPDATE users SET failed_attempts=?, is_locked=?, last_failed_login=NOW() WHERE id=?")
            ->execute([$attempts, $locked, $user['id']]);

        echo "Ошибка входа";
    }
}
?>
🔹 ✔ Результат:
5 ошибок → блок
через 10 мин → разблокировка
✅ 62. Upload в БД (PDO)
🔹 SQL
CREATE TABLE files (
    file_id INT AUTO_INCREMENT PRIMARY KEY,
    file_name VARCHAR(100),
    file_type VARCHAR(50),
    file_data LONGBLOB
);
🔹 upload.php
<?php
$pdo = new PDO("mysql:host=localhost;dbname=test", "root", "");
?>

<form method="POST" enctype="multipart/form-data">
    <input type="file" name="file">
    <button>Upload</button>
</form>

<?php
if ($_FILES) {
    $file = $_FILES['file'];

    $allowed = ['image/jpeg', 'image/png', 'image/gif'];

    if (in_array($file['type'], $allowed)) {
        $data = file_get_contents($file['tmp_name']);

        $stmt = $pdo->prepare("INSERT INTO files (file_name, file_type, file_data) VALUES (?, ?, ?)");
        $stmt->execute([$file['name'], $file['type'], $data]);

        echo "Файл загружен";
    } else {
        echo "Неверный формат";
    }
}
?>
🔹 ✔ Результат:
JPG/PNG/GIF → OK
другой файл → ошибка
✅ 63. Форма с полной валидацией
🔹 Код:
<?php
if ($_POST) {
    $username = $_POST['username'];
    $password = $_POST['password'];
    $email = $_POST['email'];
    $phone = $_POST['phone'];

    // username
    if (!preg_match("/^[a-zA-Z0-9_]{3,20}$/", $username)) {
        die("Ошибка username");
    }

    // password
    if (!preg_match("/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[\W]).{8,}$/", $password)) {
        die("Ошибка password");
    }

    // email
    if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
        die("Ошибка email");
    }

    // phone
    if (!preg_match("/^\+7 \(\d{3}\) \d{3}-\d{2}-\d{2}$/", $phone)) {
        die("Ошибка phone");
    }

    echo "Регистрация успешна!";
}
?>
🔹 ✔ Результат:
всё ок → успех
ошибка → сообщение
✅ 64. Студенты + предметы
🔹 SQL
CREATE TABLE students (
    student_id INT AUTO_INCREMENT PRIMARY KEY,
    student_name VARCHAR(50),
    group_name VARCHAR(50)
);

CREATE TABLE grades (
    grade_id INT AUTO_INCREMENT PRIMARY KEY,
    student_id INT,
    subject VARCHAR(50),
    grade INT
);
🔹 PHP
<?php
$pdo = new PDO("mysql:host=localhost;dbname=test", "root", "");

// вывод
$stmt = $pdo->prepare("
    SELECT s.student_name, g.subject
    FROM students s
    JOIN grades g ON s.student_id = g.student_id
");
$stmt->execute();

echo "<table border=1>";
while ($row = $stmt->fetch()) {
    echo "<tr>
        <td>{$row['student_name']}</td>
        <td>{$row['subject']}</td>
    </tr>";
}
echo "</table>";

// добавление предмета
$pdo->prepare("INSERT INTO grades (student_id, subject, grade) VALUES (?, ?, ?)")
    ->execute([1, "Physics", 4]);
?>


1–8. XSS (Reflected / Stored / DOM)
📍 Куда нажимать:
Если XSS через форму:
Открываешь страницу (например XSS Reflected)
Вводишь payload
Нажимаешь кнопку (Submit / Go)

👉 В Burp:

Proxy → Intercept
появится запрос

📌 Дальше:

нажимаешь 👉 Forward
(или просто выключаешь Intercept)
💡 ВАЖНО:

Для XSS Burp почти НЕ нужен
👉 Просто:

вводишь payload
смотришь результат
📍 Если DOM XSS (через URL)
Вставляешь payload в URL
Нажимаешь Enter

👉 Burp:

можно просто Intercept OFF
🟡 9. Insecure CAPTCHA (DVWA)
📍 Шаги:
Включить:
👉 Proxy → Intercept → ON
Вводишь пароль → нажимаешь Submit
В Burp появится запрос
✏️ Что нажимать и менять:
LOW:
найти:
step=1
изменить на:
step=2

👉 нажать:

Forward
MEDIUM:
найти:
step=1
изменить:
step=2&passed_captcha=true

👉 Forward

HIGH:
найти:
g-recaptcha-response=
вписать:
hidd3n valu3
найти:
User-Agent
заменить на:
reCAPTCHA

👉 Forward

🟢 10. IDOR (bWAPP)
📍 Шаги:
Proxy → Intercept ON
Выполняешь действие (например Change Secret)
В Burp:

👉 находишь параметр:

user=...

👉 меняешь:

user=другой_пользователь

👉 нажимаешь:

Forward
🔵 11. JS уязвимости
📍 Способ 1 (Burp):
Intercept ON
Нажимаешь Submit
В запросе:

👉 находишь:

token=

👉 заменяешь на свой

👉 Forward

📍 Способ 2 (без Burp):
F12 → Console
вызываешь функцию:
generate_token()
🟣 12–13. CSRF
📍 Через Burp:
Intercept ON
Отправляешь форму
✏️ Меняешь:
LOW:
password_new=test1

👉 Forward

MEDIUM:

👉 найти:

Referer

👉 изменить на:

http://dvwa.local

👉 Forward

🟠 14–15. CSP
📍 MEDIUM:
Открыть страницу
View Source
Найти <script nonce=...>

👉 просто меняешь payload

📍 HIGH (через Burp):
Intercept ON
Выполняешь действие

👉 в Burp:

найти:
callback=solveSum

👉 заменить:

alert(1)

👉 Forward

⚫ 16–17. File Upload
📍 LOW:
Загружаешь .php
Всё — Burp не нужен
📍 MEDIUM:
Intercept ON
Загружаешь файл

👉 В Burp:

найти:

filename="shell.jpg"

👉 заменить:

shell.php

👉 Forward

📍 HIGH:

👉 В Burp:

после:

Content-Type: image/jpeg

добавить строку:

GIF98

👉 Forward

⚪ 18. Weak Session ID
📍 Шаги:
Intercept ON
Нажимаешь "Generate"

👉 В Burp:

ПКМ → Send to Repeater
📍 Дальше:
Вкладка Repeater
Жмёшь:
👉 Send (10 раз)
Смотришь:
dvwaSession=
🔴 19. SQL Injection (DVWA)
📍 Burp не обязателен

👉 Просто вводишь:

1' OR 1=1--
🟡 24–30. SQL (через Burp)
📍 Если GET:

👉 меняешь прямо в URL

📍 Если POST:
Intercept ON
Отправляешь форму

👉 В Burp:

меняешь:

username=

или:

id=

👉 Forward

🟢 26. CAPTCHA (Web for Pentester)
📍 Burp:
Intercept ON
Submit

👉 удалить:

captcha=

👉 Forward

🔵 31–32. XSS (bWAPP)
📍 GET:
просто вводишь payload
📍 POST:
Intercept ON
Submit

👉 изменить:

firstname=

👉 Forward

💥 СУПЕР-КРАТКО (чтобы запомнить)
👉 ВСЕГДА:
Proxy → Intercept ON
Сделать действие
В Burp:
изменить параметр
Нажать:
👉 Forward

