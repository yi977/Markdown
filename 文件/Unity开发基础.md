# Unity开发基础



## 1.MonoBehaviour生命周期









# C#基础



## 1.面向对象



### 1.1面向对象基础-抽象

### 案例-表单抽象设计

#### 🧠 抽象思路

表单的类型虽然样式多样，但从交互和功能层面可以抽象为两个核心类型：

- **填空题（Input）**
- **选择题（选择/多选/下拉等 Toggle）**

统一这些类型背后共通的行为逻辑，抽象出通用的基类和接口以便扩展。

#### 🏗️ 基础抽象结构

#### `BaseAnswer`（基础答题逻辑）

所有答题控件（输入、选择）都继承自此类，包含统一行为：

```c#
class BaseAnswer : IHintInterface {
    // 1. 接收题目与答案数据
    public virtual void SetData(QuestionData data);

    // 2. 答案判断
    public virtual bool CheckAnswer();

    // 3. 按 T 提示（来自 IHintInterface）
    public virtual void HintAnswer();

    // 4. 错误提示
    public virtual void ShowErrorTip();

    // 5. 输入监听（用于答题状态联动）
    public virtual void RegisterInputListener();

    // 6. 禁用/启用
    public virtual void SetInteractable(bool state);

    // 7. 重置状态
    public virtual void Reset();
}
```

## 📥 输入类（`InputAnswer`）

```c#
class InputAnswer : BaseAnswer {
    override void SetData(QuestionData data) {
        // 设置输入框文本、标题、宽度自适应
    }

    override bool CheckAnswer() {
        // 判断用户输入与正确答案是否一致
    }

    override void HintAnswer() {
        // 将正确答案填入输入框
    }

    override void ShowErrorTip() { ... }
    override void RegisterInputListener() { ... }
    override void SetInteractable(bool state) { ... }
    override void Reset() { ... }
}
```

------

## ✅ 选择类（`ToggleAnswer`）

```c#
class ToggleAnswer : BaseAnswer {
    override void SetData(QuestionData data) {
        // 创建选项 Toggle，根据数据渲染
    }

    override bool CheckAnswer() {
        // 判断选择项与答案是否一致
    }

    override void HintAnswer() {
        // 自动勾选正确项，取消错误项
    }

    override void ShowErrorTip() { ... }
    override void RegisterInputListener() { ... }
    override void SetInteractable(bool state) { ... }
    override void Reset() { ... }
}
```

------

## 🧩 表单容器（`FormDialog`）

```c#
class FormDialog 
{
    InputAnswer inputAnswer;
    ToggleAnswer toggleAnswer;

    void CheckAllAnswers() {
        inputAnswer.CheckAnswer();
        toggleAnswer.CheckAnswer();
    }
}
```

------

## 💡 提示功能抽象：`IHintInterface`

```c#
interface IHintInterface {
    void HintAnswer();
}
```

> 不仅仅是答题表单可用，**拖拽题 (Drag)**、**工具题 (Tool)** 等也可复用该接口。
