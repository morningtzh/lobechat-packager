# lobechat-packager
本项目用于[Lobechat](https://github.com/lobehub/lobe-chat)私有镜像打包，注入构建时配置，以解决例如Clerk部署问题。

通过 Github Action 获取 Lobechat 最新 tag，用于打包。目前使用新的 Dockerfile.database 构建镜像，可以在每次启动时进行migrate

## 构建

在镜像构建时注入 secret，配置方式见 [使用服务端数据库部署](https://lobehub.com/zh/docs/self-hosting/advanced/server-database)

```
# 指定服务模式为 server
NEXT_PUBLIC_SERVICE_MODE=server

# Postgres 数据库 URL
DATABASE_URL=
KEY_VAULTS_SECRET=jgwsK28dspyVQoIf8/M3IIHl1h6LYYceSYNXeLpy6uk=

# Clerk 相关配置
NEXT_PUBLIC_CLERK_PUBLISHABLE_KEY=pk_live_xxxxxxxxxxx
CLERK_SECRET_KEY=sk_live_xxxxxxxxxxxxxxxxxxxxxx
CLERK_WEBHOOK_SECRET=whsec_xxxxxxxxxxxxxxxxxxxxxx

# S3 相关配置
# S3 秘钥
S3_ACCESS_KEY_ID=9998d6757e276cf9f1edbd325b7083a6
S3_SECRET_ACCESS_KEY=55af75d8eb6b99f189f6a35f855336ea62cd9c4751a5cf4337c53c1d3f497ac2

# 存储桶的名称
S3_BUCKET=lobechat
# 存储桶的请求端点
S3_ENDPOINT=https://0b33a03b5c993fd2f453379dc36558e5.r2.cloudflarestorage.com
# 存储桶对外的访问域名
NEXT_PUBLIC_S3_DOMAIN=https://s3-for-lobechat.your-domain.com
# 桶的区域，如 us-west-1，一般来说不需要添加，但某些服务商则需要配置
# S3_REGION=us-west-1
```

## 运行

部署时需要同样注入S3和database相关环境变量。

```
# Postgres 数据库 URL
DATABASE_URL=
KEY_VAULTS_SECRET=jgwsK28dspyVQoIf8/M3IIHl1h6LYYceSYNXeLpy6uk=

# S3 相关配置
# S3 秘钥
S3_ACCESS_KEY_ID=9998d6757e276cf9f1edbd325b7083a6
S3_SECRET_ACCESS_KEY=55af75d8eb6b99f189f6a35f855336ea62cd9c4751a5cf4337c53c1d3f497ac2

# 存储桶的名称
S3_BUCKET=lobechat
# 存储桶的请求端点
S3_ENDPOINT=https://0b33a03b5c993fd2f453379dc36558e5.r2.cloudflarestorage.com
# 存储桶对外的访问域名
NEXT_PUBLIC_S3_DOMAIN=https://s3-for-lobechat.your-domain.com
```

## 常见问题

以往通过 db:push 创建的数据库，需要手动migrate。见 [Server DB Docker Image Feedback | 服务端 Database Docker 镜像问题反馈](https://github.com/lobehub/lobe-chat/issues/3391#issuecomment-2289088921)

1. 创建容器, 修改启动命令
2. 进入容器
3. 使用 `--` 注释一切遇到的问题，以下是经验修改，后续版本可能不同
  1. `/app/migrations/0001_add_client_id.sql` 和 `/app/migrations/0002_amusing_puma.sql` 所有sql语句
  2. `/app/migrations/0004_add_next_auth.sql` 中的 `email_verified_at` 和 `full_name` 新增字段
4. 手动执行 `node /app/docker.cjs` 确保成功，否则按 3 进行注释
5. 恢复注释，再次执行 `node /app/docker.cjs` 会发现没有任何障碍。


