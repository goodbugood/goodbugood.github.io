# Hugo使用手册


## 编译博客

```sh
hugo
```

执行上述命令即可，重新生成文章。

## 临时预览博客

```sh
hugo server --theme=hyde --buildDrafts
```

- --theme 指定临时主题
- --buildDrafts 草稿也要编译

## 常见报错

### you need the extended version to build SCSS/SASS

由于你使用了 `hugo_0.114.1_windows-amd64`，但是他不支持编译 `SCSS/SASS`，应该下载使用 `hugo_extended_0.114.1_windows-amd64`。

#### 参考

- [Warning “you need the extended version to build SCSS/SASS“ not triggered when it should 的解决办法](https://blog.csdn.net/qq_41136216/article/details/112674327)
