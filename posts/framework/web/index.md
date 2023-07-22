# Web


web 常用功能：

出/入参格式化：

1. paja 转 json 序列化时，定义格式，`com.fasterxml.jackson.annotation.@JsonFormat(pattern = DatePattern.NORM_DATETIME_PATTERN, timezone = "GMT+8")` 指定了序列化格式以及时区。
2. 格式化控制器请求参数，`org.springframework.format.annotation.@DateTimeFormat(pattern = DatePattern.NORM_DATETIME_PATTERN)`


