# database-schema-dump

生成一个可重复运行的 SQL 脚本（`migration_script.sql`），安全地创建用户定义的 schema、类型、序列、同义词和可编程对象（视图、存储过程、函数、触发器），支持多种数据库（SQL Server、Azure SQL 等）。

## 支持的数据库及结构

- **当前支持：** Azure SQL Server（**注意：** 本工具比 `mssql-scripter` 更稳定。）
- **计划支持：** 所有主流 SQL 数据库（MySQL、PostgreSQL、Oracle、SQLite 等）

### 新结构

每种支持的数据库类型都有自己的子文件夹（如 `azure-sql-server/`、`mysql/`、`postgresql/` 等）。

- 每种数据库类型的脚本和代码都隔离在各自的子文件夹中。
- 只在对应子文件夹中安装所需依赖，避免不必要的包安装。
- 这样可以简化迁移脚本的编写和运行，避免依赖冲突。

## TODO

- [ ] 添加 MySQL、PostgreSQL、Oracle、SQLite 等子文件夹
- [ ] 支持 MySQL
- [ ] 支持 PostgreSQL
- [ ] 支持 Oracle
- [ ] 支持 SQLite
- [ ] 改进迁移脚本生成
- [ ] 添加自动化测试
- [ ] 加强文档

## 前置条件
- Python 3.9+

## 安装步骤
1. 进入目标数据库的子文件夹（如 `cd azure-sql-server`）。
2. 根据 `.env.example` 创建并填写 `.env`（如有）。
3. 安装该数据库类型所需的 Python 依赖：

```bash
python -m pip install -r requirements.txt
```

## 运行
```bash
python generate_migration.py
```
将在当前子文件夹下生成 `migration_script.sql`。

## 说明
- 脚本按如下顺序生成对象：schemas → 类型（别名、表类型）→ 序列 → 同义词 → 视图/存储过程/函数/触发器，以减少依赖错误。
- 对于用 ALTER 定义的模块，脚本会重写为 CREATE。
- 输出脚本可多次安全重复运行。
- 每个子文件夹都是针对其数据库类型的自包含环境。

## 限制
- 不包含表、数据、角色、权限或索引（除表类型中的主键定义外）。
- 对模块间复杂依赖排序支持有限，通常顺序已足够，但极少数情况下可能需手动调整。
