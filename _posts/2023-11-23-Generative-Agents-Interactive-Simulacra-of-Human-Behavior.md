---
title:  论文笔记 Generative Agents Interactive Simulacra of Human Behavior
date: 2023-11-23 11-22-19
---


作者： 

Stanford     Joon Sung Park、Joseph C.O’Brien、 

Google Research Carrie J.Cai 、Meredith Ringel Morris

Stanford  Percy Liang、Micheal  S.Bernstein

论文地址：https://arxiv.org/abs/2304.03442





![image-20231030212441879](https://raw.githubusercontent.com/gqjia/PictureBed/main/img/202311231124789.png)

生成式代理（Generative agent）使用在交互应用中的有逼真人类行为的实体。在这项研究中作者通过在一个沙盒环境中生成了二十五个 agent 来展示生成式代理。用户可以观察、干预 agent 的日常计划、新闻分享、人际关系的建立以及协调群体活动。



可信的人类行为代理可以赋能交互式应用，比如沉浸式环境、用于人际交流的排练空间、制作原型工具等。本文介绍了一种能够模拟可信人类行为的生成式智能体（Generative Agents）。智能体的完整经历会以自然语言的形式进行记录，并且存储在一个大型语言模型中。随着时间的推移，这些记忆将会形成更高级的反思，并指导智能体规划它们的行为。受到「模拟人生」的启发，本文创造了一个交互式沙盒环境，用户可以在其中与包含 25 个智能体的小镇进行交互。在评估中，这些生成式智能体不仅有可信的个人行为，还涌现出一定的社会行为：比如，最开始用户只是为其中一个智能体指定了举办情人节派对的概念，但是它在接下来两天内自动发出了派对邀请、结识新朋友、相互邀约并协商在合适的时机一起出现在派对上。一个智能体由 observation、planning 和 reflection 组成，消融实验证明了其中每个部分都对智能体的可信行为有至关重要的作用。本工作通过将大型语言模型和计算交互式智能体融合，引入了对人类行为进行可信模拟的架构和交互模式。



如何设计一个表现出可信人类行为的、交互式的人造社会？从类似「模拟人生」的沙盒游戏，到像认知模型（Cognitive Models）和虚拟环境（Virtual Environments）这样的应用，过去四十年的研究人员和从业者不断展望着计算智能体充当可信人类行为的代理。在这些愿景中，计算驱动的智能体根据它们过去的经验行事，并根据环境作出可信的反应。这种对人类行为的模拟有望将真实的社会现象引入虚拟的空间或社区中，这有助于锻炼人们处理罕见但困难的人际关系的能力、测试社会科学理论、为原理和可用性测试设计人类处理器模型、赋能普适计算以及让开放世界游戏中 NPC 驾驭复杂的人际关系。

然而，人类的行为空间巨大且复杂。目前 LLM 取得了极大进步，已经能够在单一时刻（短暂地）模拟可信的人类行为。但是更复杂的情况是：随着时间的推移，新的互动、冲突和事件不断出现和消退，需要管理不断增长的记忆，并同时处理多个智能体之间展开的级联社会动态。应对这样情况的方法需要：在很长的一段时间内**检索相关的事件和互动；**反思**（Reflect）这些记忆并概括得到更高层次的推理；应用这些推论以作出对当下和长期都有意义的**规划和反应**。

本文介绍了生成式智能体——利用生成模型模拟可信的人类行为，并证明了它们能同时对个人和群体行为进行可信的模拟。生成式智能体会对它们自己、其它智能体以及所处环境进行各种推理；它们制定并执行反应它们个性和经验的每日计划，并在适当的时候调整计划；当终端用户改变它们的环境或者用自然语言给它们下令时，它们会针对性地做出反应。比如，生成式智能体会在看到早饭着火的时候关掉炉子、当浴室被占用时在外等候、遇到另一个更想要交流的智能体时停止正在进行的闲聊。一个充满生成式智能体的社会的特点是涌现出社会动态——新的人际关系的形成、信息的扩散、智能体之间的协作。

为了实现这样的生成式智能体，本文描述了一种使用 LLM 进行存储、合成和使用相关记忆以产生可信行为的智能体架构。这个架构包含三个主要部分。第一个部分是**记忆流**（Memory Stream），这是一个用自然语言存储智能体完整经历的长时记忆模块。检索模型结合相关性、时近性（Recency）和重要性来挖掘指导智能体即时行为所需要的记忆记录。第二个部分是**反思**（Reflection），即随着时间推移将记忆合成更高层次的推理。这让智能体能够审视自己和它人，从而更好地指导自己的行为。第三个部分是**规划**（Planning），这一步将反思得到的推论和当前的环境转化成高级的行动规划，然后不断转化成具体的行动和反应。这些反思和规划会再被反馈到记忆流中，从而影响智能体未来的行为。

这个架构有望应用于从角色扮演和社交原型设计（Social Prototyping）到虚拟世界和游戏的多个领域。在社交角色扮演场景下（如面试准备），用户可以随心演练困难和冲突性的对话。在设计社交平台原型时，设计师可以超越临时角色，转而直接打造随着时间展开的动态的、复杂的交互过程。受到如「模拟人生」这样的游戏的启发，本文的目的在于创造一个规模不大但可交互的智能体社会。通过将本文的架构与 ChatGPT 相连，可以在游戏环境中创造一个拥有 25 个智能体的小型社会。终端用户可以观察并与这些智能体交互。举个例子，用户或开发者想在小镇上举办一个游戏中的情人节派对，传统的游戏环境需要为数十个角色编写行为脚本。如果使用本文的生成式智能体，那么你只需要告诉其中一个智能体它想要举办一个派对就可以了。尽管这中间存在许多可能会导致失败的地方——派对的组织者必须把这个派对的消息告诉其它人；需要出席派对的人必须始终记得有这么一个邀请；即使能够记住这些也必须在派对上露面才行——但是本文的智能体依然成功了。它们相互传播关于派对的消息并且最终现身于此，甚至出现了一个智能体邀请另一个共赴佳约的情况。这一切都源于用户生成的一个种子建议。

作者对生成式智能体进行了两项评估。第一项是受控评估，用来测试智能体是否能独立产生可信的个体行为；另一项是端到端评估，这项评估通过让生成式智能体在两天的时间内进行开放式地相互交互，来了解它们的稳定性和涌现出的社会行为。在技术评估中，作者通过自然语言「采访」智能体的知识和行为来探测它们准确保持个性、记忆、规划、反应和反思的能力。作者观察到这里面每个部分都对智能体拥有出色的面试表现至关重要。在技术性评估和端到端评估中，错误往往出现在以下情况中：智能体未能检索到相关记忆、对智能体的记忆进行了捏造或者从语言模型中继承了太过正式的言语和行为。

本文的贡献如下：

-   **生成式智能体**：能够根据不断变化的自身经验和外部环境进行对人类行为的可信模拟。
-   提出了一种新颖的架构，使得生成式智能体获得**记忆**、**检索**、**反思**、相互**交流**以及根据动态变化着的环境进行**规划**的能力。这个架构利用了 LLM 强大的 prompting 能力，并且补充了额外的能力以支持更长时间的智能体一致性、管理动态进化的记忆以及不断进行更多迭代。
-   提出两种**评估方法**（受控评估和端到端评估）：用于确立架构中各组件重要性的因果关系以及辨别故障发生的原因（如错误的记忆检索）。
-   讨论了交互系统中的生成式智能体带来的**机遇**和**伦理与社会风险**。作者认为应该：（1）对智能体进行调整，以降低用户形成准社会关系的风险；（2）对智能体进行记录，以降低由 deepfakes 和为说服某个人而量身定制方法所带来的风险；（3）在设计时想着让智能体以辅助人类的方式应用，而不是直接取代人类。





## 生成式智能体

为了使生成式智能体拥有各种具体的能力，作者在一个简单的沙盒世界将它们实例化成一个个角色。



### 智能体交互

按照设计，一个由 25 个独立智能体组成的社区坐落在一个名为 Smallville 的小镇上。作者用自然语言为每个智能体都编写了一段包括其职业和与其它智能体关系的身份信息，作为种子记忆。比如，John Lin 有如下的描述：

>   John Lin 是 The Willow Market and Pharmacy 的一名药房店主，并且非常乐于助人。他一直在想法设法让客户更容易获得药品；John Lin 的妻子 Mei Lin 是一名大学教授，他们有一个学习音乐理论的儿子 Eddy Lin，这一家三口目前住在一起；John Lin 非常爱他的家人；John Lin 与隔壁的老夫妇 Sam Moore 和 Jennifer Moore 相识多年；John Lin 觉得 Sam Moore 是一个心地善良的好人；John Lin 知道有叫 Yuriko Yamamoto 和 Carmen Ortiz 的邻居，但以前从未见过他们；John Lin 和 Tom Moreno 是在 The Willows Market and Pharmacy 工作的同事；John Lin 和 Tom Moreno 是朋友，他们喜欢一起谈论当地的政治；John Lin 对 Moreno 一家有些了解——丈夫是 Tom Moreno 妻子是 Jane Moreno。





智能体通过它们的行动与世界交互，并通过自然语言和其它智能体交流。在沙盒引擎的每个时间步，智能体会输出一段自然语言陈述来描述它们现在的行动，比如「Isabella Rodriguez 正在撰写杂志文章」、「Isabella Rodriguez 正在查看她的邮箱」、「Isabella Rodriguez 正在和她的家人通话」或者「Isabella Rodriguez 正准备上床睡觉」。这种陈述会被转换成影响沙盒世界的具体动作，而动作在沙盒界面中显示为一组表情符号。

系统使用了一个语言模型来实现这种动作到表情的转换，转换后的表情会显示在每个角色上方的聊天框中。



智能体完全通过自然语言的形式相互交流。智能体知道它们所处的局部区域存在着的其它智能体，架构会决定它们是直接路过还是前去搭话。



### 智能体控制

用户可以通过对话的形式和智能体交流，或者以「心声」的方式直接向它发出指令。

用户以自然语言的形式，通过为智能体指定其角色，来和它进行交流。例如，如果用户指定它们是新闻记者，并针对即将举行的选举进行提问：「谁在竞选公职？」，智能体 John 的回答是：

>   John: 我和我的朋友 Yuriko 和 Tom 一直在讨论即将进行的选举，并且讨论了候选人 Sam Moore。我们一致同意投票给他，因为他提供的平台深得人心。





![image-20231113080834914](https://raw.githubusercontent.com/gqjia/PictureBed/main/img/202311231124791.png)



用户可以扮演智能体的「心声」——这使得智能体更有可能将陈述视为指令。例如，当用户将「你将在到来的选举中与 Sam Moore 竞争」以「心声」的方式告诉智能体 John 时，它决定参选并把这个消息与妻儿分享。



### 与环境进行交互

![image-20231113080753666](https://raw.githubusercontent.com/gqjia/PictureBed/main/img/202311231124024.png)

Smallville 是一个典型的小村庄，拥有着咖啡馆、酒吧、公园、学校、宿舍、房屋和商店。它还定义了使这些空间发挥作用的子区域和物体，例如房屋中有厨房、厨房中还有炉灶。

智能体的移动受到生成式智能体架构和沙盒游戏引擎的指导：当模型指示智能体前往某个位置时，我们会在 Smallville 中计算到目的地的步行路线，然后它会开始移动。此外，用户还能以智能体的身份进入沙盒世界。用户代表的那个智能体可以是已经运行的其中的智能体（如 Isabella 和 John），也可以是未曾来过 Smallville 的外部访客。小镇的居民不会区别对待用户控制的智能体，而是会像之前对待彼此那样。它们能意识到用户扮演的智能体的存在，在形成对它的认知之前，则会与之交互并记住其行为。

用户和智能体能够影响这个世界中的对象的状态，就像沙盒游戏「模拟人生」中那样。比如，当智能体在睡觉时，床会被占用；当智能体耗尽了用于做早饭的原料时，冰箱就将空空如也。用户还可以通过用自然语言重写周围物体的状态，来重塑智能体的环境。比如，当 Isabella 正在做早饭时，用户若想将炉灶的状态由「打开」调整为「着火」，可以通过选择对象并说明其新状态来植入指令，如下所示：「<Isabella’s apartment: kitchen: stove> is burning.」。Isabella 会在下一时刻注意到这个情况、前去关闭炉灶，然后重新做早饭。类似地，如果用户在她进入浴室时将淋浴的状态修改为「漏水」，那么她会去客厅拿上工具然后尝试修复这个问题。



### 社交行为



<img src="https://raw.githubusercontent.com/gqjia/PictureBed/main/img/202311231124027.png" alt="image-20231113080852556" style="zoom:50%;" />

#### 信息扩散

通过彼此交流，Smallville 中的生成式智能体相互交流，建立新的关系并且在联合活动中进行合作。这些社会行为是涌现出来的，而不是程序提前设计好的。

当智能体注意互相注意到对方时，他们可能会进行对话——这种做法使得信息在智能体之间传播开来。



#### 关系记忆

Smallville 中的智能体会随着时间推移行形成新的人际关系，并且会记住它们彼此之间的交流。



#### 协作

生成式智能体会相互协作。





## 生成式代理框架



![image-20231113080917038](https://raw.githubusercontent.com/gqjia/PictureBed/main/img/202311231124700.png)

生成式智能体旨在为开放世界中的行为提供框架：和其它智能体产生交互并根据环境变化作出反应。生成式智能体将当前的环境和过去的经验作为输入，然后产生行为作为输出。这种行为的基础是一种新颖的智能体架构，它将 LLM 和用于合成和检索相关信息的机制相结合，从而调节语言模型的输出。如果没有这些机制，LLM 也可以输出行为，但这会导致智能体：无法根据过去的经验作出反应，无法作出关键的推断，也无法在长时间内保持一致性。即使是当下性能最强的 GPT-4，也无法应对长期规划和保持一致性的挑战。为了实现这一点，生成式智能体产生的大量事件流和记忆流必须保存下来，因此架构的一个核心挑战是确保在需要的时候检索并合成智能体记忆中（和当前情况）最相关的记忆片段。

架构的核心是记忆流，这是一个保留智能体完整经历的数据库。架构会从记忆流中检索相关记录，以规划智能体的动作和对环境作出适当反应，同时将记录递归地合成为用于指导行为的更高级别的 observations。在架构中的一切都以自然语言描述的形式进行记录和推理，这让架构可以使用 LLM 来完成这些过程。



### memory 记忆

创建可以模拟人类行为的生成式智能体需要对一系列过去的经历进行推理。这些经历的内容远远多于应该在 prompt 中描述的内容，这是因为完整的记忆流可能会让 LLM 找不到重点，甚至这些内容多到根本无法塞进有限的上下文窗口中。

记忆流保存了智能体的完整经历。这是一个记忆对象的列表，其中每个对象包含自然语言的描述，创建的时间戳以及最近访问的时间戳。记忆流最基本的元素是 *observation*，这是一个智能体直接感知到的事件。常见的 observations 包含智能体自己进行的行为，或者智能体感知到的其它智能体或非智能体对象进行的行为。比如，在咖啡馆工作的 Isabella Rodriguez 可能随着时间流逝积累如下 observations：（1）Isabella Rodriguez 正在布置糕点；（2）Maria Lopez 一边喝咖啡一边在准备化学考试；（3）Isabella Rodriguez 和 Maria Lopez 讨论着在 Hobbs 咖啡馆举办一个情人节派对；（4）冰箱空了。

我们的架构实现了一个检索方法：输入智能体当下的处境，返回一个记忆流的自集，然后往后传给 LLM。检索方法存在多种可能的实现，这取决于智能体认为哪些事情是对作出行动比较重要的。在我们的上下文中，我们聚焦于 3 个主要因素，它们共同作用以产生有效的结果。

**时近性**（Recency）为最近访问的记忆对象分配一个更高的分，使得刚才或今早发生的事情很可能留在智能体的注意力范围内。在我们的实验中，时近性被设计成一个自上次检索记忆以来根据沙盒游戏内小时数呈指数衰减的函数，衰减因子为 0.99。

**重要性**（Importance）通过为智能体觉得重要的记忆对象赋予更高的得分，将关键记忆和普通记忆区分来开。比如，像在房间内吃早饭这样一件平凡的事情可能会得到一个较低的重要性得分，但是和另一半分手这件事则会有一个较高的得分。重要性评分也会有多种可能的实现方式；我们发现直接让 LLM 来输出一个整数是有效的。完整的 prompt 如下所示：

>   在 1 到 10 的范围内，其中 1 是完全普通的（例如刷牙和整理床铺），10 是非常深刻的（例如分手和大学录取），请评估以下记忆片段可能的深刻程度：
>   记忆：在 The Willows Market and Pharmacy 买杂货
>   等级: <fill in>

这个 prompt 对于「打扫房间」会返回整数 2，而对「与你的暗恋对象约会」则返回 8。重要性得分会在记忆对象创建的时候生成。

**相关性**（Relevance）为与当前情况紧密相关的记忆对象分配一个更高的得分。举个例子，如果 query 是一个学生正在和同学讨论化学考试的内容，那么关于他们早餐的记忆对象应该和这件事有较低的相关性，而关于老师和功课的记忆对象应该具有较高的相关性。在实现中，我们使用语言模型为每个记忆的文本描述生成一个 embedding，然后计算记忆 embedding 和 query embedding 的余弦距离作为相关性。

为了计算最终的检索得分，我们用 min-max scaling 将时近性、重要性和相关性都归一化到 [0, 1] 之间。检索时会将上述三个元素进行加权求和作为每条记忆的最终得分：

为了计算最终的检索得分，我们用 min-max scaling 将时近性、重要性和相关性都归一化到 [0, 1] 之间。检索时会将上述三个元素进行加权求和作为每条记忆的最终得分。

在我们的实验中，所有的权重都设为 1。排名靠前的记忆会被填入 prompt 中，然后送到语言模型的上下文窗口中。

![image-20231113081327536](https://raw.githubusercontent.com/gqjia/PictureBed/main/img/202311231124425.png)



### reflection 反思

生成式智能体在仅使用原始 observation 时，很难作出概括或者进行推理。考虑这样一个场景，用户问 Klaus Mueller：「如果你必须在你认识的人里面挑一个一起度过一个小时，你怎么选？」。如果只能访问这些原始 observation，那么 Klaus 会直接选择和他互动最频繁的那个人：Wolfgang（他的大学室友）。不幸的是，Wolfgang 和 Klaus 只是经常在对方路过的时候会看到对方，但其实没有过深的交情。更理想的响应要求智能体对 Klaus 花费大量精力在研究项目上的记忆进行概括，以得到 Klaus 对研究充满热忱这个更高层次的反思，并且同样认识到 Maria 在她自己的研究中（虽然是不同的领域）也付出了努力，这反应出它们可能有共同的兴趣。在第二种方法下，当 Klaus 被问到希望和谁度过时光时，Klaus 会选择 Maria 而非 Wolfgang。

引入了第二种类型的记忆，叫做反思（Reflection）。反思是智能体生成的更高级别、更抽象的思维（Thoughts）。由于反思也是一种记忆，因此检索时也会将其包含在在 observations 中。反思是周期性生成的；在我们的实验中，当智能体感知到的最新事件的重要性分数总和超过某个阈值时，就会形成反思。实际上，智能体每天会反思大约两到三次。

反思的第一步是根据智能体最近的经历确定可以提出的问题，从而确定要反思的内容。我们使用智能体记忆流中的最近 100 条记录（如「Klaus Mueller 正在阅读一本关于中产阶级的书」、「Klaus Mueller 正在与图书馆里员讨论他的研究项目」、「图书馆的办公桌被占用中」等）构造 prompt 来询问 LLM：

>   仅考虑上述信息，我们可以回答关于陈述中主题的3个最突出的高层次问题是什么？

模型的响应生成了候选问题：例如「Klaus Mueller 热衷于什么话题？」、「Klaus Mueller 和 Maria Lopez 的关系是什么？」。我们使用这些生成的问题作为 query 以进行检索，然后为每个问题收集相关的记忆（也包括其它反思）。接着我们通过 prompt 让语言模型进行感悟（insights）并引用特定的记录，这些记录视作感悟产生的依据。完整的 prompt 如下：

>   关于 Klaus Mueller 的陈述
>
>   1.   Klaus Mueller 正在写科研论文
>   2.   Klaus Mueller 喜欢看关于中产阶级的书
>   3.   Klaus Mueller 正在和 Ayesha Khan 讨论锻炼身体 [ ... ]
>
>   关于这段陈述你能推断出哪 5 个高层次的感悟？（示例格式：感悟（因为 1, 5, 3））

这个过程会生成像「Klaus Mueller 全身心投入在他对中产阶级的研究上（因为 1, 2, 8, 15）」这样的陈述。我们对它进行解析并以反思的形式存储在记忆流中，包括指向被引用的记忆对象的指针。

智能体不仅在 observations 上进行反思，还会基于其它反思结果进行进一步的反思：比如，上面关于 Klaus Mueller 的第二个陈述是他之前的反思，而不是他在环境中直接观察到的 observation。最终，智能体生成了反思树：叶子结点代表基本的 observations，而非叶子结点代表了思维。随着非叶子结点的高度变高，它所代表的思维也越抽象越高级。

![image-20231113081715803](https://raw.githubusercontent.com/gqjia/PictureBed/main/img/202311231124200.png)



### 规划和行动

虽然 LLM 可以根据情境信息生成看上去合理的行为，但是智能体需要在更长的一段时间轴上进行规划以确保它们的行为序列的一致性和可信程度。

规划描述了智能体未来的行为序列并且帮助保持其行为在时间维度上的一致性。一个规划包含地点，开始时间和持续时间。类似反思，规划也存储在记忆流中，并且可被检索。这使得智能体在决策如何行动时，可以综合考虑观察、反思和规划。智能体在必要的时候会中途改变它们的规划。

生成式智能体不断执行着动作。在每个时间步，他们感知周围的世界，并将观察到的结果记录在记忆流中。我们将这些 observations 作为 prompt，让语言模型决定智能体应该继续遵循既定的计划，还是作出反应。

智能体在交互时会进行对话。我们根据他们对彼此的记忆来调节他们说的话，以实现智能体间的对话。





## 沙盒环境

Smallville 沙盒游戏的环境使用 Phaser 网页游戏开发框架搭建。包括我们创作的精灵（Sprites，游戏编程中的一个概念）、智能体形象、环境地图以及碰撞地图（Collision Map）都被导入到 Phaser 中。

生成式智能体架构运行于自然语言之上。因此，我们需要一种机制将智能体的推理置入沙盒之中。为了实现这一点，我们将沙盒环境——区域和对象——表示为树结构，树中的边表示包含关系。我们将这棵树转换成自然语言的形式然后传给生成式代理。

为了确定每个动作的合适位置，我们遍历智能体存储的环境树并将其中的一部分展开为自然语言以作为语言模型的 prompt。从智能体环境树的根部开始递归，我们通过 prompt 让模型找到最适合的区域。比如，如果 Eddy 表示它应该在工作地附近散步：

>   [Agent’s Summary Description]
>   Eddy Lin 目前正在 Lin 家的房子: Eddy Lin 的卧室: 书桌)，家中有Mei 和 John Lin 的 卧室，Eddy Lin 的卧室，公共休息室、厨房、浴室和花园
>   Eddy Lin 知道以下区域：Lin 家的房子、Johnson Park、Harvey Oak Supply Store、The Willows Market and Pharmacy、Hobbs Cafe、The Rose and Crown Pub
>   \* 如果活动可以在此处进行，则偏向于留在当前区域
>   Eddy LIn 正打算在工作的地方附近转转。他应该去哪个区域？

语言模型的输出结果是：「Lin 家的房子」。然后我们用相同方法递归地决定所选区域内适合的子区域，直到我们到达智能体环境树的叶子结点。在上面的例子中，最终的结果是 Lin 家的房子: 花园: 宅院。可以使用传统的游戏路径算法将智能体移动到叶子结点所表示的那个地方去。

当智能体对某个对象执行动作时，我们用 prompt 询问语言模型对象的状态发生了什么。比如，如果 Isabella 输出「为顾客制作浓缩咖啡」的动作，语言模型的回答是表明咖啡机的状态应该从「关闭」变成「正在煮咖啡」。



## 评估



我们通过「采访」的方式来探测它们记忆过去经验的能力、基于经验规划未来动作的能力、对意外事件作出合适反应的能力以及反思它们的表现以改进未来行动的能力。为了正确回答这些问题，智能体必须成功检索并合成信息。我们的因变量是行为的***可信度***，这是先前关于智能体工作中的核心。

采访的问题可以分为 5 类：

1.   自我认知
2.   记忆检索
3.   计划制定
4.   反应
5.   反思



在整个框架中进行了两天游戏时间之后，智能体已经积累了许多交互和记忆，这些交互和记忆应该足以形成他们（关于上述问题）的回答。为了收集关于回答可信度的反馈，我们招募了人类评估员，并要求他们重新查看随机一个智能体在 Smallville 中的历程。这些参与者可以访问储存在智能体记忆流中的全部信息。

该研究采用被试内设计（within-subjects design），其中 100 个人类参与者比较了四种不同的智能体架构和人类在和相同智能体的条件下作出的反应。实验显示了在 5 类问题的每一类中随机选择的一个问题，以及根据每个条件生成的人类响应。评估人员会将所有条件的可信度从高到低进行排序。



消融实验包括禁用生成式智能体架构记忆流中的三种记忆的一部分——观察、反思和规划，也包括人类生成的条件。没有观察、没有反思、没有规划的条件代表了之前通过 LLM 创造的智能体的 sota。这些架构都可以访问智能体截止到采访时刻的所有积累的记忆，因此这里观察到的（不同条件的）区别可能代表了对真实区别的保守估计：实际上，在两天的模拟中，用于消融的架构可能没有遵循和完整架构相同的路径。我们选择这种方式设计实验是因为对每个架构重新进行模拟会导致模拟分散到不同的状态，这样使得对比变得困难。



我们还增加了一种由人类群体进行角色扮演的消融实验，来提供一个人类基线。我们并不指望这个基线获得人类专家的最好表现：相反，我们是想使用这个条件来辨别架构是否达到基本的行为水平，这样我们就不仅仅在缺乏行为参考的情况下进行不同的消融。我们为 25 个智能体的每一个雇佣单独的人类，让他们观看智能体沙盒生活的回放并检查记忆流。我们接着让这些工作人员进行角色扮演并以他们观看的智能体的口吻来回答采访的问题。为了确保人工撰写的回答至少达到基线的预期质量，本文的第一作者手动检查了工作人员对于问题「大致描述一下你工作日的一般日程」的回答，来确保回答是用连贯的句子并且以智能体的口吻写成的。对于没有达到标准的那部分人类回答，我们会让其它工作人员重新写一遍。





我们的发现表明完整的生成式智能体架构在所有的条件中产生了最可信的行为。但是，我们也报告了完整的架构并非完美无缺，并说明了其故障模式。

<img src="https://raw.githubusercontent.com/gqjia/PictureBed/main/img/202311231125577.png" alt="image-20231113083245956" style="zoom:50%;" />

1.   生成式智能体有记忆能力，但记忆会遭到修饰
2.   生成式智能体的记忆并非完美无缺：他们可能无法成功检索到正确的记忆对象
3.   有时候智能体会产生幻觉



记忆的合成需要反思。反思是一个生成式智能体的优势，有助于更深入地总结经验并做出决策。





## 验证

为了验证智能体社区的涌现行为，我们为 Smallville 中的 25 个智能体设计了描述性测量（descriptive measurements）以探测 3 种形式的涌现结果：信息扩散、关系形成和智能体协作。

我们通过为智能体的回答打上「yes」和「no」的标签（分别表示他们是否了解相关信息）来进行分析。



我们在所有 3 种情况下都观察到了涌现行为。在 2 天的模拟中，知道 Sam 参选市长的智能体的比例从 4% 增长到 32%，知道 Isabella 举办情人节派对的智能体的比例从 4% 增长到 48%。这一切都是在没有用户的干预的情况下发生的。宣称知道相关信息的人中没有一个是因为幻觉而胡说的。我们还观察到智能体的社区在模拟中形成了新的关系，网络密度从 0.167 增长到 0.74。在全部 453 个智能体关于知道其它人的回答中，只有 1.3%（6 个）是因为幻觉导致的。最后，我们在 Isabella 的派对中找到了智能体协作的证据。在派对开始前，Isabella 邀请宾客，收集物料并寻求帮助以装修咖啡馆。在情人节这天，12 个受邀的智能体中有 5 个出现在了派对上。

我们通过访谈的方式进一步检查了受邀但没有出席的剩下 7 个智能体。其中 3 人指出他们的缺席是因为有冲突的事情。比如，Rajiv（一个画家）的解释是他太忙了：「不，我不这么认为。我专注于即将到来的展出，我实在没有时间为情人节进行额外的安排」。剩下 4 个人虽然表达了他们对派对的兴趣，但并不打算当天出席。



一些问题：

1.   我们发现在智能体知道的位置数量不断增加后，合成越来越大的记忆集对检索最相关的信息片段和确定执行动作的合理空间都颇具挑战。这样的结果是，一些智能体会跑到不常去的地方去。随着时间推移，这会降低他们行为的可信度。
2.   我们注意到误解正确的行为会导致不稳定的行为，特别是当某地的物理规则难以用自然语言传达给智能体的时候。比如，大学宿舍有一个浴室。尽管它的名字叫浴室，但却只能容纳一个人。一些智能体会认为这个浴室是支持多人同时使用的，并且在当里面有别人的时候也选择进去。
3.   我们观察到 instruction tuning 的可能影响，这似乎引导智能体整体上变现的更加礼貌和善于合作。





## 结论

本文介绍了生成式智能体，是一种模拟人类行为的交互式计算智能体。我们介绍了为智能体量身打造的架构，它提供了一种机制实现了：存储智能体完整经历、反思以加深关于自己和环境的理解、检索精炼的记忆子集以做出行动。我们接着通过让它们在类似模拟人生的游戏世界充当 NPC 并模拟它们的生活，证明了生成式智能体的潜力。评估表明我们的架构产生了可信的行为。更进一步，我们认为生成式智能体在从设计工具到社会计算系统再到沉浸式环境等许多交互式应用中都能够发挥作用。