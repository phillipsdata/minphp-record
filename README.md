# minphp/Db

[![Build Status](https://travis-ci.org/phillipsdata/minphp-record.svg?banch=master)](https://travis-ci.org/phillipsdata/minphp-record) [![Coverage Status](https://coveralls.io/repos/phillipsdata/minphp-record/badge.svg)](https://coveralls.io/r/phillipsdata/minphp-record)

Database Access Library.

Provides a fluent interface for generating SQL queries.

## Installation

Install via composer:

```sh
composer require minphp/record:~1.0
```

## Basic Usage

First, initialize your connection:

```php
use minphp\Record\Record;

$dbInfo = array(
    'driver' => 'mysql',
    'host' => 'localhost',
    'database' => 'databasename',
    'user' => 'user',
    'pass' => 'pass'
);

$record = new Record($dbInfo);
```

### Select

Use `fetch()` to fetch a single record, `fetchAll()` to fetch all records, `getStatement()` for the `\PDOStatement` object which you can iterate over. Finally, `get()` will return the SQL for the query.

#### All

```php
$users = $record->select()
    ->from('users')
    ->fetchAll();
```

#### Tuples

```php
$users = $record->select(array('id', 'name', 'email'))
    ->from('users')
    ->fetchAll();
```

#### Tuple Aliasing

```php
$users = $record->select(array('id', 'name', 'email' => 'login'))
    ->from('users')
    ->fetchAll();
```

#### Value Injection

```php
$users = $record->select(array('id', 'name', 'email' => 'login', '\'active\'' => 'status'))
    ->from('users')
    ->fetchAll();
```

#### Limiting

Limit 10 records:

```php
$users = $record->select()
    ->from('users')
    ->limit(10);
```

Limit 10 records, starting at record 20:

```php
$users = $record->select()
    ->from('users')
    ->limit(10, 20);
```

#### Ordering

```php
$users = $record->select()
    ->from('users')
    ->order(array('id' => 'asc'));
```

#### Where

Operators include:

- `=` equality
- `!=`, `<>` inequality
- `>` greather than
- `>=` greather than or equal
- `<` less than
- `<=` less than or equal
- `in` in the given values
- `notin` not in the given values
- `exists` exists in the result set
- `notexists` does not exist in the result set

**Note:** If `null` is supplied as the value, with `=` or `!=` the result becomes `IS NULL` or `IS NOT NULL`, respectively.

```php
$users = $record->select()
    ->from('users')
    ->where('id', '=', 10);
```