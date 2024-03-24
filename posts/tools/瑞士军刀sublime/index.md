# Sublime Text 3


## 常用命令

### 去除重复行

```sh
Edit > Premute Lines > Unique
```

其实还有其他 2 个操作：

**Reverse**：倒序行

**Shuffle**：打乱行

#### 添加快捷键

添加快捷键，执行先排序，后移除重复行。

- 录制宏，或者自建宏文件

新建文件`C:\Users\Shali\AppData\Roaming\Sublime Text 3\Packages\User\先排序再去除重复行.sublime-macro`

填充如下内容，json 格式：

```json
[
	{
		"args":
		{
			"case_sensitive": false
		},
		"command": "sort_lines"
	},
	{
		"args":
		{
			"operation": "unique"
		},
		"command": "permute_lines"
	}
]
```

**case_sensitive**：标识排序时是否区分大小写，按照 ASCII 码，大写字母排前面。

- 绑定快捷键

添加如下快捷键配置项：

```json
{ "keys": ["ctrl+alt+l"], "command": "run_macro_file", "args": {"file": "Packages/User/先排序再去除重复行.sublime-macro"} },
```


