#Check integer type of PHP

##1, 0 -1 => Integer
```PHP
<?php

echo gettype(1) . PHP_EOL;
echo gettype(0) . PHP_EOL;
echo gettype(-1) . PHP_EOL;
```
##terminal
```Bash
sudo docker compose exec app php sample.php

//result
integer
integer
integer
```

##2, 8, 16進法
```PHP
var_dump(0b10); //2進数
var_dump(010); //8進数
var_dump(0x10); //16進数
```

```Bash
int(2)
int(8)
int(16)
```

Bug of Integer Example
```PHP
var_dump((PHP_INT_MAX + 1) === (PHP_INT_MAX + 2));
```
```Bash
bool(true)
```







