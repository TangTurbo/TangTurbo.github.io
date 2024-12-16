# Slicing MATLAB Simulink Models

论文链接：https://ieeexplore.ieee.org/abstract/document/6227161

## 研究背景

MATLAB Simulink是汽车行业中开发复杂嵌入式系统最广泛使用的工业工具。这些系统通常包含数万个块和多个层次，常被部署在安全关键领域，需要全面的质量保证措施。手动审查复杂的Simulink模型由于其复杂的结构层次和模型引用，难以识别数据和控制流。

## 研究目的

本文旨在通过开发Simulink的静态切片技术来减少模型的复杂性。该方法移除不影响特定块的模型部分，使模型切片保持语义和结构层次，便于后续的自动化质量保证措施和手动审查。

## 研究问题

1. 如何进行MATLAB Simulink模型的控制和数据依赖分析。
2. 如何通过依赖图的方法对Simulink模型进行切片，以保留模型的层次和语义。

## 问题1 数据依赖&控制依赖

![image](https://github.com/user-attachments/assets/0924977f-5a0e-4b85-9dbc-e3e849d4bc01)

### 定义1：数据依赖 (Data Dependence)

条件：节点j数据依赖于节点i，如果满足以下三点：

- X在节点i被定义（即赋值）；
- x在节点j被引用（即使用）；
- 从i到j存在一条路径，在这条路径上x没有被重新定义（覆盖）。

![image](https://github.com/user-attachments/assets/2ddbdda5-6ef9-44dc-a15b-5a71d8a40880)

### 定义2：控制依赖 (Control Dependence)

条件：节点j控制依赖于节点i，如果满足以下两点：

- 存在一条路径从i到j，在这条路径上，j后支配 (postdominates) 路径上的所有节点（不包括i）；
- i不能被j后支配。

后支配 (Postdominate)：如果从i到程序出口的所有路径都必须经过j，则称节点j后支配节点i。

示例：

![image](https://github.com/user-attachments/assets/69053a14-772a-451d-bdf6-f42ae68536ca)

![image](https://github.com/user-attachments/assets/34423c07-b2ea-4c98-82a8-d17fa4f4f249)

![image](https://github.com/user-attachments/assets/26210df1-2414-4212-bfab-ea3c4ff81f61)

### Simulink模型中的依赖关系

#### 数据依赖：

定义：如果块B2的输入通过线路L从块B1的输出接收信号，则B2对B1有数据依赖。

在Simulink中，数据依赖通过**信号线**来表示，即如果一个块的输出是另一个块的输入，则第二个块对第一个块有数据依赖。

#### 控制依赖：

定义：如果块B2在一个条件执行上下文中，且这个上下文由块B1的条件控制，则B2对B1具有控制依赖。

控制依赖在Simulink中通过条件执行上下文（Conditional Execution Contexts, CECs）来确定，这些上下文代表了模型中的执行路径。

### 通过CEC分析依赖（控制依赖）

#### 条件执行上下文（CEC）：

条件子系统和循环只有在特殊端口或迭代块给出的条件评估为 “真”时才会执行。因此，相应的EC被称为条件执行上下文（CEC）。

![image](https://github.com/user-attachments/assets/00c1bc61-e423-4bed-bff0-d260df4a6ce5)

在对模型m的块进行调度时，Simulink会创建两个执行上下文：

- 模型的根上下文和条件子系统 S 的执行上下文，即CEC_S。

  调度流程：

  ![image](https://github.com/user-attachments/assets/bae1288d-70d1-4a2b-bd5e-aa6b1e4d80cc)

  对应的控制流程图：

  ![image](https://github.com/user-attachments/assets/e52a46c6-9a87-4257-afd1-ebfa0181b1cd)

从控制流图中可以看出，子系统CEC_S的执行依赖于cond，只有当cond为真时，子系统内的块才会被调度。所以，子系统与根上下文之间的这种依赖关系，直接体现了控制依赖。

关于cond的归属问题：即使cond节点本身包含在子系统m中，cond也必须被执行才能确定子系统是否必须被执行。因此cond必须被分配到父执行上下文中。

## 问题2 Simulink模型的切片方法

作者介绍了一种利用**依赖图**对Simulink模型进行切分的方法：

### 切片准则

![image](https://github.com/user-attachments/assets/5b20f2be-d3fb-47a1-bf4f-ddf6c7b7bf1f)

切片准则C(B)是一组选定的Simulink块。

虚拟块和非虚拟块都可以被选为切片准则，但**子系统块不能直接作为切片准则**。如果要针对子系统切片，则必须使用该子系统的Inport（输入端口）或Outport（输出端口）块。

如果切片准则为空，则生成的切片也为空。

### 块的相关性

块c与块b相关当且仅当c直接或间接地（通过数据依赖或控制依赖）依赖于b。这一概念包括虚拟块（如信号线连接的虚拟块），同时也允许保留Simulink模型的层次结构。

### Simulink切片

![image](https://github.com/user-attachments/assets/2bdb656b-ffea-4677-ab05-8a00c2178e9c)

后向切片：模型切片仅包含与切片准则C(B)**相关**的所有块。

前向切片：模型切片仅包含切片准则C(B)**所影响**的所有块。

层次结构保留：在生成的切片中，Simulink模型的层次结构（如子系统）会被保留。

![image](https://github.com/user-attachments/assets/7eb52062-b9e0-4698-b723-fb5a657e3b37)

### 计算依赖关系（条件执行上下文的计算）

![image](https://github.com/user-attachments/assets/12809c20-29ec-4f5f-abcd-b965aae90d13)

### Simulink模型切片算法解释

#### 示例  
假设一个 Simulink 模型包含以下组件：  
1. 条件子系统：由一个布尔信号控制。  
2. MultiPortSwitch 块：选择多个输入信号之一。  
3. 输入信号：包含一个Gain 块和一个Bias 块，最终输出通过Outport 块传递。  

#### 具体执行步骤 

##### 1. 模型初始化  
- 重新分配信号：将子系统端口重新映射到 Inport 和 Outport 块。  

##### 2. 遍历根上下文  
- 从模型根开始，调用findExecutionContexts(model, root_context)。  

##### 3. 进入条件子系统  
- 识别到条件子系统（例如 If 子系统）：  
    创建新的CEC（条件执行上下文），例如CEC_1，用于子系统。  
    将控制信号的块（如If条件判断块）添加到父CEC中。  
- 继续遍历子系统内部。  

##### 4. 遍历 MultiPortSwitch 块  
- 当发现 MultiPortSwitch 块时：  
    为每个数据端口（如输入1、输入2）创建一个新的CEC，例如CEC_2和CEC_3。  
    向后传播CEC：将与数据端口连接的块（如 Gain 和 Bias 块）添加到对应的CEC中。  

##### 5. 向外传播上下文  
- 如果PropExecContextOutsideSubsystem参数为ON，算法会：向前、向后检查信号路径，将CEC传递到相关的块。  

#### CEC传播示例解释  

##### MultiPortSwitch 数据端口上下文传播  
- 示例：CEC_2是 MultiPortSwitch 数据输入端口2的上下文。  
    如果该端口连接到Gain块：  
        CEC_2会向后传播到Gain块。  
        Gain块现在属于CEC_2。  

##### Gain块加入CEC的意义  
- 如果条件信号控制MultiPortSwitch的输出：  
    Gain块的输出会成为条件执行上下文的一部分。  
    这表示Gain块的输出仅在特定条件下被执行。  

#### 总结  
- 该算法通过递归遍历Simulink模型结构，识别条件执行上下文（CEC）。  
- 对于MultiPortSwitch或Switch块，CEC会从数据端口向上游传播，使相关的块（如Gain和Bias）加入到同一上下文中。  
- 这种方法确保了Simulink模型中所有条件执行的依赖关系被正确捕获。
  
### 建立依赖关系图并计算模型的静态切片

![image](https://github.com/user-attachments/assets/2dc7f55e-88a9-4de4-9b54-4c314a074451)
![image](https://github.com/user-attachments/assets/0392223a-56eb-4f08-9767-669374363f31)
![image](https://github.com/user-attachments/assets/3e544931-3f65-4b99-be76-a640ebd56cca)

#### 依赖图的生成步骤

##### 1. 创建执行上下文（CEC）
- 根上下文：模型最外层的执行上下文（root）。
- 子上下文：遇到条件执行子系统（如 While Iterator）时，创建新的 CEC。
  e.g. 在 Figure 3 中，While Iterator 形成了一个独立的 CEC。

具体算法实现：
- 遍历 Simulink 模型中的每个块。
- 如果发现条件执行子系统（如 While 块、Switch 块），为其创建新的上下文。

##### 2. 创建依赖图节点
- 遍历所有非子系统块，为每个块创建对应的依赖图节点。
  e.g. 在 Figure 9 中，每个模型块（如Constant, IC, sum+i, write(sum)）都对应一个节点。

##### 3. 添加数据依赖
- 对于每个信号，记录其数据流向，连接对应的节点。

e.g. 在Figure 3中：
 i的值被输入到累加器sum+i和乘法器mul*i，因此i节点有数据流向这些节点。
 sum+i节点的计算结果输出到write(sum)节点。

##### 4. 添加控制依赖
- 如果某个上下文（CEC）由条件执行块（如 While Iterator）触发：
   在依赖图中添加虚线边，表示控制依赖。
   这些数据依赖关系在Figure 9中以灰色实线表示。

e.g. 在Figure 3中：
- While Iterator子系统的所有内部节点都依赖于 Relational Operator 块的判断结果。
- 在Figure 9中，这种控制依赖用虚线边表示。

##### 5. 处理 Switch 和 MultiPortSwitch
- 如果遇到 Switch 或 MultiPortSwitch 块：
   创建额外的虚拟节点，表示选择的条件。
   在虚拟节点与实际数据输入块之间建立控制依赖。

### 小结
- 依赖图通过遍历模型中所有块和信号，生成数据依赖和控制依赖关系。
   数据依赖：表示数据的传递路径，例如i → sum+i → write(sum)。
   控制依赖：表示执行顺序的约束，例如 Relational Operator 控制 While Iterator 子系统内部的计算。
- 虚线和实线共同描述了模型中复杂的依赖关系，帮助理解执行上下文和数据流。

### C(write(mul)) 切片的产生过程

#### 1. 切片标准定义：C(write(mul))
- 切片标准（slicing criterion）：关注特定程序变量或信号的计算过程。
- 在本例中，切片标准是 C(write(mul))，表示我们关心write(mul)信号的计算过程及其所有依赖项。

#### 2. 反向可达性分析（Backward Reachability Analysis）
- 概念：
   从目标节点出发，逆向遍历依赖图，标记所有与目标节点相关的数据依赖和控制依赖节点。
   目的是保留计算目标节点所必需的所有依赖项。

- 应用于图中（Figure 9）：
   目标节点：write(mul)。
   过程：从write(mul)出发，沿着数据依赖和控制依赖路径逆向追踪，标记所有相关节点。

#### 3. 标记的依赖节点
- 关键节点的标记：
  在 Figure 9 中，write(mul) 依赖以下节点：
   mul * i：表示乘法操作。
   i：参与计算mul * i。
   Constant1：提供乘法的初始值。
   While Iterator：控制整个迭代过程的执行。
   Relational Operator 和 read(n)：判断迭代条件，控制While子系统。

- 去除无关节点：
   与 write(mul) 无关的节点不会被标记。
   例如，sum + i及其相关路径（如write(sum)）对write(mul)无影响，因此不会被标记。

#### 4. 移除未标记节点和子系统
- 过程：
   标记完成后，所有未被标记的节点都会被移除。
   如果某个子系统中的所有节点都未被标记，则该子系统也会被移除。

e.g. 在Figure 9中：
   粗体节点和线条表示切片中保留的部分。
   未标记的节点（如 sum, write(sum)）被移除。

#### 5. 切片结果分析
- 切片的最终结果（如 Figure 10 所示）：
   包含所有参与计算write(mul)的必要节点。
   与sum相关的计算（如sum + i, write(sum)）被完全排除，因为它们不影响write(mul)。

### 小结
C(write(mul)) 切片的生成过程如下：
1. 从目标节点write(mul)开始，进行反向可达性分析。
2. 标记所有与write(mul)相关的节点和路径，包括数据依赖和控制依赖。
3. 移除未被标记的节点（例如sum和其相关路径）。
4. 切片结果形成一个依赖子图，只保留计算write(mul)必需的节点。

通过这一过程，原始模型被简化，保留了目标计算的必要部分，去除了无关计算，提升了模型的可读性和分析效率。


## 结论

- 基于依赖图的切片方法在Simulink模型中有效提取数据和控制依赖关系，并能在**块级别**上进行切片。
  块级别：每个块是一个功能单元，执行特定操作（如加法、乘法、条件判断等）。
  
- 该方法相比其他建模符号的切片方法（如 UML、Statecharts），具有良好的性能和准确性，结果表现出较高的前景。
  
- 通过依赖图的构建，实验成功展示了不同 Simulink 结构（如控制依赖块）的处理过程，并能够生成相关的切片图。

## 不足之处与未来展望

- 执行上下文传播不够精确：现有算法对 Simulink 执行上下文的处理不完善，未能完全支持 MATLAB Simulink 文档中的规则 4 和 5（Definition 7）。在未来可以改进执行上下文传播算法，提升执行上下文的传播精度。

- 多速率块处理不足：当前方法未能识别多速率块，这些块包含不同采样率的信号，可能导致切片精度降低。未来可以研究采样率传播机制，确定多速率块（尤其是混合采样率的块）。

- 信号总线的精确性问题：Simulink的线条不能反映所携带的信号数量，运行时才确定总线上携带的具体信号。对总线块的处理尚未深入，数据依赖的精确性有待提高。在未来可以深入研究信号在总线块中的传播机制，精确确定数据依赖关系，提升切片的精确度。

- Stateflow 模块简化处理：当前方法将 Stateflow 模型 视为黑盒，假设所有输入信号与输出信号相关，但这种处理方式可能过于保守，导致切片不够精确。未来可以开发针对 Stateflow 的切片算法，以识别输入与输出之间的相关性，实现更全面的 Simulink/Stateflow 切片技术。

