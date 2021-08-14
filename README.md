# アプリ名

LIPHOLIC

## アプリの概要

簡単な診断ページからおすすめ商品を提案する機能がある  
リップアイテム専用のECサイトです。

## DB関連スクリプト

```sql
CREATE DATABASE lip_holic;
CREATE USER lip_user IDENTIFIED BY '1234';
GRANT ALL ON lip_holic .* TO lip_user;
```

## テーブル作成(計6個)

1. users
2. items
3. carts
4. purchase_informations
5. purchase_information_details
6. diagnosis

```sql
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    nick_name VARCHAR(25) NOT NULL,
    last_name VARCHAR(25) NOT NULL,
    first_name VARCHAR(25) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    password VARCHAR(255) NOT NULL,
    postcode CHAR(7) NOT NULL,
    residence VARCHAR(255) NOT NULL,
    phone_number VARCHAR(11) NOT NULL,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    update_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
``` 
```sql
CREATE TABLE items (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(25) NOT NULL,
    amount INT NOT NULL,
    explanation TEXT NOT NULL,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    update_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
``` 
```sql
CREATE TABLE carts (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    item_id INT NOT NULL,
    quantity INT NOT NULL,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    update_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    CONSTRAINT fk_carts_user_id
        FOREIGN KEY (user_id)
        REFERENCES users(id)
        ON DELETE CASCADE ON UPDATE CASCADE,
    CONSTRAINT fk_carts_item_id
        FOREIGN KEY (item_id)
        REFERENCES items(id)
        ON DELETE CASCADE ON UPDATE CASCADE
);
```
```sql
CREATE TABLE purchase_informations (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT NOT NULL,
    purchase_date DATETIME DEFAULT CURRENT_TIMESTAMP,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    update_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    CONSTRAINT fk_pi_user_id
        FOREIGN KEY (user_id)
        REFERENCES users(id)
        ON DELETE CASCADE ON UPDATE CASCADE
);
``` 
```sql
CREATE TABLE purchase_information_details (
    id INT PRIMARY KEY AUTO_INCREMENT,
    purchase_information_id INT NOT NULL,
    item_id INT NOT NULL,
    quantity INT NOT NULL,
    amount INT NOT NULL,
    total_amount INT NOT NULL,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    update_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    CONSTRAINT fk_pi_details_pi_id
        FOREIGN KEY (purchase_information_id)
        REFERENCES purchase_informations(id)
        ON DELETE CASCADE ON UPDATE CASCADE,
    CONSTRAINT fk_pi_details_item_id
        FOREIGN KEY (item_id)
        REFERENCES items(id)
        ON DELETE CASCADE ON UPDATE CASCADE
);
```
```sql
CREATE TABLE diagnosis (
    id INT PRIMARY KEY AUTO_INCREMENT,
    explanation TEXT NOT NULL,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    update_at DATETIME DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
``` 