# Docker配置

## Docker Engine
```json
{
  "builder": {
    "gc": {
      "defaultKeepStorage": "20GB",
      "enabled": true
    }
  },
  "experimental": false,
  "registry-mirrors": [
    "https://hub.uuuadc.top",
    "https://docker.anyhub.us.kg",
    "https://dockerhub.jobcher.com",
    "https://dockerhub.icu",
    "https://docker.ckyl.me",
    "https://docker.awsl9527.cn"
  ]
}
```

## Mysql配置
```bash
#配置本地持久化
-v /my/data:/var/lib/mysql

#配置密码
-e MYSQL_ROOT_PASSWORD=

#本地化配置文件
-v /my/custom:/etc/mysql/conf.d

```