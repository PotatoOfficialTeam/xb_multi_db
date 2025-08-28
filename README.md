# 数据库管理系统

本项目使用 Docker Compose 管理两个独立的 MySQL 5.7 数据库实例。

## 数据库信息

### V2WORD 数据库
- **容器名**: v2word_mysql
- **端口**: 3307 (映射到容器内 3306)
- **数据库名**: v2word
- **用户名**: v2word_user

### WUJIE 数据库  
- **容器名**: wujie_mysql
- **端口**: 3308 (映射到容器内 3306)
- **数据库名**: wujie
- **用户名**: wujie_user

## 安全特性

1. **强密码**: 使用 OpenSSL 生成的 32 字节 Base64 编码密码
2. **环境变量**: 敏感信息存储在 .env 文件中
3. **网络隔离**: 使用自定义 Docker 网络
4. **配置优化**: 禁用不安全的功能，启用慢查询日志

## 使用方法

### 启动服务
```bash
cd database
docker-compose up -d
```

### 停止服务
```bash
docker-compose down
```

### 查看日志
```bash
docker-compose logs -f v2word-db
docker-compose logs -f wujie-db
```

### 备份数据库
```bash
# 备份 v2word 数据库
docker exec v2word_mysql mysqldump -u root -p v2word > v2word_backup.sql

# 备份 wujie 数据库
docker exec wujie_mysql mysqldump -u root -p wujie > wujie_backup.sql
```

### 连接数据库
```bash
# 连接 v2word 数据库
mysql -h localhost -P 3307 -u v2word_user -p v2word

# 连接 wujie 数据库
mysql -h localhost -P 3308 -u wujie_user -p wujie
```

## SQL 备份恢复

### V2WORD 数据库恢复
1. 将备份文件复制到对应目录：
   ```bash
   cp your_v2word_backup.sql database/scripts/v2word/
   ```

2. 启动 V2WORD 数据库：
   ```bash
   cd database
   docker-compose up -d v2word-db
   ```

### WUJIE 数据库恢复  
1. 将备份文件复制到对应目录：
   ```bash
   cp your_wujie_backup.sql database/scripts/wujie/
   ```

2. 启动 WUJIE 数据库：
   ```bash
   cd database
   docker-compose up -d wujie-db
   ```

### 重新初始化
如果需要重新导入备份：
```bash
# 停止服务并删除数据卷
docker-compose down -v

# 重新启动服务
docker-compose up -d
```

## 文件结构
```
database/
├── docker-compose.yml    # Docker Compose 配置
├── .env                 # 环境变量 (敏感信息)
├── .gitignore           # Git 忽略文件
├── config/              # 数据库配置
│   ├── v2word.cnf
│   └── wujie.cnf
├── data/                # 数据存储目录
│   ├── v2word/
│   └── wujie/
├── logs/                # 日志目录
│   ├── v2word/
│   └── wujie/
└── scripts/             # 初始化脚本目录
    ├── v2word/          # V2WORD 数据库脚本
    └── wujie/           # WUJIE 数据库脚本
```

## 安全建议

1. **定期更换密码**: 建议每 3-6 个月更换一次密码
2. **防火墙设置**: 仅允许必要的 IP 访问数据库端口
3. **SSL/TLS**: 生产环境建议启用 SSL 连接
4. **备份策略**: 建立定期备份和灾难恢复计划
5. **监控**: 监控数据库性能和异常访问

## 端口映射

- **3307**: V2WORD 数据库端口
- **3308**: WUJIE 数据库端口

请确保这些端口在服务器防火墙中正确配置。