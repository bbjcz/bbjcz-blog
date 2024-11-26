---
title: "Cpp Timer"
date: 2024-11-26T20:04:02+08:00
draft: false
---
C++测试代码运行时间的代码
---

```cpp
#include <chrono>
class Timer {
private:
    std::chrono::steady_clock::time_point begin_time;
public:
    Timer() :begin_time(std::chrono::steady_clock::now()) {}
    double elapsed() const {
        auto end_time = std::chrono::steady_clock::now();
        auto duration = (end_time - begin_time).count();
        return duration / 1e9;
    }
};
```

使用示例：

```cpp
Timer t;
for (volatile int i = 0; i < int(1e9); i++);
cout << t.elapsed() << endl;
```
