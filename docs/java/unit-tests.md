# 单元测试 Unit tests

> 参考资料:
>
> - [汪文君Mockito实战视频](https://www.bilibili.com/video/BV1jJ411A7Sv)

## Working with unit tests

A unit test should exhibit the following characteristics:

- It should be automated.  （自动化\持续集成）
- It should have a fast test execution. （执行效率高）
- A test should not depend on the result of another test or rather test execution order. Unit test frameworkds can execute tests in any order.（不依赖于其他的unit test及其结果，不依赖于unit test的执行顺序）
- A test should not depend on database access, file access, or any long running task. Rather, an appropriate test double should isolate the external dependencies.（不依赖于外部资源）
- A test result should be consistent and time-and-location transparent.（在任何时间和环境执行结果都是一样的）
- Tests should be meaningful.（有意义）
- Tests should be short and tests should not be treated as second-class citizens.（简短且与源码同等重要）

## Unit test Qualities

Writing clean, readable, and maintainable unit test cases (`Junit`, `TestNG`) is an art; Just like writing clean code.（如编写整洁的代码一样去写整洁、可读性高、可以维护性的test cases）

Unit tests should adhere to a number of principles（遵循的原则）:

- Should be reliable: A unit test should fail if, and only if, the production code is broken.（如果unit test出现异常，需考虑生产环境是否也会出现）
- Unit tests should be automated
  - Assumptions are continually verified.（持续的验证假设）
  - Side effects are detected immediately. （副作用检测）
  - Saves time with no need for immediate manual regression testing.（节省[回归测试](https://baike.baidu.com/item/%E5%9B%9E%E5%BD%92%E6%B5%8B%E8%AF%95/1925732)的时间）
- Tests should be executed extremely fast provide quick feedback.（高效的执行并反馈）
- Trouble-free setup an run.（无故障启动运行）

## Unit test frameworkds

- []