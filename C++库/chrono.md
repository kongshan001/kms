## 学习目标

## 学习实践
- [ ] 基于RAII实现函数调用耗时统计工具

## std::chrono::steady_clock
- 单调性（Monotonic）：steady_clock是一个单调递增的时钟，不会因为系统时间调整（手改、时区切换、NTP同步等）导致回退或跳跃
- 稳定性（Steadiness）：steady_clock的时钟频率稳定，其时间间隔的物理意义与实际时间一致，即使CPU频率动态调整（休眠、节能等），其计数依然可靠
- 在耗时测量中，还有其他两种可选方案
  - system_clock：无法确保单调性
  - high_resolution_clock：根据文章中(https://en.cppreference.com/w/cpp/chrono/high_resolution_clock)中提到，该类在不同的编译器实现中可能是system_clock的别名，也有可能是steady_clock的别名，或者是其他第三方库中有关时间计数的别名，所以更不推荐使用


## 基于RAII实现函数调用耗时统计工具
- 需求
  - [x] 采用宏`ENABLE_SCOPE_TIMER`来控制是否开启功能，默认情况下在发布端不开启
  - [x] 采用宏`SCOPE_TIME`支持用户来使用函数调用耗时功能，在`ENABLE_SCOPE_TIMER`未开启时让调用耗时降低
  - [x] 使用RAII机制进行统计函数耗时
  - [x] 支持根据当前耗时来控制显示单位，例如1000毫秒会显示1秒
  - [ ] 提供相关的单元测试
- 接入示例
```cpp
void test1() {
    SCOPE_TIME("test1");
    std::this_thread::sleep_for(std::chrono::seconds(1));
}

```
- 源码链接：https://github.com/kongshan001/scopetime