URL函数中$opts['base_uri']未定义
====================
core\context.php 1171行
$url    = strlen($opts['base_uri']) > 0
改为
$url    = isset($opts['base_uri'])&&strlen($opts['base_uri']) > 0


多表关联时分表setColumns
========================
>在关联联表查询的时候，需要指定每个表的查询字段。例如：Users::find($cond)->setColumns("username", Users::meta()->table->name)->setColumns("avatar,nickname,", UserDetail::meta()->table->name)->getOne()

db/select.php 431行
注释掉整行，即 $this->_parts[self::COLUMNS] = array();


MySQL查询adpter中字段类型映射不完善
========================
>在取MySQL表字段信息回来，与内部做映射的时候，内置的$type_mapping字段map表没有unsigned及其它信息，当数据库字段有的时候，会产生数组成员undefined的错误。
library\db\adapter\mysql.php 365行

$type = strtolower($row['type']);
修改为
//取字段类型 第一个字段类型 描述信息 抛弃后面的字段属性 例如:float unsigned 取float 
$row_type = explode(" ",$row['type']); 
$type = strtolower($row_type[0]);