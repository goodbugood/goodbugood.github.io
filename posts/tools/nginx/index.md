# Nginx


## 常见问题

### nginx 用 root 用户启动,仍然遇到 403

```bash
/usr/sbin/sestatus -v
```

解决办法,关闭

```bash
setenforce 0
```
