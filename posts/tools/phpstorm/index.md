# PHPStorm


## 编码格式约定

| 位置                                  | 调整                                                         |
| ------------------------------------- | ------------------------------------------------------------ |
| Code Generation                       | ![image-20220823142223251](./images/image-20220823142223251.png) |
| Wrapping and Braces/Array initializer | 不要勾选`Align key-value pairs`，即数组元素对齐。            |

其他保持默认即可。

## 常用插件整理

- Composer Dump-Autoload
- IDE Eval Reset
- PHP Annotations
- Swagger
- Git，可以使用代码版本控制
- Terminal，使得可以在 PHPStorm 中使用 命令行工具

### 代码规范检查工具 PHP CodeSniffer

打开配置 `File > Settings > Language & Frameworks > PHP > Quality Tools`

点开配置

![image-20220818120916983](./images/image-20220818120916983.png)

配置 Composer 安装的扩展

```sh
composer.phar global require "squizlabs/php_codesniffer=*"
```

安装完对应的脚本路径为：

> C:\Users\Shali\AppData\Roaming\Composer\vendor\bin

![image-20220818144314961](./images/image-20220818144314961.png)

配置工具的路径：

![image-20220818121533844](./images/image-20220818121533844.png)

### 配置测试框架

![image-20220818121657473](./images/image-20220818121657473.png)

## 常用快捷键

### 光标控制

| 快捷键         | 描述                     |
| -------------- | ------------------------ |
| Ctrl+Enter     | 往上新增一行             |
| Shift+Enter    | 往下新增一行             |
| Shift+Delete   | 删除当前行               |
| Ctrl+D         | 复制当前行               |
| Ctrl+Backspace | 删除一个单词             |
| Alt+Insert     | 自动生成set/get/test方法 |
| Ctrl+B         | 追踪变量定义             |

### 格式化

| 快捷键     | 描述                                               |
| ---------- | -------------------------------------------------- |
| Ctrl+Alt+L | 自动格式化整个文件，也可选中部分代码，仅部分格式化 |
| Ctrl+Alt+O | 优化导入，使用的类自动导包，未使用的包自动移除     |

### 附加功能

| 快捷键                 | 描述                                                   |
| ---------------------- | ------------------------------------------------------ |
| F11                    | 添加代码书签                                           |
|                        | 添加喜爱Favorites<br/>标签页Tab，右键 Add to Favorites |
| Ctrl+Shift+A/双击Shift | 快速搜索功能                                           |

## 自定义快捷键习惯

| 原快捷键   | 自定义快捷键 | 搜索关键词                        | 功能描述         |
| ---------- | ------------ | --------------------------------- | ---------------- |
| Ctrl+S     | Alt+S        | Save All                          | 保存所有文件     |
|            | Alt+X        | Edit>Macros>Start Macro Recording | 运行单元测试方法 |
| Ctrl+Alt+V | Alt+A        | Introduce Variable                | 自动生成变量     |

## 版本跟踪

### SVN

开启当前当前项目的 SVN。

File -> Settings -> Version Control -> 点击右侧弹出窗口的 + 添加工程映射。

![image-20220920114022125](./images/image-20220920114022125.png)

