# lobechat-packager
本项目用于[Lobechat](https://github.com/lobehub/lobe-chat)私有镜像打包，注入构建时配置，以解决例如Clerk部署问题。

通过 Github Action 获取 Lobechat 最新 tag，用于打包。

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
