---
layout: posts
title:  "How to Design an Accounting System using SQLite - Part 2"
date:   2023-11-28
tags: [SQL, SQLite]
highlight_home: true
author_profile: true
author: Kenneth Infante
categories: article
tagline: "Reverse Engineering Xero to Teach SQL"
header:
  overlay_image: https://images.unsplash.com/photo-1581091226825-a6a2a5aee158?q=80&w=2070&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D
  teaser: https://images.unsplash.com/photo-1581091226825-a6a2a5aee158?q=80&w=2070&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D
  caption: "Photo credit: [**ThisisEngineering RAEng**](https://unsplash.com/@thisisengineering)"
description: This article shows how to design an accounting system using SQLite.
---

> This article is originally published in this [link](https://towardsdatascience.com/how-to-build-an-accounting-system-using-sqlite-2ce31f8b8652?source=friends_link&sk=85caa76af517b9ebc772c8cb9ba4d3a5) at Medium.com. I decided to split the original article into two parts for ease of reading. The files are also available at this [repo](https://github.com/kennethinfante/accounting-database-using-sqlite).


## Implementing Our Version in SQL

In the previous [article]({{ site.url }}{{ site.baseurl }}{% link _posts/2023-11-28-how-to-build-an-accounting-system-using-SQLite-part2.md %})., we discussed about the basics of accounting and SQL. We also had the preliminary design for our accounting system. Now it's time to implement our design in SQL. Let's start by defining some conventions first.

The primary key will have column name of *id*, and the foreign key will have the column name format *table_id* where *table* is the table containing the primary key. For example, in the `Invoices` table, the foreign key will be *customer_id*.

Time for the SQL code. Here it is.

```sql
DROP TABLE IF EXISTS `Accounts`;
CREATE TABLE IF NOT EXISTS `Accounts` (
    id INTEGER PRIMARY KEY,
    name TEXT,
    description TEXT,
    type TEXT,
    category TEXT
);

DROP TABLE IF EXISTS `Customers`;
CREATE TABLE IF NOT EXISTS `Customers` (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    contact_person TEXT,
    email TEXT,
    phone TEXT,
    fax TEXT,
    address TEXT
);

DROP TABLE IF EXISTS `Invoice_Payments`;
CREATE TABLE IF NOT EXISTS `Invoice_Payments` (
    id INTEGER PRIMARY KEY,
    tran_date DATE NOT NULL,
    description TEXT,
    reference TEXT,
    total DECIMAL(20,2) NOT NULL,
    account_id INTEGER NOT NULL, -- automatically Bank
    FOREIGN KEY(`account_id`) REFERENCES `Accounts`(`id`)
);

DROP TABLE IF EXISTS `Invoices`;
CREATE TABLE IF NOT EXISTS `Invoices` (
    id INTEGER PRIMARY KEY,
    tran_date DATE NOT NULL,
    due_date DATE,
    description TEXT,
    reference TEXT,
    total DECIMAL(10,2) NOT NULL,
    status BOOLEAN,
    customer_id INTEGER,
    invoice_payment_id INTEGER,
    account_id INTEGER NOT NULL, -- automatically AR
    FOREIGN KEY(`customer_id`) REFERENCES `Customers`(`id`),
    FOREIGN KEY(`invoice_payment_id`) REFERENCES `Invoice_Payments`(`id`),
    FOREIGN KEY(`account_id`) REFERENCES `Accounts`(`id`)
);

DROP TABLE IF EXISTS `Received_Moneys`;
CREATE TABLE IF NOT EXISTS `Received_Moneys` (
    id INTEGER PRIMARY KEY,
    tran_date DATE NOT NULL,
    description TEXT,
    reference TEXT,
    total DECIMAL(20,2) NOT NULL,
    customer_id INTEGER,
    account_id INTEGER NOT NULL, -- automatically Bank
    FOREIGN KEY(`customer_id`) REFERENCES `Customers`(`id`),
    FOREIGN KEY(`account_id`) REFERENCES `Accounts`(`id`)
);

DROP TABLE IF EXISTS `Invoice_Lines`;
CREATE TABLE IF NOT EXISTS `Invoice_Lines` (
    id INTEGER PRIMARY KEY,
    line_amount DECIMAL(20,2) NOT NULL,
    invoice_id INTEGER,
    line_account_id INTEGER NOT NULL,
    FOREIGN KEY(`invoice_id`) REFERENCES `Invoices`(`id`),
    FOREIGN KEY(`line_account_id`) REFERENCES `Accounts`(`id`)
);

DROP TABLE IF EXISTS `Received_Money_Lines`;
CREATE TABLE IF NOT EXISTS `Received_Money_Lines` (
    id INTEGER PRIMARY KEY,
    line_amount DECIMAL(20,2) NOT NULL,
    received_money_id INTEGER,
    line_account_id INTEGER NOT NULL,
    FOREIGN KEY(`received_money_id`) REFERENCES `Received_Moneys`(`id`),
    FOREIGN KEY(`line_account_id`) REFERENCES `Accounts`(`id`)
);

DROP TABLE IF EXISTS `Suppliers`;
CREATE TABLE IF NOT EXISTS `Suppliers` (
    id INTEGER PRIMARY KEY,
    name TEXT NOT NULL,
    contact_person TEXT,
    email TEXT,
    phone TEXT,
    fax TEXT,
    address TEXT
);

DROP TABLE IF EXISTS `Bill_Payments`;
CREATE TABLE IF NOT EXISTS `Bill_Payments` (
    id INTEGER PRIMARY KEY,
    tran_date DATE NOT NULL,
    description TEXT,
    reference TEXT,
    total DECIMAL(20,2) NOT NULL,
    account_id INTEGER NOT NULL, -- automatically Bank
    FOREIGN KEY(`account_id`) REFERENCES `Accounts`(`id`)
);

DROP TABLE IF EXISTS `Bills`;
CREATE TABLE IF NOT EXISTS `Bills` (
    id INTEGER PRIMARY KEY,
    tran_date DATE NOT NULL,
    due_date DATE,
    description TEXT,
    reference TEXT,
    total DECIMAL(10,2) NOT NULL,
    status BOOLEAN,
    supplier_id INTEGER,
    bill_payment_id INTEGER,
    account_id INTEGER NOT NULL, -- automatically AP
    FOREIGN KEY(`supplier_id`) REFERENCES `Suppliers`(`id`),
    FOREIGN KEY(`bill_payment_id`) REFERENCES `Bill_Payments`(`id`),
    FOREIGN KEY(`account_id`) REFERENCES `Accounts`(`id`)
);

DROP TABLE IF EXISTS `Bill_Lines`;
CREATE TABLE IF NOT EXISTS `Bill_Lines` (
    id INTEGER PRIMARY KEY,
    line_amount DECIMAL(20,2) NOT NULL,
    bill_id INTEGER,
    line_account_id INTEGER NOT NULL,
    FOREIGN KEY(`bill_id`) REFERENCES `Bills`(`id`),
    FOREIGN KEY(`line_account_id`) REFERENCES `Accounts`(`id`)
);

DROP TABLE IF EXISTS `Spent_Moneys`;
CREATE TABLE IF NOT EXISTS `Spent_Moneys` (
    id INTEGER PRIMARY KEY,
    tran_date DATE NOT NULL,
    description TEXT,
    reference TEXT,
    total DECIMAL(20,2) NOT NULL,
    supplier_id INTEGER,
    account_id INTEGER NOT NULL, -- automatically Bank
    FOREIGN KEY(`supplier_id`) REFERENCES `Suppliers`(`id`),
    FOREIGN KEY(`account_id`) REFERENCES `Accounts`(`id`)
);

DROP TABLE IF EXISTS `Spent_Money_Lines`;
CREATE TABLE IF NOT EXISTS `Spent_Money_Lines` (
    id INTEGER PRIMARY KEY,
    line_amount DECIMAL(20,2) NOT NULL,
    spent_money_id INTEGER,
    line_account_id INTEGER NOT NULL,
    FOREIGN KEY(`spent_money_id`) REFERENCES `Spent_Moneys`(`id`),
    FOREIGN KEY(`line_account_id`) REFERENCES `Accounts`(`id`)
);

DROP TABLE IF EXISTS `Journals`;
CREATE TABLE IF NOT EXISTS `Journals` (
    id INTEGER PRIMARY KEY,
    journal_no INTEGER,
    tran_date DATE NOT NULL,
    description TEXT,
    reference TEXT,
    line_account_id INTEGER NOT NULL,
    line_amount DECIMAL(20,2) NOT NULL,
    FOREIGN KEY(`line_account_id`) REFERENCES `Accounts`(`id`)
);


```

A couple of things here:

* SQL commands are not case-sensitive, `CREATE TABLE` is same as `create table`. However, it is standard practice to capitalize the SQL commands.
* The `IF EXISTS` and `IF NOT EXISTS` are optional. I've used them to prevent errors in my SQL commands. For example, if I drop a non-existing table, SQLite will give an error. Also, I put `IF NOT EXISTS` on the create table command so that we don't accidentally override any existing table.
* Be careful with the `DROP TABLE `command! It will delete an existing table without warning even if it has contents.
* If table names have spaces, they should be enclosed with backticks (`).
* Order is important as some tables are dependent to another because of the foreign key. For example, you have to create first `Invoice_Payments` first before the `Invoice` table as the former is dependent on the latter.
* Though SQL is a bit relax with regards to syntax, you should strive to maintain consistency in your SQL code.

I'm using the free and open-sourced SQLite Browser app to view the database.

## Adding Contents to Our Database

Now that we have the database, let's input data to it. Sample data are available in this Github [repo](https://github.com/kennethinfante/accounting-database-using-sqlite).

To make this easier, I included two Python scripts, `fill_existing_db.py`, and `create_and_fill_new_db.py`. You can use either of the files to quickly fill our database with sample data.

## Creating Financial Reports from Our Database

To prove that our database works as a crude accounting system, let's create the **Trial Balance**.

The first step is to create the transaction views for our Invoices, Bills, Received_Moneys, and Spent_Moneys transactions. The code will be as follows

```sql
DROP VIEW IF EXISTS Invoice_Trans;
CREATE VIEW IF NOT EXISTS Invoice_Trans AS
with
itrans as (SELECT
    -- 'INV'||i.id as `tran_id`,
    i.id,
    i.tran_date,
    i.account_id,
    'Accounts Receivable' as `account_name`,
    i.total,
    il.line_account_id,
    il.line_amount,
    ip.account_id as bank_account,
    'Business Bank Account' as `bank_name`,
    i.status
from Invoices as i
left join Invoice_Lines as il on i.id = il.invoice_id
left join Invoice_Payments as ip on i.invoice_payment_id = ip.id
)
select
itrans.*,
c.name as `line_account_name`
from itrans
left join Accounts as c on itrans.line_account_id = c.id;
SELECT * from Invoice_Trans;

------------------------------------------------
DROP VIEW IF EXISTS Bill_Trans;
CREATE VIEW IF NOT EXISTS Bill_Trans AS
with
btrans as (SELECT
    -- 'INV'||i.id as `tran_id`,
    b.id,
    b.tran_date,
    b.account_id,
    'Accounts Payable' as `account_name`,
    b.total,
    bl.line_account_id,
    bl.line_amount,
    bp.account_id as bank_account,
    'Business Bank Account' as `bank_name`,
    b.status
from Bills as b
left join Bill_Lines as bl on b.id = bl.bill_id
left join Bill_Payments as bp on b.bill_payment_id = bp.id
)
select
btrans.*,
c.name as `line_account_name`
from btrans
left join Accounts as c on btrans.line_account_id = c.id;
SELECT * from Bill_Trans;
------------------------------------------------
DROP VIEW IF EXISTS Received_Money_Trans;
CREATE VIEW IF NOT EXISTS Received_Money_Trans AS
SELECT
    rm.id,
    tran_date,
    account_id,
    'Business Bank Account' as `account_name`,
    total,
    rml.id  as `line_id`,
    rml.line_account_id,
    c.name as `line_account_name`,
    rml.line_amount
from Received_Moneys as rm
left join Received_Money_Lines as rml on rm.id = rml.received_money_id
left join Accounts  as c on c.id = rml.line_account_id;
SELECT * from Received_Money_Trans;

------------------------------------------------
DROP VIEW IF EXISTS Spent_Money_Trans;
CREATE VIEW IF NOT EXISTS Spent_Money_Trans AS
SELECT
    sm.id,
    tran_date,
    account_id,
    'Business Bank Account' as `account_name`,
    total,
    sml.id  as `line_id`,
    sml.line_account_id,
    c.name as `line_account_name`,
    sml.line_amount
from Spent_Moneys as sm
left join Spent_Money_Lines as sml on sm.id = sml.spent_money_id
left join Accounts  as c on c.id = sml.line_account_id;
SELECT * from Spent_Money_Trans;

```

Finally, we create the code for the Trial Balance or TB for short. Note that TB is just a collection of the balances of our transactions taking note of the rules we laid down when we designed our database.

The code is as follows

```sql
DROP VIEW IF EXISTS Trial_Balance;
CREATE VIEW IF NOT EXISTS Trial_Balance as
-- CREATE TB

-- select all sales
select
    line_account_id as acct_code,
    line_account_name as acct_name,
    (case when sum(line_amount) > 0 then sum(line_amount) else 0 end) as debit_bal,
    (case when sum(line_amount) < 0 then sum(line_amount) else 0 end) as credit_bal
from Invoice_Trans
group by line_account_id

union all

-- select all purchases
select
    line_account_id as acct_code,
    line_account_name as acct_name,
    (case when sum(line_amount) > 0 then sum(line_amount) else 0 end) as debit_bal,
    (case when sum(line_amount) < 0 then sum(line_amount) else 0 end) as credit_bal
from Bill_Trans
group by line_account_id

union all

-- select all received money counterpart account like Sales
select
    line_account_id as acct_code,
    line_account_name as acct_name,
    (case when sum(line_amount) > 0 then sum(line_amount) else 0 end) as debit_bal,
    (case when sum(line_amount) < 0 then sum(line_amount) else 0 end) as credit_bal
from Received_Money_Trans
group by line_account_id

union all

-- select all spent money counterpart account like Purchases
select
    line_account_id as acct_code,
    line_account_name as acct_name,
    (case when sum(line_amount) > 0 then sum(line_amount) else 0 end) as debit_bal,
    (case when sum(line_amount) < 0 then sum(line_amount) else 0 end) as credit_bal
from Spent_Money_Trans
group by line_account_id

union all

-- select all AP
select
    account_id as acct_code,
    account_name as acct_name,
    -(case when sum(line_amount) < 0 then sum(line_amount) else 0 end) as debit_bal,
    -(case when sum(line_amount) > 0 then sum(line_amount) else 0 end) as credit_bal
from Bill_Trans
where status = "0"
-- no need for group by as per our design, there should be only one AP account

union all

-- select all AR
select
    account_id as acct_code,
    account_name as acct_name,
    -(case when sum(line_amount) < 0 then sum(line_amount) else 0 end) as debit_bal,
    -(case when sum(line_amount) > 0 then sum(line_amount) else 0 end) as credit_bal
from Invoice_Trans
where status = "0"

union all

-- select all bill_payments under Cash account
select
    bank_account as acct_code,
    bank_name as acct_name,
    -(case when sum(line_amount) < 0 then sum(line_amount) else 0 end) as debit_bal,
    -(case when sum(line_amount) > 0 then sum(line_amount) else 0 end) as credit_bal
from Bill_Trans
where status = "1"

union all

-- select all invoice_payments under Cash account
select
    bank_account as acct_code,
    bank_name as acct_name,
    -(case when sum(line_amount) < 0 then sum(line_amount) else 0 end) as debit_bal,
    -(case when sum(line_amount) > 0 then sum(line_amount) else 0 end) as credit_bal
from Invoice_Trans
where status = "1"

union all

-- select all received_money under Cash account
select
    account_id as acct_code,
    account_name as acct_name,
    -(case when sum(line_amount) < 0 then sum(line_amount) else 0 end) as debit_bal,
    -(case when sum(line_amount) > 0 then sum(line_amount) else 0 end) as credit_bal
from Received_Money_Trans

union all

-- select all spent_money under Cash account
select
    account_id as acct_code,
    account_name as acct_name,
    -(case when sum(line_amount) < 0 then sum(line_amount) else 0 end) as debit_bal,
    -(case when sum(line_amount) > 0 then sum(line_amount) else 0 end) as credit_bal
from Spent_Money_Trans
order by acct_code
```

The above code contains multiple SQL queries joined by the command union all. I've annotated each query to show what each is trying to achieve.

Executing it should result in the following TB.

![Trial balance]({{ site.url }}{{ site.baseurl }}/assets/images/accounting-system-using-sqlite/trial_balance.png)

Putting this to Excel results to total debits and credits of 14115 and -14115 respectively.

## Hooray! You've Made It

Creating an accounting system is really complex. We essentially explored the whole gamut of database design — from concepts to ERD to creation to querying it. Pat yourself on the back for reaching this far.

Take note that we deliberately limited our database to focus more on the concepts. You can relaxed these assumptions and build another to explore more about database design.

That's it! You’re now an SQL ninja! Congratulations!

![Success!)](https://images.unsplash.com/photo-1519834785169-98be25ec3f84?q=80&w=1964&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D "Ian Stauffer at https://unsplash.com/@ianstauffer")