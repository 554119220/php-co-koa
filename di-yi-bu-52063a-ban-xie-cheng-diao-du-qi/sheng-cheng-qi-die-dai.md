## 生成器迭代

手动迭代生成器，递归执行next，调用send方法将将yield值作为yield表达式结果。(yield表达式可能是一个异步调用，我们之后会把异步调用的结果作为yield表达式结果。)

```
如, $ip = (yield async_dns_lookup(...)  );
     ^          |--------------------|
     |                  yield值
     |   |---------------------------|
yield表达式结果       yield 表达式
```


```php
<?php

final class AsyncTask
{
    public $gen;

    public function __construct(\Generator $gen)
    {
        $this->gen = new Gen($gen);
    }

    public function begin()
    {
        $this->next();
    }

    public function next($result = null)
    {
        $value = $this->gen->send($result);
        if ($this->gen->valid()) {
            $this->next($value);
        }
    }
}

function newGen()
{
    $r1 = (yield 1);
    $r2 = (yield 2);
    echo $r1, $r2;
}
$task = new AsyncTask(newGen());
$task->begin(); // output: 12

```