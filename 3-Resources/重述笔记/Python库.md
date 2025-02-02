---
Project: 
Status: 
tags:
  - Resources
Deadline: 
CreateTime: 2024-10-25
Connected:
---

#review

y is the true value in the training set
y-hat(prediction) is the rusult obtained by x through the f(x) model (based on the input feature x)
## Numpy


##  matplotlib.pyplot

### rcParams
`rcParams` 是 Matplotlib 中的一个全局配置字典，用于控制图形的默认样式、属性和行为通过修改 `rcParams` 字典中的值，你可以全局地改变 Matplotlib 的默认设置

```py
plt.rcParams['font.size'] = 12  # 设置字体大小
plt.rcParams['figure.figsize'] = [10, 6]  # 设置默认的图形尺寸
plt.rcParams['lines.linewidth'] = 2  # 设置线条的默认宽度

```

### 绘制
- `plt.scatter` 用于绘制散点图。
- `plt.xlabel` 和 `plt.ylabel` 分别为横轴和纵轴添加标签。
- `plt.title` 设置图表的标题。
- `plt.show` 显示图表。

## With  as

在 Python 中，`with ... as ...` 是**上下文管理器**的一种语法形式，主要用于**管理资源**。当你使用 `with` 语句时，Python 会在进入代码块时自动执行一些初始化操作，并在代码块结束时自动执行清理操作，确保资源被正确释放。常见的用途包括文件读写、锁的管理、网络连接处理等。

在你的代码中：

```python
with tf.GradientTape() as tape:
    # 前向传播，计算损失
    y_pred = model(x)
    loss = mean_squared_error(y, y_pred)
```

### 1. **`with ... as ...` 的作用**
这里的 `with tf.GradientTape() as tape:` 语句的作用是创建一个 TensorFlow 的 **`GradientTape`** 上下文管理器。

- **`tf.GradientTape()`** 是 TensorFlow 提供的一个工具，用来记录你对张量（tensor）所做的操作，尤其是用来计算**梯度**。
- 在 `with` 语句内，所有对张量的操作（如 `model(x)` 和 `mean_squared_error(y, y_pred)`）都会被 `GradientTape` 记录下来。
- 当 `with` 代码块结束时，`GradientTape` 会自动停止记录，之后你可以调用 `tape.gradient()` 方法来计算损失函数相对于模型参数的梯度。

### 2. **`with` 的好处**
使用 `with` 语句的好处在于，它自动管理了 `GradientTape` 的生命周期：
- 当你进入 `with` 块时，`GradientTape` 开始记录。
- 当你离开 `with` 块时，`GradientTape` 会自动清理（释放内存资源）。

### 3. **等价代码（不使用 `with`）**
如果不使用 `with`，你需要手动开始和停止 `GradientTape`，例如：

```python
tape = tf.GradientTape()  # 创建 GradientTape
tape.__enter__()          # 手动进入上下文
y_pred = model(x)
loss = mean_squared_error(y, y_pred)
gradients = tape.gradient(loss, [model.w, model.b])
tape.__exit__(None, None, None)  # 手动退出上下文
```

### 总结
- `with tf.GradientTape() as tape:` 是一种简洁的方式来确保 TensorFlow 的 `GradientTape` 在使用时能够正确管理资源，记录梯度操作并最终计算梯度。
- `with` 语句的核心思想是通过上下文管理器自动管理资源的初始化和清理工作，避免手动管理这些过程的复杂性。
## Tensor
reduce_mean()
gradient
`gradient` 是 `tf.GradientTape` 对象的一个方法，用于**计算张量的梯度**。

#### 用途：

在机器学习模型中，梯度（gradients）表示损失函数（loss function）相对于模型参数（如权重 `w` 和偏置 `b`）的偏导数。通过这些梯度，模型可以知道如何更新参数以减少损失。
assign_sub

`assign_sub` 是 TensorFlow 中张量变量 (`tf.Variable`) 的一个方法，用于**原地更新变量**，即通过**减法**更新变量的值。


## 梯度
是的，梯度确实可以理解为**代价函数的偏导数**。

具体来说，**梯度**是一个向量，表示代价函数（或损失函数）**相对于每个模型参数的变化率**。它告诉我们如果调整模型的参数，代价函数将如何变化。

### 详细解释：
1. **代价函数（Cost Function 或 Loss Function）**：  
   在机器学习中，代价函数是用来衡量模型预测值与真实值之间差异的函数。我们希望通过调整模型的参数，使代价函数的值尽可能小（即最小化误差）。
   
   一个常见的代价函数是**均方误差**（MSE）：
   \[
   J(\theta) = \frac{1}{2m} \sum_{i=1}^{m} (h_\theta(x^{(i)}) - y^{(i)})^2
   \]
   其中，`h_θ(x)` 是模型的预测值，`y` 是真实值，`m` 是样本数。

## TODO
目标函数

2. **偏导数**：  
   偏导数表示函数对某个特定变量的变化率。在多元函数中，比如代价函数涉及多个参数（如 `θ₀` 和 `θ₁`），我们可以对每个参数分别求导，得到该参数的偏导数。偏导数告诉我们：如果只调整某个参数，代价函数的值会如何变化。

   对于线性回归问题，假设代价函数 `J(θ)` 取决于参数 `θ₀` 和 `θ₁`，它的偏导数分别表示：
   - 代价函数对 `θ₀` 的偏导数：\(\frac{\partial J}{\partial \theta_0}\)
   - 代价函数对 `θ₁` 的偏导数：\(\frac{\partial J}{\partial \theta_1}\)

3. **梯度（Gradient）**：  
   梯度是一个由所有偏导数组成的向量。对于每个参数，梯度提供了代价函数在该参数方向上的**变化率**。它告诉我们如何改变每个参数，以最快的方式减小代价函数。  
   
   如果我们用向量表示梯度，假设我们有两个参数 `θ₀` 和 `θ₁`，则梯度可以表示为：
   \[
   \nabla J(\theta) = \left( \frac{\partial J}{\partial \theta_0}, \frac{\partial J}{\partial \theta_1} \right)
   \]
   这个梯度向量指向代价函数增长最快的方向。在梯度下降法中，我们沿着梯度的**相反方向**更新参数，以最小化代价函数。

### 梯度与偏导数的关系：
- **梯度是偏导数组成的向量**：梯度中的每个分量都是代价函数相对于某个参数的偏导数。
- **梯度的大小与方向**：梯度的大小告诉我们在某个参数方向上，代价函数变化的速度；梯度的方向告诉我们应该如何调整参数，以减少代价函数。

### 梯度下降法中的梯度更新：
在梯度下降法中，我们利用梯度（即代价函数的偏导数）来逐步更新模型的参数，使得代价函数逐渐减小。

例如，更新参数的公式：
\[
\theta_j := \theta_j - \alpha \frac{\partial J}{\partial \theta_j}
\]
其中：
- `θ_j` 是模型的第 `j` 个参数。
- `α` 是学习率，决定了每次参数更新的步长。
- \(\frac{\partial J}{\partial \theta_j}\) 是代价函数对 `θ_j` 的偏导数（也就是梯度的一个分量）。

### 总结：
- **梯度**是代价函数对每个参数的偏导数组成的向量，它告诉我们参数应该如何调整来减小代价函数。
- **偏导数**表示代价函数对某个参数的变化率，梯度是所有偏导数组成的向量，用于指导参数的更新方向和大小。

## 几种常见的模型评价指标
在机器学习中，**模型评价指标**用于评估模型在特定任务上的表现。根据任务的不同（如分类或回归），使用的评价指标也会有所不同。以下是几种常见的模型评价指标：

### 一、分类问题的评价指标

1. **准确率（Accuracy）**  
   准确率是最常见的分类模型评价指标，表示模型预测正确的样本占总样本的比例：
   \[
   \text{Accuracy} = \frac{\text{预测正确的样本数}}{\text{总样本数}}
   \]
   适合类别均衡的数据集，但在类别不均衡时可能表现较差。

2. **精确率（Precision）**  
   精确率是指模型预测为正类的样本中，实际为正类的比例：
   \[
   \text{Precision} = \frac{TP}{TP + FP}
   \]
   - TP（True Positive）：预测为正类，且实际为正类。
   - FP（False Positive）：预测为正类，但实际为负类。

   精确率衡量模型的**准确性**，避免错误将负类预测为正类。

3. **召回率（Recall）**  
   召回率是指实际为正类的样本中，被正确预测为正类的比例：
   \[
   \text{Recall} = \frac{TP}{TP + FN}
   \]
   - FN（False Negative）：实际为正类，但被预测为负类。

   召回率衡量模型的**覆盖率**，用于关注正类样本的完整预测。

4. **F1 值（F1 Score）**  
   F1 值是精确率和召回率的调和平均数，综合考虑了这两者之间的平衡：
   \[
   F1 = 2 \times \frac{\text{Precision} \times \text{Recall}}{\text{Precision} + \text{Recall}}
   \]
   F1 值在精确率和召回率之间取得平衡，适用于类别不均衡的数据。

5. **ROC 曲线（Receiver Operating Characteristic Curve）**  
   ROC 曲线展示了分类模型的**真阳率**（Recall）和**假阳率**（False Positive Rate, FPR）之间的权衡。真阳率表示正确预测正类的比例，假阳率表示错误预测正类的负类比例。ROC 曲线的**AUC（Area Under Curve）**值衡量模型的整体性能，AUC 值越接近 1，模型性能越好。

6. **混淆矩阵（Confusion Matrix）**  
   混淆矩阵是一个 `2x2` 的表格，展示了模型的分类结果，包含：
   - True Positive (TP)：预测为正类，且实际为正类。
   - True Negative (TN)：预测为负类，且实际为负类。
   - False Positive (FP)：预测为正类，但实际为负类。
   - False Negative (FN)：预测为负类，但实际为正类。

   混淆矩阵可以帮助可视化模型的分类表现，特别是了解不同类型的错误分类。

### 二、回归问题的评价指标

1. **均方误差（Mean Squared Error, MSE）**  
   MSE 是回归任务中最常用的评价指标，表示预测值与真实值之间差值的平方和的平均数：
   \[
   \text{MSE} = \frac{1}{n} \sum_{i=1}^{n} (\hat{y}_i - y_i)^2
   \]
   其中，`n` 是样本数，`y_i` 是第 `i` 个真实值，`\hat{y}_i` 是第 `i` 个预测值。MSE 衡量了模型的总体误差，误差较大的预测会被放大。

2. **均方根误差（Root Mean Squared Error, RMSE）**  
   RMSE 是 MSE 的平方根，能够提供一个更直观的误差衡量值：
   \[
   \text{RMSE} = \sqrt{\text{MSE}}
   \]
   RMSE 保留了 MSE 的优点，同时与真实误差量级更为接近。

3. **平均绝对误差（Mean Absolute Error, MAE）**  
   MAE 衡量预测值与真实值之间的**绝对误差**的平均值：
   \[
   \text{MAE} = \frac{1}{n} \sum_{i=1}^{n} |\hat{y}_i - y_i|
   \]
   与 MSE 不同的是，MAE 对异常值的敏感性较小。

4. **决定系数（R-squared, \(R^2\)）**  
   \(R^2\) 衡量模型对数据的拟合程度，表示模型解释了多大比例的方差：
   \[
   R^2 = 1 - \frac{\sum_{i=1}^{n} (\hat{y}_i - y_i)^2}{\sum_{i=1}^{n} (y_i - \bar{y})^2}
   \]
   其中，`y_i` 是真实值，`\hat{y}_i` 是预测值，`\bar{y}` 是真实值的均值。\(R^2\) 的值在 0 到 1 之间，值越接近 1，模型的拟合效果越好。

评价标准越小越精确
### 三、选择评价指标的注意事项
- **数据不均衡时**，准确率可能无法充分反映模型性能，此时应考虑使用精确率、召回率或 F1 值。
- **多类别分类问题**，可以扩展二分类指标，如使用微平均和宏平均来计算 F1 值。
- **回归问题**时，MSE 和 MAE 都是常用的，但 MSE 对异常值更敏感，MAE 则更适合衡量整体误差水平。

通过选择合适的模型评价指标，可以全面评估模型在不同任务下的性能并进行模型优化



训练集的损失持续减少，而验证集的损失开始上升

## 过拟合欠拟合
**过拟合**和**欠拟合**是机器学习模型中常见的两种问题，它们分别反映了模型在学习过程中的两种极端表现。

### 1. **过拟合（Overfitting）**

**过拟合**是指模型在**训练集上表现得很好**，但在**测试集或新数据上表现较差**。它意味着模型过于复杂，以至于它不仅学到了训练数据中的模式，还学到了训练数据中的**噪声和异常**，从而在遇到新数据时表现不佳。

#### 特征：
- 模型复杂度过高，学习了数据中的细微波动和噪声。
- 在训练集上准确率很高，但在测试集或验证集上表现较差，出现较大的误差。
- 通常发生在数据量较少或特征过多的情况下。

#### 如何检测过拟合：
- 训练集的损失持续减少，而验证集的损失开始上升。
- 训练集的准确率远高于验证集的准确率。

#### 解决方法：
- **正则化**：通过 L1 或 L2 正则化增加模型的约束，使得模型参数不会过大，防止过拟合。
- **增加数据量**：更多的训练数据可以帮助模型学到更泛化的模式，避免过拟合。
- **交叉验证**：使用交叉验证方法评估模型的泛化能力。
- **简化模型**：减少模型的复杂度，例如减少神经网络的层数或神经元的数量。
- **提前停止（Early Stopping）**：在训练过程中监控验证集的性能，当验证集性能不再提升时，停止训练。

---

### 2. **欠拟合（Underfitting）**

**欠拟合**是指模型在**训练集和测试集**上都表现不好，意味着模型过于简单，无法学到数据中的模式。它没有充分捕捉到数据的特征，导致预测性能不佳。

#### 特征：
- 模型复杂度太低，无法捕捉数据中的重要模式。
- 在训练集和测试集上都表现差，误差较大。
- 训练集的表现和测试集的表现相差不大，但都不好。

#### 如何检测欠拟合：
- 训练集和测试集的误差都较高，且误差相差不大。
- 模型训练结束后，模型仍然无法很好地拟合训练数据。

#### 解决方法：
- **增加模型复杂度**：增加更多的特征，或者使用更复杂的模型（如更深的神经网络、更多的树或更复杂的算法）。
- **增加训练时间**：如果模型还没有充分训练，可以通过增加训练迭代次数来提升表现。
- **特征工程**：通过创建更多有意义的特征，帮助模型更好地学习数据的模式。
- **选择合适的算法**：有时可以通过选择更强的算法来解决欠拟合问题，如从线性回归换成多项式回归等。

---

### 3. **过拟合和欠拟合的可视化**
过拟合和欠拟合常常通过**学习曲线**来观察，学习曲线展示了训练集和验证集的损失或准确率随时间变化的情况：

- **欠拟合**：训练集和验证集的误差都很高，模型不能很好地拟合数据。
- **适当拟合**：训练集和验证集的误差都较低，并且相差不大，说明模型具有良好的泛化能力。
- **过拟合**：训练集的误差很低，而验证集的误差很高，说明模型在训练集上表现良好，但不能泛化到新数据上。

---

### 总结：
- **过拟合**：模型太复杂，导致训练集表现很好，但泛化能力差。
- **欠拟合**：模型太简单，无法捕捉数据中的模式，训练集和测试集表现都不好。
- **理想模型**：模型在训练集和测试集上都能表现良好，说明模型具有较强的泛化能力。

正则项，二范式


## Cost function
![[Pasted image 20241120102954.png]]

![[Pasted image 20241120104146.png]]

![[Pasted image 20241120104947.png]]When the cost is relatively small, closer to zero, it means the model fits the data better compared to other choices for w and b.

![[Pasted image 20241120110530.png]]

智能本质上就是针对不同情境给出针对性的输出反应

模型结构哪来这种神奇黑箱？
损失函数怎么奖励一个机器？
训练过程机器怎么建立条件反射

感知机
多层感知机
[00:30:03](ziyunote://play?path=https%3A%2F%2Fwww.bilibili.com%2Fvideo%2FBV1atCRYsE7x%2F%3Fspm_id_from%3D333.1007.top_right_bar_window_default_collection.content.click%26vd_source%3D8b450300cfa6415cb0312754cf65ba30&time=00:30:03)
[00:40:05](ziyunote://play?path=https%3A%2F%2Fwww.bilibili.com%2Fvideo%2FBV1atCRYsE7x%2F%3Fspm_id_from%3D333.1007.top_right_bar_window_default_collection.content.click%26vd_source%3D8b450300cfa6415cb0312754cf65ba30&time=00:40:05)


我们用两个垂直于坐标轴的截面和曲面相交,截面会切出一根曲线来,
然后我们再求这根曲线的导数将这两个导数拼在一起,再求曲线的导数，得到梯度

[00:48:30](ziyunote://play?path=https%3A%2F%2Fwww.bilibili.com%2Fvideo%2FBV1atCRYsE7x%2F%3Fspm_id_from%3D333.1007.top_right_bar_window_default_collection.content.click%26vd_source%3D8b450300cfa6415cb0312754cf65ba30&time=00:48:30)
泛化



反向传播
最小二乘法
```python
# 根据最小二乘法公式求解 w1 和 w0
w1 = (np.sum((x - x_mean) * (y - y_mean))) / (np.sum((x - x_mean) ** 2))  
w0 = y_mean - w1 * x_mean
```
梯度下降法
```python
# 计算预测值 
y_pred = w1 * x + w0 
# 计算损失函数的梯度 
dw1 = (1 / n) * np.sum((y_pred - y) * x) 
dw0 = (1 / n) * np.sum(y_pred - y) 
# 更新参数 
w1 = w1 - alpha * dw1 
w0 = w0 - alpha * dw0
```
学习率

逻辑回归
![[Pasted image 20241124174605.png]]

## **逻辑回归的完整工作流程**

1. **输入特征**： 输入特征 x\mathbf{x}x 是一个 nnn-维向量（或矩阵）。这些特征是我们用来预测输出类别的数据。
    
2. **线性组合**： 使用权重 www 和偏置 bbb 对输入特征进行线性组合：
    
    z=w⋅x+bz = \mathbf{w} \cdot \mathbf{x} + bz=w⋅x+b
3. **非线性变换**： 将线性组合的结果 zzz 输入到 sigmoid 函数中，计算输出概率：
    
    y^=σ(z)=11+e−z\hat{y} = \sigma(z) = \frac{1}{1 + e^{-z}}y^​=σ(z)=1+e−z1​
4. **输出类别**： 根据概率 y^\hat{y}y^​，通过设置阈值（如 0.5）将结果映射到类别 0 或 1。

```python
import numpy as np

# 数据
X = np.array([1, 2, 3, 4, 5]).reshape(-1, 1)  # 输入特征
y = np.array([0, 0, 0, 1, 1])  # 标签
n = len(y)

# 初始化参数
w = 0.0  # 权重
b = 0.0  # 偏置
alpha = 0.1  # 学习率

# Sigmoid函数
def sigmoid(z):
    return 1 / (1 + np.exp(-z))

# 梯度下降迭代
for epoch in range(1000):  # 迭代 1000 次
    # 预测
    z = np.dot(X, w) + b
    predictions = sigmoid(z)
    
    # 计算梯度
    dw = (1 / n) * np.dot(X.T, (predictions - y))  # 对 w 的梯度
    db = (1 / n) * np.sum(predictions - y)        # 对 b 的梯度
    
    # 参数更新
    w -= alpha * dw
    b -= alpha * db

    # 计算损失（交叉熵）
    loss = -np.mean(y * np.log(predictions) + (1 - y) * np.log(1 - predictions))
    if epoch % 100 == 0:
        print(f"Epoch {epoch}, Loss: {loss:.4f}")

print(f"最终权重: {w:.4f}, 最终偏置: {b:.4f}")

```
向量化
KNN
![[Pasted image 20241120115419.png]]
![[Pasted image 20241120115950.png]]
支持向量机
决策树

矩阵相乘相加

神经网络
卷积神经网络
池化

OpenCv
计算机视觉开发
自然语言处理