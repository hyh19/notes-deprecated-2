[TOC]

# 第 12 章 使用 Cookie 和用户会话

## 12.1 Cookie 简介

n/a

## 12.2 使用 PHP 设置一个 cookie

**在 PHP 脚本中设置一个 cookie 的两种方法：**

***注意：以下两个函数都应该在任何其他内容发送给浏览器之前调用。***

- `header()` 函数（不推荐）

```php
header("Set-Cookie: vegetable=artichoke; expires=Thu, 19-Jan-12 14:39:58 GMT; path=/; domain=yourdomain.com")
```

- `setcookie()` 函数

http://php.net/setcookie \
http://php.net/$_COOKIE

这个函数接受 cookie 名字、cookie 值、UNIX 时间戳格式的过期时间、路径、域，以及一个整数作为参数（1 表示安全发送，0 表示非安全发送，详情参考 API）。除了第一个参数以外，其他参数都是可选的。

**程序清单 12.1 设置并显示一个 cookie 值**
```php
<?php
setcookie("vagetable", "artichoke", time()+3600, "/", "localhost", 0);

if (isset($_COOKIE['vagetable'])) {
    echo "<p>Hello again! You have chosen: ".$_COOKIE['vagetable']."</p>";
} else {
    echo "<p>Hello, you. This may be your first visit.</p>";
}
?>
```

第一次访问：

<p>Hello, you. This may be your first visit.</p>

第二次访问：

<p>Hello again! You have chosen: artichoke</p>

**删除一个 cookie**

使用一个确定已经过期的时间，还要确保传递给 `setcookie()` 与最初设置 `cookie` 时候所使用的是相同的路径、域和安全参数。
```php
<?php
setcookie("vagetable", "artichoke", time()-60, "/", "localhost", 0);
```

## 12.3 会话函数概览

n/a

## 12.4 开始一个会话

**启动会话的两种方式：**

- 在配置文件 `php.ini` 中设置 `session.auto_start = 1`，这样可以确保一个会话针对每个 PHP 脚本而启动。
- 在每个脚本中调用 `session_start()` 函数。

http://php.net/session_start \
http://php.net/session_id

**程序清单 12.2 开始或继续一个会话**
```php
<?php
session_start();
echo "<p>Your session ID is ".session_id()."</p>";
?>
```

结果：

<p>Your session ID is 3bf0afd6990fb62e3d17c81c72770dca</p>

**注意：**
- **第一次初始化一个会话的时候，由于 `session_start()` 尝试设置一个 cookie，因此必须在向浏览器输出任何内容之前调用这个函数。**
- **只要浏览器是激活的，会话将一直保持。如果重新打开浏览器，cookie 将不再存储。**
- **在配置文件中可以修改 `session.cookie_lifetime` 的值，设置 cookie 的过期周期。**

## 12.5 使用会话变量

http://php.net/$_SESSION

**可以在超全局变量 `$_SESSION` 中存储任意多个变量，然后在任何支持会话的页面上访问它们。**

**程序清单 12.3 在一个会话中存储变量**
```php
<?php
session_start();
$_SESSION['product1'] = "Sonic Screwdriver";
$_SESSION['product2'] = "HAL 2000";
echo "The products have been registered.";
?>
```

结果：

The products have been registered.

在同一个浏览器打开以下页面

**程序清单 12.4 访问存储的会话变量**
```php
<?php
session_start();
?>
<p>Your chosen products are:</p>
<ul>
    <li><?php echo $_SESSION['product1']; ?></li>
    <li><?php echo $_SESSION['product2']; ?></li>
</ul>
```

结果：

<p>Your chosen products are:</p>
<ul>
    <li>Sonic Screwdriver</li>
    <li>HAL 2000</li>
</ul>

**在幕后，PHP 把会话变量写入到一个临时文件，函数 `session_save_path()` 可以查看这个文件保存在系统上的什么地方。**

http://php.net/session_save_path

```php
<?php
session_start();
$_SESSION['product1'] = "Sonic Screwdriver";
$_SESSION['product2'] = "HAL 2000";
echo "The products have been registered.<br>";
// 打印临时文件保存的路径
echo session_save_path();
?>
```

结果：

The products have been registered.<br>/Applications/MAMP/tmp/php

临时文件的内容
```bash
$ cat /Applications/MAMP/tmp/php/sess_d7aea7c7dff0697f792bab9df3cf0712 
product1|s:17:"Sonic Screwdriver";product2|s:8:"HAL 2000";
```

**把一个数组添加到会话变量中**

http://php.net/unserialize \
http://php.net/serialize \
http://php.net/array_merge \
http://php.net/array_unique

**程序清单 12.5 把一个数组变量添加到一个会话变量中**

`arraysession.php`
```php
<?php
session_start();
?>
<!DOCTYPE html>
<html>
<head>
    <title>Storing an array with a session</title>
</head>
<body>
    <h1>Product Choice Page</h1>

    <?php
    if (isset($_POST['form_products'])) {
        if (!empty($_SESSION['products'])) {
            // 反序列化数组，并添加新的值到数组中。
            $products = array_unique( array_merge( unserialize($_SESSION['products']), $_POST['form_products'] ) );
            // 序列化数组，修改会话变量的值。
            $_SESSION['products'] = serialize($products);
        } else {
            // 序列化数组，初始化会话变量的值。
            $_SESSION['products'] = serialize($_POST['form_products']);
        }
        echo "<p>Your products have been registered!</p>";
    }
    ?>

    <form method="post" action="<?php echo $_SERVER['PHP_SELF']; ?>">
        <p>
            <label>Select some products:</label><br>
            <select id="form_products" name="form_products[]" multiple="multiple" size="3">
                <option value="Sonic Screwdriver">Sonic Screwdriver</option>
                <option value="Hal 2000">Hal 2000</option>
                <option value="Tardis">Tardis</option>
                <option value="ORAC">ORAC</option>
                <option value="Transporter bracelet">Transporter bracelet</option>
            </select>
        </p>
        <button type="submit" name="submit" value="choose">Submit Form</button>
    </form>
    <p>
        <a href="session1.php">go to content page</a>
    </p>
</body>
</html>
```

**程序清单 12.6 访问会话变量**

`session1.php`
```php
<?php
session_start();
?>
<!DOCTYPE html>
<html>
<head>
    <title>Accessing session variables</title>
</head>
<body>
    <h1>Content Page</h1>
    <?php
    if (isset($_SESSION['products'])) {
        echo "<strong>Your cart:</strong>\n";
        echo "\t<ol>\n";
        foreach (unserialize($_SESSION['products']) as $product) {
            echo "\t\t<li>".$product."</li>\n";
        }
        echo "\t</ol>\n";
    }
    ?>
    <p><a href="arraysession.php">return to product choice page</a></p>
</body>
</html>
```

## 12.6 销毁会话和重置变量

`session_destroy()` 函数可以用来销毁一个会话，删除保存在服务器上的会话文件，但不会立刻删除已注册的变量，因为仍保存在内存中。
```php
<?php
session_start();
$_SESSION['test'] = 5;
// 销毁会话会删除服务器上的 session 文件
session_destroy();
// 仍然可以访问已注册的变量，因为还保留在内存当中。
echo $_SESSION['test'];
?>
```

要从一个会话中删除所有已注册变量，需要重置变量。
```php
<?php
session_start();
$_SESSION['test'] = 5;
session_destroy();
// 重置变量也就是从内存中重置
unset($_SESSION['test']);
echo $_SESSION['test'];
?>
```

**延伸阅读：**

`session_unset()` 释放当前在内存中已经创建的所有 `$_SESSION` 变量，但不删除 session 文件以及不释放对应的 sessionid。

`session_destroy()` 删除当前用户对应的 `session` 文件以及释放 `sessionid`，内存中的 `$_SESSION` 变量内容依然保留。

http://php.net/session_destroy \
http://php.net/session_unset

因此，释放用户的所有 session 资源，需要顺序执行如下代码：
```php
<?php
session_start();
$_SESSION['user'] = 'wangh';
session_unset();
session_destroy();
?>
```

## 12.7 在一个带有注册用户的环境中使用会话

n/a

学习结束

复习 2018.3.5