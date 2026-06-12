# cosmatic-management-system
CREATE DATABASE cosmetics_db;

USE cosmetics_db;

CREATE TABLE products (
    id INT AUTO_INCREMENT PRIMARY KEY,
    product_name VARCHAR(100),
    brand VARCHAR(100),
    category VARCHAR(50),
    price DECIMAL(10,2),
    stock INT
);
<?php
$conn = mysqli_connect("localhost","root","","cosmetics_db");

if(!$conn){
    die("Connection Failed: ".mysqli_connect_error());
}
?>
<!DOCTYPE html>
<html>
<head>
<title>Cosmetic Management System</title>
<link rel="stylesheet" href="style.css">
</head>
<body>

<div class="container">
    <h1>Cosmetic Management System</h1>

    <a href="login.php">Admin Login</a>
</div>

</body>
</html>
<?php
session_start();

if(isset($_POST['login']))
{
    $username=$_POST['username'];
    $password=$_POST['password'];

    if($username=="admin" && $password=="admin123")
    {
        $_SESSION['admin']=$username;
        header("Location: dashboard.php");
    }
    else
    {
        echo "Invalid Login";
    }
}
?>

<form method="post">
    Username:
    <input type="text" name="username"><br><br>

    Password:
    <input type="password" name="password"><br><br>

    <input type="submit" name="login" value="Login">
</form>
<!DOCTYPE html>
<html>
<head>
<title>Dashboard</title>
</head>
<body>

<h2>Admin Dashboard</h2>

<a href="add_product.php">Add Product</a><br><br>

<a href="view_products.php">View Products</a>

</body>
</html>
<?php
include 'db.php';

if(isset($_POST['save']))
{
    $name=$_POST['product_name'];
    $brand=$_POST['brand'];
    $category=$_POST['category'];
    $price=$_POST['price'];
    $stock=$_POST['stock'];

    mysqli_query($conn,
    "INSERT INTO products(product_name,brand,category,price,stock)
    VALUES('$name','$brand','$category','$price','$stock')");
}
?>

<form method="post">

Product Name:
<input type="text" name="product_name"><br><br>

Brand:
<input type="text" name="brand"><br><br>

Category:
<input type="text" name="category"><br><br>

Price:
<input type="number" step="0.01" name="price"><br><br>

Stock:
<input type="number" name="stock"><br><br>

<input type="submit" name="save" value="Save Product">

</form>
<?php
include 'db.php';

$result=mysqli_query($conn,"SELECT * FROM products");
?>

<table border="1">
<tr>
<th>ID</th>
<th>Name</th>
<th>Brand</th>
<th>Category</th>
<th>Price</th>
<th>Stock</th>
<th>Action</th>
</tr>

<?php
while($row=mysqli_fetch_assoc($result))
{
?>
<tr>
<td><?php echo $row['id']; ?></td>
<td><?php echo $row['product_name']; ?></td>
<td><?php echo $row['brand']; ?></td>
<td><?php echo $row['category']; ?></td>
<td><?php echo $row['price']; ?></td>
<td><?php echo $row['stock']; ?></td>

<td>
<a href="delete_product.php?id=<?php echo $row['id']; ?>">
Delete
</a>
</td>

</tr>
<?php
}
?>
</table>
<?php
include 'db.php';

$id=$_GET['id'];

mysqli_query($conn,"DELETE FROM products WHERE id='$id'");

header("Location:view_products.php");
?>
body{
    font-family: Arial, sans-serif;
    background:#f5f5f5;
}

.container{
    width:500px;
    margin:auto;
    text-align:center;
    margin-top:100px;
}

h1{
    color:#d63384;
}

a{
    text-decoration:none;
    background:#d63384;
    color:white;
    padding:10px 20px;
    border-radius:5px;
}
