---
title: MySQL中的数据类型
date: 2026-02-10 13:37:45
tags:
  - [MySQL]
  - [Database]
---

## MySQL 的三大数据类型

1. **数值类型**：整型（TINYINT、SMALLINT、MEDIUMINT、INT 和 BIGINT）、浮点型（FLOAT 和 DOUBLE）、定点型（DECIMAL）。
   - 整型可以使用可选的 UNSIGNED 属性来表示不允许负值的无符号整数，这可以将正整数的上限提高一倍，因为它不需要存储负数值。例如， TINYINT UNSIGNED 类型的取值范围是 0 ~ 255，而普通的 TINYINT 类型的值范围是 -128 ~ 127。
2. **字符串类型**：CHAR、VARCHAR、TINYTEXT、TEXT、MEDIUMTEXT、LONGTEXT、TINYBLOB、BLOB、MEDIUMBLOB 和 LONGBLOB 等，最常用的是 CHAR 和 VARCHAR。CHAR 是定长字符串，VARCHAR 是变长字符串。CHAR(M) 和 VARCHAR(M) 的 M 都代表能够保存的字符数的最大值，且无论是字母、数字还是中文，每个都只占用一个字符。
   - 注意，针对 VARCHAR，虽说 VARCHAR(100) 和 VARCHAR(10) 能存储的字符范围不同，但二者存储相同的字符串，所占用**磁盘**的存储空间其实是一样的，不过，VARCHAR(100) 会消耗更多的**内存**。此外，存储手机号**强烈推荐使用 VARCHAR 类型**，而不是 INT 或 BIGINT。这是为了保证格式的兼容性（手机号可能有各种前导）、查询的灵活性（VARCHAR 可以配合 `Like` 快速拼接）、以及**加密的要求**（INT 或 BIGINT 类型无法存储加密后变长的字符串密文）。
   - DECIMAL 和 FLOAT/DOUBLE 的区别是：DECIMAL 是定点数，FLOAT/DOUBLE 是浮点数。DECIMAL 可以存储精确的小数值，FLOAT/DOUBLE 只能存储近似的小数值——因为浮点数会直接截断在二进制下无法除尽的小数，而定点数则采用了另外一种表示小数的方式，确保小数的精确性。
   - 数据库规范通常不推荐使用 TEXT 和 BLOB 类型。因为检索效率较低，可能会消耗大量的网络和 IO 带宽等等。尽管它们能存储更长的字符串和二进制大对象。
3. **日期时间类型**：YEAR、TIME、DATE、DATETIME 和 TIMESTAMP 等。
   - TIMESTAMP 有时区信息，DATETIME 类型没有；TIMESTAMP 使用 4 个字节的存储空间，DATETIME 需要耗费 8 个字节。但是，Timestamp 表示的时间范围更小。TIMESTAMP 的核心优势在于其内建的时区处理能力。
   - 有了专门的表示时间的类型，一般就不建议用字符串来存储日期了。与 MySQL 内建的日期时间类型相比，字符串通常需要占用更多的存储空间来表示相同的时间信息，且查询、索引性能更差，计算功能也会受到限制。
   - 然而，实践中会常用整数类型（`INT` 或 `BIGINT`）来存储所谓的“Unix 时间戳”（即从 1970 年 1 月 1 日 00:00:00 UTC 起至目标时间的总秒数（10 位），或毫秒数（13 位）），即**数值型时间戳**。这种存储方式具有 `TIMESTAMP` 类型的所具有一些优点，并且使用它进行日期排序以及对比等操作的效率会更高，跨系统也很方便，毕竟只是存放的数值。缺点也很明显，就是无法直观地看到具体时间。

> MySQL 中没有专门的布尔类型，而是用 TINYINT(1) 类型来表示布尔值。TINYINT(1) 类型可以存储 0 或 1，分别对应 false 或 true。

## 关于 NULL

`NULL` 代表一个不确定的值，它不等于任何值，包括它自身。因此，`SELECT NULL = NULL` 的结果是 `NULL`，而不是 `true` 或 `false`。`NULL` 意味着缺失或未知的信息。任何值与 `NULL` 进行比较的结果都是 `NULL`，表示结果不确定。要判断一个值是否为 `NULL`，必须使用 `IS NULL` 或 `IS NOT NULL`。

此外，`COUNT(*)` 会统计所有行数，包括包含 `NULL` 值的行，`COUNT(列名)` 却会统计指定列中非 `NULL` 值的行数，聚合函数对 `NULL` 的行为差异会增大代码出 Bug 的可能性。所以一般都不建议以它作为列的默认值。

内容参考：[Java Guide](https://javaguide.cn/)。
