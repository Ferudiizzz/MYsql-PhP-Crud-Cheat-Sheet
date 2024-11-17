# PHP CRUD Operations with MySQL Database

This project demonstrates basic PHP CRUD operations with a MySQL database. It includes code examples for **Create**, **Read**, **Update**, and **Delete** operations.

## Features

- **Database Connection**: Establishes a connection to a MySQL database using PHP.
- **Create**: Insert new records into the database.
- **Read**: Fetch and display records from the database.
- **Update**: Update existing records in the database.
- **Delete**: Delete records from the database.

---

## Requirements

- **PHP**: Ensure PHP is installed on your system.
- **MySQL**: Set up MySQL or MariaDB server.
- **Web Server**: Apache or Nginx is required to run PHP files.

---

## Setup Instructions

### 1. Database Connection

```php
<?php
// Database configuration
$servername = "localhost";
$username = "root";
$password = "";
$database = "your_database_name";

// Create connection
$conn = new mysqli($servername, $username, $password, $database);

// Check connection
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}
?>
```
### 2.  Create (Insert Data)
```
<?php
if (isset($_POST['create'])) {
    $name = $_POST['name'];
    $email = $_POST['email'];
    
    $sql = "INSERT INTO users (name, email) VALUES ('$name', '$email')";
    
    if ($conn->query($sql) === TRUE) {
        echo "New record created successfully";
    } else {
        echo "Error: " . $sql . "<br>" . $conn->error;
    }
}
?>

<form method="post" action="">
    <input type="text" name="name" placeholder="Name" required>
    <input type="email" name="email" placeholder="Email" required>
    <button type="submit" name="create">Create</button>
</form>

```
### 3. Read (Display Data)
```
<?php
$sql = "SELECT * FROM users";
$result = $conn->query($sql);

if ($result->num_rows > 0) {
    echo "<table border='1'>
            <tr>
                <th>ID</th>
                <th>Name</th>
                <th>Email</th>
                <th>Actions</th>
            </tr>";
    
    while($row = $result->fetch_assoc()) {
        echo "<tr>
                <td>" . $row["id"] . "</td>
                <td>" . $row["name"] . "</td>
                <td>" . $row["email"] . "</td>
                <td>
                    <a href='?edit=" . $row["id"] . "'>Edit</a>
                    <a href='?delete=" . $row["id"] . "' onclick='return confirm(\"Are you sure?\")'>Delete</a>
                </td>
              </tr>";
    }
    echo "</table>";
} else {
    echo "0 results";
}
?>

```
### 4. Update (Edit Data)
```
<?php
// Fetch record to be updated
if (isset($_GET['edit'])) {
    $id = $_GET['edit'];
    $result = $conn->query("SELECT * FROM users WHERE id=$id");

    if ($result->num_rows == 1) {
        $row = $result->fetch_assoc();
        $name = $row['name'];
        $email = $row['email'];
    }
}

// Update the record
if (isset($_POST['update'])) {
    $id = $_POST['id'];
    $name = $_POST['name'];
    $email = $_POST['email'];
    
    $sql = "UPDATE users SET name='$name', email='$email' WHERE id=$id";
    
    if ($conn->query($sql) === TRUE) {
        echo "Record updated successfully";
    } else {
        echo "Error updating record: " . $conn->error;
    }
}
?>

<form method="post" action="">
    <input type="hidden" name="id" value="<?php echo $id; ?>">
    <input type="text" name="name" value="<?php echo $name; ?>" required>
    <input type="email" name="email" value="<?php echo $email; ?>" required>
    <button type="submit" name="update">Update</button>
</form>

```
### 5. Delete (Remove Data)
```
<?php
if (isset($_GET['delete'])) {
    $id = $_GET['delete'];
    $sql = "DELETE FROM users WHERE id=$id";
    
    if ($conn->query($sql) === TRUE) {
        echo "Record deleted successfully";
    } else {
        echo "Error deleting record: " . $conn->error;
    }
}
?>

```
