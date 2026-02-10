## Android

## iOS

方案 A：直接删掉 template 关键字（推荐）
在最新的 C++ 标准中，如果编译器能通过上下文推导出这是一个模板函数，或者该函数在基类中定义明确，就不再强制要求使用 template 前缀。

修改前：

```C++
return this->template addEventListener(std::forward<Fn>(func), useCapture, true);
```
修改后：

```C++
return this->addEventListener(std::forward<Fn>(func), useCapture, true);
```
