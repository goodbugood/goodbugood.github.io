# PHP输出控制函数


## 输出控制

| 函数            | 清空缓冲区 | 刷出缓冲区内容 | 返回缓冲区内容 | 关闭缓冲区 |
| --------------- | ---------- | -------------- | -------------- | ---------- |
| ob_clean        | 是         | 否             | 否             | 否         |
| ob_end_clean    | 是         | 否             | 否             | 是         |
| ob_end_flush    | 否         | 是             | 否             | 是         |
| ob_flush        | 否         | 是             | 否             | 否         |
| ob_get_clean    | 是         | 否             | 是             | 是         |
| ob_get_contents | 否         | 否             | 是             | 否         |
| ob_get_flush    | 否         | 是             | 是             | 是         |

**注意:**

- 输出缓冲区的内容一旦被刷出后, 将不再存在
