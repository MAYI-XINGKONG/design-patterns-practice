# 📘 适配器模式学习文档（基于翻译官案例）

> 文档名称：适配器模式实践 - 翻译官场景
> 
> 难度等级：初级
> 
> 示例类型：对象适配器（Object Adapter）

---

## 一、什么是适配器模式？

### 定义：

适配器模式（Adapter Pattern）是一种**结构性设计模式**，它允许将一个类的接口转换为客户期望的另一个接口。通过适配器，原本由于接口不兼容而不能一起工作的那些类可以协同工作。

### 别名：

- 包装器（Wrapper）
- 接口转换器

---

## 二、适配器模式的组成角色


| 角色      | 含义                                    | 本项目中的对应                                                                          |
| --------- | --------------------------------------- |----------------------------------------------------------------------------------|
| `Target`  | 客户端期望使用的接口                    | [EnglishSpeaker](src/main/java/com/niudada/EnglishSpeaker.java)                  |
| `ChineseSpeaker` | 已有的类，其接口与目标不兼容            | [ChineseSpeaker](src/main/java/com/niudada/ChineseSpeaker.java)                  |
| `Adapter` | 实现目标接口，并内部调用 Adaptee 的方法 | [ChineseToEnglishAdapter](src/main/java/com/niudada/ChineseToEnglishAdapter.java) |
| `Client`  | 使用目标接口的客户端代码                | [Main.java](src/main/java/com/niudada/Main.java)                                 |

---

## 三、翻译官业务场景描述

### 场景背景：

你是一个国际会议的组织者，参会人员来自不同国家，说不同的语言（如中文、英文、法语等）。为了沟通顺畅，你需要引入翻译官来帮助大家交流。

### 模式映射：


| 业务元素     | 模式角色           |
| ------------ |----------------|
| 英文听众     | Client         |
| 中文发言者   | ChineseSpeaker |
| 翻译官       | Adapter        |
| 英文表达能力 | Target         |

### ✅ 该模式Demo核心类图

```
[Client] ----(users)---> [EnglishSpeaker]
                         ↑
                      [Adapter]
                         ↓
                    [ChineseSpeaker]
```

---

## 四、代码实现详解

### 1. 目标接口 [EnglishSpeaker](src/main/java/com/niudada/EnglishSpeaker.java)

```java
public interface EnglishSpeaker {
    void speakEnglish();
}
```

- 表示客户希望听到的接口。
- 所有适配器都需要实现这个接口。

---

### 2. 被适配者 [ChineseSpeaker](src/main/java/com/niudada/ChineseSpeaker.java)

```java
public class ChineseSpeaker {
    public void speakChinese() {
        System.out.println("我说中文");
    }
}
```

- 提供了 `speakChinese()` 方法，但没有提供 `speakEnglish()`。
- 接口不兼容，需要适配。

---

### 3. 适配器 [ChineseToEnglishAdapter](src/main/java/com/niudada/ChineseToEnglishAdapter.java)

```java
public class ChineseToEnglishAdapter implements EnglishSpeaker {

    private final ChineseSpeaker chineseSpeaker;

    public ChineseToEnglishAdapter(ChineseSpeaker chineseSpeaker) {
        this.chineseSpeaker = chineseSpeaker;
    }

    @Override
    public void speakEnglish() {
        chineseSpeaker.speakChinese(); // 调用原始方法
        System.out.println("（翻译成英文：I speak Chinese）");
    }
}
```

- 实现了 `EnglishSpeaker` 接口。
- 内部组合了一个 `ChineseSpeaker` 对象。
- 将中文发言“适配”为英文输出。

---

### 4. 客户端调用代码

```java
public class Main {
    public static void main(String[] args) {
        ChineseSpeaker chinesePerson = new ChineseSpeaker();
        EnglishSpeaker englishPerson = new ChineseToEnglishAdapter(chinesePerson);

        System.out.println("会议开始...");
        englishPerson.speakEnglish();
    }
}
```

- 客户端只关心 `EnglishSpeaker` 接口。
- 不管实际是谁在说话，只要实现了这个接口就能参与交流。

---

### 输出结果：

```
会议开始...
我只会中文!谁没事说英文啊...
（翻译成英文：I only speak Chinese! Who talks in English for no reason... ）
```

---

## 五、适配器模式的优点


| 优点           | 说明                                 |
| -------------- | ------------------------------------ |
| **解耦合**     | 客户端只依赖统一接口，不依赖具体实现 |
| **复用已有类** | 不修改原有类即可将其用于新场景       |
| **提高扩展性** | 新增其他语言适配只需新增适配器类     |
| **隐藏差异**   | 外部逻辑无需感知接口之间的差异       |

---

## 六、适配器模式的缺点


| 缺点                     | 说明                                 |
| ------------------------ | ------------------------------------ |
| **增加复杂度**           | 引入额外的类，使系统变得更复杂       |
| **维护成本上升**         | 如果适配器过多，维护和管理成本会上升 |
| **不适合大范围参数转换** | 若接口差异太大，适配器难以胜任       |

---

## 七、现实世界中的应用案例（Java 标准库）


| 类/方法                     | 用途                                    |
| --------------------------- | --------------------------------------- |
| `Arrays.asList()`           | 将数组转为 List 接口                    |
| `Collections.enumeration()` | 将集合转为枚举                          |
| `XmlAdapter`                | 在 JAXB 中用于 XML 和 Java 对象之间转换 |

---

## 九、总结

适配器模式是一种非常实用的设计模式，适用于整合异构系统、遗留系统、或者多种接口统一调用的场景。

在这个例子中，清晰地展示了：

- 如何将一个不兼容的对象包装成目标接口
- 如何让多个语言的人通过统一接口进行交流
- 如何保持客户端逻辑不变，同时灵活接入新功能
