# V2WORD 数据库初始化脚本

将您的 V2WORD 数据库备份文件放在此目录中。

## 使用方法

1. 将 SQL 备份文件复制到此目录：
   ```bash
   cp your_v2word_backup.sql database/scripts/v2word/
   ```

2. 启动数据库服务：
   ```bash
   cd database
   docker-compose up -d v2word-db
   ```

## 注意事项

- 只有 `.sql` 文件会被执行
- 文件按字母顺序执行
- 只在容器首次启动时执行
- 如需重新初始化，删除数据卷：`docker-compose down -v`