-- Создание хранилища для сущности "Клиент"
CREATE TABLE hub_customer (
customer_id INT NOT NULL AUTO_INCREMENT,
customer_code VARCHAR(50) NOT NULL,
load_date TIMESTAMP NOT NULL DEFAULT NOW(),
record_surce VARCHAR(50) NOT NULL,
PRIMARY KEY (customer_id)
);

-- Создание хранилища для сущности "Товар"
CREATE TABLE hub_product (
product_id INT NOT NULL AUTO_INCREMENT,
product_code VARCHAR(50) NOT NULL,
product_name VARCHAR(100) NOT NULL,
product_category VARCHAR(50) NOT NULL,
load_date TIMESTAMP NOT NULL DEFAULT NOW(),
record_surce VARCHAR(50) NOT NULL,
PRIMARY KEY (product_id)
);

-- Создание хранилища для связи между клиентом и товаром через продажу
CREATE TABLE link_sales (
sales_id INT NOT NULL AUTO_INCREMENT,
customer_id INT NOT NULL,
product_id INT NOT NULL,
sales_date DATE NOT NULL,
load_date TIMESTAMP NOT NULL DEFAULT NOW(),
record_surce VARCHAR(50) NOT NULL,
PRIMARY KEY (sales_id),
FOREIGN KEY (customer_id) REFERENCES hub_customer(customer_id),
FOREIGN KEY (product_id) REFERENCES hub_product(product_id)
);

-- Создание сателлита для хранения истории изменений клиентов
CREATE TABLE sat_customer (
customer_id INT NOT NULL,
valid_from DATE NOT NULL,
valid_to DATE,
is_current BOOLEAN NOT NULL DEFAULT TRUE,
customer_address VARCHAR(200),
customer_phone VARCHAR(20),
customer_email VARCHAR(50),
load_date TIMESTAMP NOT NULL DEFAULT NOW(),
record_surce VARCHAR(50) NOT NULL,
PRIMARY KEY (customer_id, valid_from),
FOREIGN KEY (customer_id) REFERENCES hub_customer(customer_id)
);

-- Создание сателлита для хранения истории изменений товаров
CREATE TABLE sat_product (
product_id INT NOT NULL,
valid_from DATE NOT NULL,
valid_to DATE,
is_current BOOLEAN NOT NULL DEFAULT TRUE,
product_description TEXT,
product_price NUMERIC(10,2),
load_date TIMESTAMP NOT NULL DEFAULT NOW(),
record_surce VARCHAR(50) NOT NULL,
PRIMARY KEY (product_id, valid_from),
FOREIGN KEY (product_id) REFERENCES hub_product(product_id)
);

-- Создание таблицы для хранения метаданных
CREATE TABLE dv_metadata (
object_type VARCHAR(50) NOT NULL,
object_name VARCHAR(100) NOT NULL,
attribute_name VARCHAR(100),
attribute_value TEXT,
load_date TIMESTAMP NOT NULL DEFAULT NOW(),
record_surce VARCHAR(50) NOT NULL,
PRIMARY KEY (object_type, object_name, attribute_name, load_date)
);

-- Создание таблицы для хранения истории изменений
CREATE TABLE dv_audit (
table_name VARCHAR(100) NOT NULL,
operation_type VARCHAR(10) NOT NULL,
operation_date TIMESTAMP NOT NULL DEFAULT NOW(),
user_name VARCHAR(50),
load_date TIMESTAMP NOT NULL DEFAULT NOW(),
record_surce VARCHAR(50) NOT NULL,
PRIMARY KEY (table_name, operation_type, operation_date)
);