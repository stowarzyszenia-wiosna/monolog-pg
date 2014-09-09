monolog-mysql
=============

MySQL Handler for Monolog, which allows to store log messages to a MySQL Table.
It can log text messages to a specific table, and creates the table automatically if it does not exist.
The class further allows to dynamically add extra attributes, which are stored in a separate database field, and can be used for later analyzing and sorting.

= Installation
monolog-mysql is availaible via composer. Just add the following line to your required section in composer.json and do a `php composer.phar update`.

```
"wazaari/monolog-mysql": ">1.0.0"
```

# Usage
Just use this as any other Monolog Handler, push it to the stack of your Monolog Logger instance. The Handler however needs some parameters:

- **$pdo** PDO Instance of your database. Pass along the PDO instantiation of your database connection with your database selected.
- **$table** The table of the database where the logs should be stored
- **$additionalFields** simple array of additional database fields, which should be stored in the database. The columns are created automatically, and the fields can later be used in the extra contect section of a record. See examples below. _Defaults to an empty array()_
- **$level** can be any of the standard Monolog logging levels. Use Monologs statically defined contexts. _Defaults to Logger::DEBUG_
- **$bubble** _Defaults to true_

# Examples
Given that $pdo is your database instance, you could use the class as follows:

```php
//Create MysqlHandler
$mySQLHandler = new MySQLHandler($pdo, "log", array(' username', 'userid'), Logger::DEBUG);

//Create logger
$logger = new \Monolog\Logger($context);
$logger->pushHandler($mySQLHandler);

//Now you can use the logger, and further attach additonal information
$logger->addWarning("This is a great message, woohoo!", array('username'  => 'John Doe', 'userid'  => 245));
```