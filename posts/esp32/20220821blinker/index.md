# ESP32入门之点灯


学习一门编程语言，第一个教程是输出`hello world`。而学习单片机，第一个教程通常是`点灯`。

## 利用延时函数闪灯

```cpp
void setup()
{
    // 开启串口，并配置波特率
    Serial.begin(115200);
    // 配置灯的管脚模式
    pinMode(BUILTIN_LED, OUTPUT);
    // 高电平灯灭
    digitalWrite(BUILTIN_LED, HIGH);
}

void loop()
{
    delay(1000);
    digitalWrite(BUILTIN_LED, LOW);
    delay(1000);
    digitalWrite(BUILTIN_LED, LOW);
    // 任务
    Serial.println("hello world");
}
```

使用延时函数闪灯，最大的缺点是，后面的任务被阻塞了。就是说灯在延时的过程时，任务不会执行。而任务执行的时候，灯就不会闪烁，灯被任务阻塞了。

而我们希望任务一直执行，一旦闪灯的时间到了，立即闪灯。哈哈，那不就是外部中断吗？给个定时器中断不就好了吗？

## 定时器闪灯

```cpp
// 引入 esp32 定时器中断库
#include <Ticker.h>

// 定义闪灯定时器
Ticker shanTicker;

void shanshuo()
{
    uint8_t status = digitalRead(BUILTIN_LED);
    digitalWrite(BUILTIN_LED, !status);
}

void setup()
{
    Serial.begin(115200);
    // 配置灯的管脚模式
    pinMode(BUILTIN_LED, OUTPUT);
    // 高电平灯灭
    digitalWrite(BUILTIN_LED, HIGH);
    // 开启定时器，到了 1 秒就触发外部中断，执行回调函数 shanshuo
    shanTicker.attach(1, shanshuo);
}

void loop()
{
    // 任务
    Serial.println("hello world");
}
```

这样任务和闪灯，就能交替执行了。记住定时器中断的优先级比任务高。

esp32 是多核的，那我们能不能利用==多核==，让闪灯和任务同时执行。
