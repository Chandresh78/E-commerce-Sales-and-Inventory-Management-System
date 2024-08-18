# E-commerce Sales & Inventory Management System

## Project Description
The **E-commerce Sales & Inventory Management System** is a comprehensive SQL-based solution designed to manage product inventories, track sales, and analyze customer behavior. This project involves designing a normalized database schema, writing complex SQL queries, and creating reports to support business decisions.

## Key Features
1. **Database Schema Design**
   - Entity-Relationship (ER) diagram for the database.
   - Normalized tables to avoid redundancy and ensure data integrity.
   - Tables include `Users`, `Products`, `Orders`, `OrderItems`, `Categories`, `Suppliers`, `Inventory`, `Payments`, and `Reviews`.

2. **Data Insertion and Management**
   - Realistic sample data using INSERT statements.
   - Procedures for adding, updating, and deleting records.

3. **Complex SQL Queries**
   - Sales Analysis: Top-selling products, monthly sales trends, customer purchase patterns.
   - Inventory Management: Track inventory levels, identify low-stock items, forecast inventory needs.
   - Customer Insights: Analyze customer demographics, frequent buyers, customer feedback.

4. **Stored Procedures and Triggers**
   - Procedures for placing orders, updating inventory, processing payments.
   - Triggers to automatically update inventory levels and sales statistics after each order.

5. **Reporting and Analytics**
   - Reports on sales performance, inventory status, customer feedback using complex joins and aggregate functions.
   - Views for simplified data access and report generation.

6. **Data Security and Integrity**
   - Data integrity using foreign key constraints, check constraints, unique indexes.
   - User roles and permissions to secure sensitive data and ensure authorized access.

## Database Schema and Data

### Users Table
```sql
CREATE TABLE Users (
    UserID INT PRIMARY KEY AUTO_INCREMENT,
    Username VARCHAR(50) NOT NULL,
    Email VARCHAR(100) NOT NULL UNIQUE,
    PasswordHash VARCHAR(255) NOT NULL,
    FullName VARCHAR(100),
    Address TEXT,
    PhoneNumber VARCHAR(15),
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

INSERT INTO Users (Username, Email, PasswordHash, FullName, Address, PhoneNumber)
VALUES
('johndoe', 'john@example.com', 'hashedpassword123', 'John Doe', '123 Main St, Anytown', '555-1234'),
('janedoe', 'jane@example.com', 'hashedpassword456', 'Jane Doe', '456 Elm St, Anytown', '555-5678');
```

### Products Table
```sql
CREATE TABLE Products (
    ProductID INT PRIMARY KEY AUTO_INCREMENT,
    Name VARCHAR(100) NOT NULL,
    Description TEXT,
    Price DECIMAL(10, 2) NOT NULL,
    CategoryID INT,
    SupplierID INT,
    Stock INT NOT NULL DEFAULT 0,
    CreatedAt TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (CategoryID) REFERENCES Categories(CategoryID),
    FOREIGN KEY (SupplierID) REFERENCES Suppliers(SupplierID)
);

INSERT INTO Products (Name, Description, Price, CategoryID, SupplierID, Stock)
VALUES
('Laptop', 'A high-performance laptop', 999.99, 1, 1, 50),
('Smartphone', 'Latest model smartphone', 699.99, 2, 2, 100),
('Headphones', 'Noise-cancelling headphones', 199.99, 3, 3, 200);
```

### Orders Table
```sql
CREATE TABLE Orders (
    OrderID INT PRIMARY KEY AUTO_INCREMENT,
    UserID INT,
    OrderDate TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    TotalAmount DECIMAL(10, 2),
    Status VARCHAR(50) DEFAULT 'Pending',
    FOREIGN KEY (UserID) REFERENCES Users(UserID)
);

INSERT INTO Orders (UserID, TotalAmount, Status)
VALUES
(1, 1199.98, 'Completed'),
(2, 199.99, 'Pending');
```

### OrderItems Table
```sql
CREATE TABLE OrderItems (
    OrderItemID INT PRIMARY KEY AUTO_INCREMENT,
    OrderID INT,
    ProductID INT,
    Quantity INT NOT NULL,
    Price DECIMAL(10, 2) NOT NULL,
    FOREIGN KEY (OrderID) REFERENCES Orders(OrderID),
    FOREIGN KEY (ProductID) REFERENCES Products(ProductID)
);

INSERT INTO OrderItems (OrderID, ProductID, Quantity, Price)
VALUES
(1, 1, 1, 999.99),
(1, 2, 1, 199.99),
(2, 3, 1, 199.99);
```

### Inventory Table
```sql
CREATE TABLE Inventory (
    InventoryID INT PRIMARY KEY AUTO_INCREMENT,
    ProductID INT,
    Quantity INT NOT NULL,
    LastUpdated TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (ProductID) REFERENCES Products(ProductID)
);

INSERT INTO Inventory (ProductID, Quantity)
VALUES
(1, 50),
(2, 100),
(3, 200);
```

## Sample Complex Queries

- **Top-Selling Products**
```sql
SELECT P.Name, SUM(OI.Quantity) AS TotalSold
FROM OrderItems OI
JOIN Products P ON OI.ProductID = P.ProductID
GROUP BY P.Name
ORDER BY TotalSold DESC
LIMIT 10;
```

- **Monthly Sales Trends**
```sql
SELECT DATE_FORMAT(OrderDate, '%Y-%m') AS Month, SUM(TotalAmount) AS TotalSales
FROM Orders
GROUP BY Month
ORDER BY Month DESC;
```

- **Customer Purchase Patterns**
```sql
SELECT U.Username, COUNT(O.OrderID) AS NumberOfOrders, SUM(O.TotalAmount) AS TotalSpent
FROM Users U
JOIN Orders O ON U.UserID = O.UserID
GROUP BY U.Username
ORDER BY TotalSpent DESC;
```

## Tools and Technologies Used
- SQL (MySQL, PostgreSQL, or any other RDBMS)
- Database Design Tools (e.g., MySQL Workbench, ER/Studio)
- SQL Management Tools (e.g., pgAdmin, DBeaver)
- Reporting Tools (e.g., Tableau, Power BI)

## Getting Started

1. **Clone the repository:**
   ```sh
   git clone https://github.com/yourusername/ecommerce-inventory-system.git
   ```
2. **Navigate to the project directory:**
   ```sh
   cd ecommerce-inventory-system
   ```
3. **Create the database and tables:**
   ```sh
   mysql -u yourusername -p < create_tables.sql
   ```
4. **Insert sample data:**
   ```sh
   mysql -u yourusername -p < insert_data.sql
   ```

## Contributing
Contributions are welcome! Please open an issue or submit a pull request for any changes or improvements.

## License
This project is licensed under the MIT License.

## Contact
For any questions or feedback, please reach out to chandresh00rajpoot@gmail.com

---
