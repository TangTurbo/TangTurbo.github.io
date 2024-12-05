# Simulink与大语言模型（LLMs）结合的调研报告

## 引言

### 调研背景

Simulink是MATLAB提供的强大工具，广泛应用于动态系统的建模、仿真与设计。其模块化建模方式和多领域支持使其成为工业界和学术界不可或缺的系统设计平台。

与此同时，大语言模型（LLM, Large Language Models），如OpenAI的GPT系列和Google的BERT等，以其自然语言处理的卓越能力正在逐步影响各行业。其在语言生成、知识推理、代码生成等领域的表现，展示了跨领域协作的无限可能。

Simulink与大语言模型结合，有望通过自然语言驱动的建模、仿真与优化，进一步提升系统设计的效率和创新能力。这种结合不仅能为工程设计带来变革，也能为智能系统开发提供新的技术框架。

### 调研目的

本次调研旨在探讨Simulink与大语言模型结合的可能性，分析技术结合点、典型应用场景与技术挑战，并展望这一新兴领域的未来发展。

## Simulink概述

Simulink是由MathWorks公司开发的多领域动态系统建模和仿真工具，广泛应用于控制系统、信号处理、通信系统和嵌入式系统开发中。其主要特点包括：

- 图形化建模：基于模块的拖拽式操作，简化了复杂系统的建模过程。
- 多领域支持：支持物理建模、信号流建模和多领域交叉建模。
- 实时仿真：支持与硬件连接进行实时仿真。
- 自动化代码生成：支持将模型转换为C/C++或HDL代码，用于嵌入式系统。

### Simulink 的典型应用：

1. 汽车工业：自动驾驶与动力系统仿真。
2. 航空航天：飞行器控制和任务规划。
3. 能源行业：智能电网仿真和新能源系统优化。
![image](https://github.com/user-attachments/assets/111e680b-bc0d-4384-9be6-33bc7d616b55)

尽管Simulink强大且灵活，但在模型开发过程中仍存在挑战，如模型构建的复杂性、模块复用的困难、错误调试成本高等问题。

## 大语言模型（LLMs）概述

### 语言模型历史
![image](https://github.com/user-attachments/assets/6b0476a3-8a42-44a6-a98d-1e961ef7646a)

大语言模型（如OpenAI的GPT系列、Google的Bard）是基于深度学习的自然语言处理系统，其核心技术包括Transformer架构和大规模预训练。这些模型通过学习海量数据中的语言模式，具备以下特点：

- 强大的语言生成与理解能力：能够进行上下文对话、内容创作、问题回答等。
- 跨领域适应性：能应用于翻译、编程、文本分析等不同任务。
- 增强的推理能力：在特定情况下能够进行逻辑推导和任务规划。

### 应用现状：

- 代码生成：如 GitHub Copilot，辅助开发者完成代码编写。
- 自然语言接口：通过简单指令生成复杂的程序或脚本。
- 跨领域辅助：如药物研发、天文学数据分析等。

## Simulink与大语言模型结合的潜力与应用场景

### 自动化建模与设计支持

大语言模型可以将自然语言描述转化为Simulink模型框架。例如，用户提供“构建一个PID控制器”，模型能够自动生成对应的模块。

在复杂系统建模中，帮助设计初始框架，从而降低手工建模的工作量。

### 相关论文

#### Requirements-driven Slicing of Simulink Models Using LLMs

链接：[2405.01695](https://arxiv.org/abs/2405.01695) (arxiv.org)

##### 一、研究背景

传统的模型切片技术依赖于手动创建的追溯链接，这些链接可能会耗时且容易出错。大型语言模型的出现提供了一种可能，即不依赖手动追溯链接就能自动化切片过程。

##### 二、基础知识

链式思考：鼓励模型不仅生成最终的答案，而且逐步展示出它是如何推理并得出结论的。在执行复杂问题求解时，模型会生成一系列中间步骤，每个步骤都可以视为解答问题的一个逻辑片段或计算过程的一部分。

零样本学习：一种能够在没有任何样本的情况下学习新类别的方法。通常情况下，模型只能识别它在训练集中见过的类别。但通过零样本学习，模型能够利用一些辅助信息来进行推理，并推广到从未见过的类别上。这些辅助信息可以是关于类别的语义描述、属性或其他先验知识。
![image](https://github.com/user-attachments/assets/baae2c06-0d58-47f6-8921-daaee2031bce)

##### 三、研究目的

作者提出一种基于大型语言模型的方法，用于从图形化的Simulink模型中提取模型切片，这样可以实现自动化地从Simulink模型中提取与特定需求相关的子集，以便于在认证过程中进行设计审查。

##### 四、实验过程

1. 将Simulink模型转换为文本表示。
2. 使用LLM识别满足特定需求的Simulink块。
3. 通过系统地过滤LLM认为非必需的块来生成模型切片。
5. 通过执行生成的切片并比较它们与原始模型满足需求的能力来验证切片的准确性。
6. 实验涉及不同的文本表示粒度和LLM提示策略，以评估它们对生成切片准确性的影响。

##### 五、实验结果分析及结论

![image](https://github.com/user-attachments/assets/6257f68d-701d-499c-95f6-c9cd2499b9d3)

“CT”、“NS”和“ZS”分别对应思维链策略、N-shot 策略和Z-shot策略。

每列对应Tustin模型的要求R，即 R1-R5。

✓表示切片和原始模型的适配性极性相同；

✗表示切片的适配性极性与原始模型不同；

V表示切片完全满足要求。
![image](https://github.com/user-attachments/assets/bbee9360-71a0-4ac8-9570-cb51dc06aa5f)

-表示不准确的模型切片

由表3可得，低冗余度的配置生成的无效切片最多。这表明高度抽象化（即低冗余度）的模型缺乏必要的细节，导致LLM无法很好地识别需求。同时，高冗余度配置在模型地文字描述中加入了视觉渲染细节，这可能会导致LLM无法处理所有细节以精准定位相关区块，从而产生错误。

初步发现表明，保留Simulink块的语法和语义信息，同时省略视觉渲染信息的文本表示，能够产生最准确的切片。结合链式思考和零样本提示策略，中等粒度的转换能够产生最准确的模型切片（即M-CT与M-ZS）。

##### 六、个人思考

该研究仅使用了两个Simulink模型进行实验，可以考虑更多种类的Simulink模型来扩大评估范围，并与基线方法进行比较。

论文中仅使用单一的LLM进行实验，个人认为不同LLM的选择可能会影响模型切片的质量。可以在接下来的实验中研究探索不同LLMs对模型切片质量的影响，以及如何进一步提高模型切片的准确性和效率。

### 优化与自动调试

利用LLMs分析Simulink模型，建议改进信号传递、优化参数设置，甚至自动修复模型错误。

### 相关论文

#### SLGPT: Using Transfer Learning to Directly Generate Simulink Model Files and Find Bugs in the Simulink Toolchain

链接：[SLGPT](https://arxiv.org/abs/SLGPT) (arxiv.org)

##### 一、研究背景

由于Simulink代码库庞大且缺乏完整的形式化语言规范，发现错误非常困难。深度学习技术可以从样本模型中学习语言规范，但需要大量训练数据才能有效工作。

##### 二、基础知识

迁移学习

目标：将某个领域或任务上学习到的知识或模式应用到不同但相关的领域或问题中。

主要思想：从相关领域中迁移标注数据或者知识结构、完成或改进目标领域或任务的学习效果。

关键点：

![image](https://github.com/user-attachments/assets/50e1f9cc-04f6-45ab-a18a-0bf2c16c8873)

##### 三、方法提出与问题解决

作者提出SLGPT（使用迁移学习直接生成Simulink模型文件并发现Simulink工具链中的错误），利用预训练的GPT-2模型，通过迁移学习适应Simulink，以生成更高质量的Simulink模型并发现更多的工具链错误。

##### 四、技术方法

1. 数据收集：通过随机模型生成器SLforge和开源代码库（GitHub和MATLAB Central）获取Simulink模型。
![image](https://github.com/user-attachments/assets/ae2d88a1-7cf5-498d-8ec3-73fffc051037)

3. 数据预处理：简化模型以去除无法处理的特征，并重构模型以适应GPT-2的学习风格。
4. 模型训练：使用预训练的GPT-2模型，并用随机生成的模型和开源模型进行微调。
5. 模型生成：从调整过的GPT-2模型中迭代采样生成Simulink模型文件。
6. 模型验证：使用有效性检查器检测Simulink工具的崩溃，并手动审查每个崩溃案例。

##### 五、实验评估

![image](https://github.com/user-attachments/assets/1d948359-95cd-47a3-b538-1705e3c6324a)

与最接近的竞争对手相比，SLGPT能够生成有效且更接近开源模型的Simulink模型，并且发现了比DeepFuzzSL更多的Simulink开发工具链错误。同时，其实现、参数设置和训练集都是开源的。

##### 六、个人观点

尽管SLGPT通过迁移学习解决了训练数据不足的问题，但仍然需要更大、更多样化的数据集以进一步提高模型的泛化能力。同时，因为Simulink模型较为复杂，且没有公开可用的完整规范，所以仍需要进一步研究以减少模型生成过程中出现的错误。

### 测试与验证自动化

#### 测试用例生成与智能验证

根据模型功能和设计目标，LLM自动生成全面的测试用例，确保模型在各种极端条件下的可靠性。分析测试结果，诊断失败原因并提出改进方案，提升系统的安全性和稳定性。

#### 相关论文1

链接：[An Empirical Study of Using Large Language Models for Unit Test Generation](https://arxiv.org/abs/2305.00418) (arxiv.org)

##### 一、实验方法

该研究以三种生成模型（StarCoder、Codex和GPT-3.5-Turbo）为对象，分析它们在单元测试生成任务中的性能。具体而言，研究采用了两个基准测试，分别是HumanEval和Evosuite SF110（Java版本），并通过对编译率、测试正确性以及覆盖率等方面进行评估，深入探讨了这些LLM在生成测试用例方面的有效性。

##### 二、基础知识

HumanEval是由OpenAI编写发布的代码生成评测数据集，包含164道人工编写的Python编程问题，模型针对每个单元测试问题生成k（k=1,10,100）个代码样本，如果有任何样本通过单元测试，则认为问题已解决，并报告问题解决的总比例，即 Pass@k得分。
![image](https://github.com/user-attachments/assets/08988aba-1035-4095-9795-627517a5eaec)

##### 三、实验结果分析

###### 编译率分析：

![image](https://github.com/user-attachments/assets/0dc62894-2da8-4e66-9f35-db64c3a9c721)
在HumanEval数据集上表现最好的是StarCoder，高达70.0%，而其他只有不到一半是可编译的；在SF110数据集上，编译率都不高，最高的StarCoder仅有12.7%；启发式修复后，编译率平均提高了41%，Codex(2K)增幅最大，而启发式方法对StarCoder的作用不大，只提高了6.9%的编译率。单看HumanEval数据集，常出现的编译错误原因是编译器找不到符号、从数据格式转换不兼容等原因；而在SF110数据集方面，常出现的编译错误是编译器找不到符号，或者class是抽象类而无法实例化造成的。

###### 测试通过分析：

![image](https://github.com/user-attachments/assets/4547fc6a-a136-4d95-b023-4ff1d3857d81)

测试正确性方面有两个指标，Correct指的是所有测试方法均能通过，Somewhat Correct指的是至少有通过一个测试方法的测试。

在HumanEval数据集方面，StarCoder表现最好，测试通过率为81.3%，但是ChatGPT的Somewhat Correct的指标高达92.3%。同时增加Codex的Tokens数不能产生更高的正确性。

在SF110数据集方面：表现最好的同样是StarCoder，有51.9%的测试通过率。在Somewhat Correct指标上，Codex(2K)性能最好。与HumanEval相比，所有LLM的两个测试通过率都比较低。

###### 测试覆盖率：

![image](https://github.com/user-attachments/assets/b216d96d-c09e-467e-a11e-d0259dd86cd6)

在HumanEval数据集方面，LLM的行覆盖率在67%-87.7%，分支覆盖率在69.3-92.8%，但是低于手动测试和Evosuit生成的覆盖率。在SF110数据集方面，比HumanEval的覆盖率要差很多，均不到2%，Evosuite覆盖率分别是27.5%和20.2%。

###### 总结

对编译率、测试正确性和覆盖率进行比较后，发现尽管LLM无法产生正确的测试，但能够生成一些对输入/输出有用的测试用例。在HumanEval数据集方面，LLM的能力与当前先进的测试生成工具有比较接近的性能。

#### 相关论文2

链接：[Reinforcement Learning from Automatic Feedback for High-Quality Unit Test Generation](https://arxiv.org/abs/2310.02368) (arxiv.org)

##### 一、实验方法

文章提出的方法是静态质量指标强化学习(RLSQM)，通过使用静态质量指标分析对程序进行评分，将其输入到LLM中，接着训练奖励模型(RM)根据静态质量指标对测试用例进行评分，并使用评分为近端策略优化(PPO)算法提供反馈。来增强LLM能够生成更高质量的测试用例。

这里提到的静态质量指标包括：

1. 语法是否正确：根据代码语言的规范，生成的测试用例在语法上应有效；
2. 是否有断言；
3. 是否有焦点方法的调用；
4. 输出的测试用例是否有多余字符；
5. 是否生成注释/评论；
7. 是否重复断言；
8. 是否包含try/catch块、控制语句（if、switch、while）或异常处理；
9. 是否包含空测试(例如public void TestMethod(){ })。

##### 二、实验结果及分析

![image](https://github.com/user-attachments/assets/26121799-e52b-4054-80e5-559c1f80c3eb)

表中使用个人奖励对RL进行**微调后**测试集的质量指标。“√”表示应该鼓励的积极特征；“×”表示应该劝阻的负面特征。

与基础模型相比，RL优化模型，使模型提高了21%，并生成了将近100%的语法正确的代码。并且7个指标中的四个指标上优于GPT-4。

强化学习能够将所有质量指标提高到SFT模型之上，这表明RLSQM微调能够增强单个质量指标。

#### 总结

上述两篇文章主要介绍了LLM生成测试用例的有效性验证以及微调方法。在学术论文搜索引擎上检索LLM测试用例生成时，从论文发表的时间和数量能明显感觉到LLM在单元测试领域还处于起步阶段。但不可否认的是，它已经表现出了巨大的潜力。

当前评价测试用例的好坏主要是编译率、测试通过率、覆盖率等静态质量指标。评价的测试集有两种，一组是HumanEval，另一组是开源库。前者的指标明显高于后者。当前SFT是主流的微调方法，但强化学习也开始融入微调的过程中，能够有效提高模型的性能。不过每一种语言对于测试用例都有各自的测试框架，在目前的大部分研究中，主要是以单语言来进行训练，并未考虑多语言的情况，所以在未来的研究中，可以对这方面进行更深入的探索。

### 跨平台集成与扩展

#### 代码接口生成

生成Simulink模型与外部工具（如Python、数据库）的通信接口代码。

#### 多工具协作

实现Simulink与其他工具的无缝整合（如AutoCAD的机械设计模块）。

### 当前研究进展与案例

#### LLM驱动的代码生成与解释

已有研究表明，LLMs可将自然语言翻译为MATLAB代码，而MATLAB代码可进一步用于Simulink建模。例如，OpenAI的Codex模型被用于将控制系统的文字描述转化为MATLAB脚本，再通过脚本自动生成Simulink模型。

#### 自然语言接口的开发

一些研究团队正在探索开发自然语言与Simulink的接口。例如，通过API集成，用户可以用语音或文字直接控制Simulink模型的创建与仿真。

#### 相关论文

链接：[Evaluating Large Language Models Trained on Code](https://arxiv.org/abs/2107.03374) (arxiv.org)

论文详细描述了Codex模型的能力，其中包括将自然语言描述转换为代码的能力，虽然论文中没有直接提及Simulink，但是Codex生成的MATLAB代码可以进一步用于Simulink建模。

### 技术挑战

#### 模型和工具之间的接口设计

**数据格式不兼容**：Simulink模型主要以模块化、图形化表示，而LLMs处理的是文本和语言。将 Simulink的模块、参数和信号流映射为LLMs可理解的表示存在技术障碍。

**通信机制复杂**：需要设计一个高效、可靠的通信接口，使Simulink和LLMs能在多轮交互中保持一致性。

#### 数据和上下文的表示与传递

**数据规模限制**：Simulink 模型可能包含大量模块和参数，直接传递给LLMs处理会超出其上下文窗口限制。

**复杂系统的分层结构**：Simulink模型常包含嵌套的子系统或多层次的模块化设计，LLMs在解析多层次结构时可能出现误解。

#### LLM的知识局限性

LLMs的训练数据可能不包含特定行业的复杂控制算法或仿真知识，无法直接理解物理过程的动态行为，例如机械振动、电力系统瞬态等，难以生成精准的模型或建议。

#### 实时性与性能

**推理速度瓶颈**：LLMs在处理复杂指令或大型模型时，推理可能耗时较长，无法满足Simulink实时仿真的要求。

**高算力需求**：集成LLMs需要额外的计算资源，而在嵌入式系统或资源受限的设备上可能无法运行。

**模型大小限制**：使用大规模LLMs（如GPT-4）时，设备内存和性能可能成为瓶颈。

#### 模型验证与可靠性

**生成结果的可信度**：LLMs可能生成不可靠的模型组件或参数，导致仿真错误甚至灾难性后果。

**自动化测试难度**：对LLMs的输出模型进行验证需要额外的工具支持，而现有的验证工具无法完全覆盖所有自动生成内容。

#### 数据隐私与安全

**敏感数据泄露**：将工业或工程模型信息传递给LLMs可能带来数据泄露风险，尤其是在使用云端LLMs时。

**合规性问题**：某些行业对数据和模型的隐私性有严格要求（如医疗、国防）。

### 未来发展方向

**实时决策支持**：LLM在运行仿真时动态分析结果，提供调整建议，例如交通信号系统的实时优化。

**深度集成工具链**：通过插件或扩展模块支持自然语言输入与实时反馈。

**多模态交互的工程工作环境**：用户可通过语音或文本对LLMs提出问题或需求，实时生成或修改Simulink模型，同时在图形化界面上呈现结果，实现自然语言与图形化结合。

**教育和普及化**：降低Simulink使用门槛，帮助非技术背景用户轻松设计复杂系统。

## 结论

Simulink和大语言模型结合可以带来巨大的技术价值和应用前景，这种结合不仅可以提高建模效率、优化仿真质量，还可以推动自动化设计流程的发展。然而，目前仍需进一步研究以克服数据接口、模型可靠性和实时性等技术挑战。未来，随着工具链和技术的成熟，这一结合有望成为工程仿真领域的关键创新方向。

## 参考文献

1. Luitel D, Nejati S, Sabetzadeh M. Requirements-driven Slicing of Simulink Models Using LLMs[J]. arXiv preprint arXiv:2405.01695,2024.
2. Shrestha S L, Csallner C. SLGPT: Using transfer learning to directly generate Simulink model files and find bugs in the Simulink toolchain[C]//Proceedings of the 25th International Conference on Evaluation and Assessment in Software Engineering. 2021: 260-265.
3. Siddiqa M L, Santos J C S, Tanvirb R H, et al. An Empirical Study of Using Large Language Models for Unit Test Generation[J]. arXiv preprint arXiv:2305.00418, 2023.
4. Steenhoek B, Tufano M, Sundaresan N, et al. Reinforcement learning from automatic feedback for high-quality unit test generation[J]. arXiv preprint arXiv:2310.02368, 2023.
5. Chen M, Tworek J, Jun H, et al. Evaluating large language models trained on code[J]. arXiv preprint arXiv:2107.03374, 2021.
