### 建造者模式（Builder Pattern）学习笔记

---

#### ✅ 什么是建造者模式？

**建造者模式（Builder Pattern）** 是一种 **创建型设计模式**，用于构建复杂对象，并将构建过程与表示(构建的结果/最终的对象配置)分离

- 主要目的是 **解耦对象的构造逻辑与其具体实现**
- 允许通过相同的构建流程生成不同的对象配置
- 常用于避免“伸缩构造器反模式([README.md](README.md)中有解释)”

```java
// 示例：传统构造器参数爆炸问题
public Hero(Profession profession, String name, HairType hairType, HairColor hairColor, Armor armor, Weapon weapon) {}
```

---

#### 🧱 建造者模式结构组成


| 组成部分             | 描述                                     |
|------------------|----------------------------------------|
| `Product`        | 最终构建的对象                                |
| `Builder`        | 定义构建步骤的接口或抽象类,推荐做法：一般作为`Product`的静态内部类 |

---

#### 🛠️ 为什么使用建造者模式？


| 优势              | 说明                                                   |
| ----------------- | ------------------------------------------------------ |
| ✅ 避免构造器污染 | 构造函数参数过多时难以维护，Builder 提供清晰的链式调用 |
| ✅ 支持默认值     | 可在 Builder 中设置字段默认值                          |
| ✅ 更好的封装性   | 构建细节对外部隐藏，只暴露 build() 方法                |
| ✅ 支持多种配置   | 同一个 Builder 可构建不同配置的对象                    |
| ✅ 不可变对象支持 | 构建完成后返回不可变对象（final 字段）                 |
| ✅ 易于扩展       | 新增字段无需改动已有构建逻辑               |

---

#### 🔍 实际案例分析

以 [SmartWatchHealthConfig](src/main/java/com/niudada/SmartWatchHealthConfig.java) 为例：

```java
SmartWatchHealthConfig config = new SmartWatchHealthConfig.Builder("XXX Watch Pro")
        .setHeartRateMonitoring("实时")
        .setBloodOxygenMonitoring("每小时一次")
        .enableSleepTracking(true)
        .enableStepCounting(true)
        .enableGpsTracking(true)
        .setDataSyncFrequency(5)
        .enableHealthAlerts(true)
        .build();
```

**特点：**

- Builder 类中定义了字段默认值（如 `heartRateMonitoring = "off"`）
- 构建过程中做了参数校验（如设备型号不能为空）
- 构建出的对象是 final 的、不可变的
- 每个设置方法都返回 this，实现链式调用（无需 Lombok）

---

#### ⚖️ 和 Lombok 注解的区别：@Accessors(chain = true)


| 特性     | 使用 Lombok`@Accessors(chain = true)`              | 使用 Builder 模式        |
| -------- |--------------------------------------------------| ------------------------ |
| 实现方式 | 自动生成 setter 返回 this                              | 手动编写 Builder 类      |
| 默认值   | 不支持                                              | Builder 可预设           |
| 参数验证 | 需手动添加                                            | Builder 构造函数统一处理 |
| 不可变性 | 对象字段可变(默认是为可变对象设计的，不会阻止你写final,但是写了就会导致setter报错) | 构建后对象不可变         |
| 使用场景 | 简单 POJO、DTO                                      | 复杂对象构建、配置化对象 |

---

#### 📝 总结：什么时候该用 Builder？

✅ 推荐使用 Builder 模式的情况：

- 对象字段多、构造过程复杂
- 需要默认值或校验逻辑
- 构建过程需要复用或封装
- 要求对象不可变
- 避免构造器参数爆炸问题

❌ 不推荐使用 Builder：

- 对象简单、字段少
- 不关心默认值和校验
- 快速开发、POJO 场景，可结合 Lombok 简化代码

---

> 💡 **一句话总结**：
> **建造者模式适用于构建复杂对象，提供良好的封装性和可扩展性；而 Lombok 的 `@Accessors(chain = true)` 更适合简单对象快速链式调用。两者各有所长，按需选择。**
>
