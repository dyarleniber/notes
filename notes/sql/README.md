# SQL

## MySQL

### Commands

```sql
GRANT ALL ON db_payment_api.* TO 'defaultuser'@'%' IDENTIFIED BY 'userpassword123';
```

It creates a new user account called defaultuser that will be allowed to access all tables in db_payment_api database.

```sql
FLUSH PRIVILEGES;
```

It flushs the privileges to notify the MySQL server of the changes:
