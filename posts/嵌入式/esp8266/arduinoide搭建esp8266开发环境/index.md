# ArduinoIDE搭建ESP8266开发环境


## 配置开发板库

添加 esp8266 库：`https://arduino.esp8266.com/stable/package_esp8266com_index.json`

### 第三方开发板库

参考：https://github.com/arduino/Arduino/wiki/Unofficial-list-of-3rd-party-boards-support-urls

#### ESP8266

ESP8266 Community: https://arduino.esp8266.com/stable/package_esp8266com_index.json

Generic ESP8266 modules
Olimex MOD-WIFI-ESP8266
NodeMCU 0.9 (ESP-12)
NodeMCU 1.0 (ESP-12E)
Adafruit HUZZAH ESP8266 (ESP-12)
SparkFun Thing
SweetPea ESP-210
WeMos D1
WeMos D1 mini

#### 安装速度过慢或失败

1. 先将库索引文件下载到本地：`file:///Users/t470p/Documents/Arduino/package_esp8266com_index.json`

2. 再将索引文件里要下载的文件 `x86_64-apple-darwin14.xtensa-lx106-elf-e5f9fec.220621.tar.gz` 下载到本地，并在文件所在目录运行起 web 服务器。并修改索引文件中的文件下载地址：

```json
{
  "host": "x86_64-apple-darwin",
  "url": "http://127.0.0.1:8080/x86_64-apple-darwin14.xtensa-lx106-elf-e5f9fec.220621.tar.gz",
  "archiveFileName": "x86_64-apple-darwin14.xtensa-lx106-elf-e5f9fec.220621.tar.gz",
  "checksum": "SHA-256:e58d166d4c9925585bea67b69dd82b295d86faaac2fe7c74b26ca61e87249e1d",
  "size": "75923095"
}
```

#### ESP32


