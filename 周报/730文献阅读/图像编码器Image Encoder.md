**CLIP模型分析：**

多模态预训练神经网络，使用**大量图像和文本的配对**数据进行预训练，以学习图像和文本之间的对齐关系。CLIP模型有两个模态

1. Text Encoder：用于将文本转换为低维向量表示-Embedding。、

   ![image-20260730230531238](image-20260730230531238.png)

2. Image Encoder：用于将图像转换为类似的向量表示-Embedding。

   ![image-20260730230610056](image-20260730230610056.png)

   CLIP模型通过计算文本和图像向量之间的余弦相似度来生成预测。这种模型特别适用于零样本学习任务，即模型不需要看到新的图像或文本的训练示例就能进行预测。CLIP模型在多个领域表现出色，如图像文本检索、图文生成等。

**CLIP组件**

 是一种High-Level的框架，不局限于某个具体的网络结构，可以使用各种不同的子组件来实现相同的结果。

### 图像编码器Image Encoder

使用ResNet50 作为基础架构，并在此基础上根据ResNetD的改进和抗锯齿rect-2模糊池对原始版本进行了修改。同时，还将全局平均池化层替换为注意力池化机制。

第二个架构使用最近引入的Vision Transformer(ViT)进行实验。只进行了小修改，即 在transformer之前对 combined patch 和 position embeddings添加了额外的层归一化。
![在这里插入图片描述](6f23d9010df9ce84033f38135f9df691.png)

### 文本编辑器Text Encoder

Transformer架构，如下图所示，并在此基础上根据Radford模型进行了架构修改。作为基础尺寸，文章使用12层512宽的模型，有8个注意头。、

Transformer 中的[自注意力机制](https://zhida.zhihu.com/search?content_id=251487510&content_type=Article&match_order=1&q=自注意力机制&zd_token=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJ6aGlkYV9zZXJ2ZXIiLCJleHAiOjE3ODU1OTY5ODIsInEiOiLoh6rms6jmhI_lipvmnLrliLYiLCJ6aGlkYV9zb3VyY2UiOiJlbnRpdHkiLCJjb250ZW50X2lkIjoyNTE0ODc1MTAsImNvbnRlbnRfdHlwZSI6IkFydGljbGUiLCJtYXRjaF9vcmRlciI6MSwiemRfdG9rZW4iOm51bGx9.3Ji9RK_18Cfa7R8B6WQ-HJrUH7pfGf3rnUgYqOT09fQ&zhida_source=entity)是创建该上下文化表示的主要机制。

CLIP 对通用 Transformer 所做的一项修改是，它**只会输出一个向量**，而不是上图所示的一个矩阵，它直接提取输入序列中最后一个标记的向量来表示整个输入的文本序列。

![在这里插入图片描述](1bf9ca450bd0b23a308e05ece82393ed.png)

工作的核心是从与图像配对的自然语言中包含的监督中学习感知的想法。

**参数标准**

在机器学习中，有很多种方法可以定义“接近”。可以说，最常见的方法是余弦相似度，CLIP 就是采用这种方法。余弦相似度背后的理念是，如果两个向量之间的角度较小，我们可以说它们是相似的。![img](v2-3e1e8eedf76294d54ed2658c0f567b7e_1440w.jpg)

**计算公式**

![img](v2-03d442ded87cee3913f7ef8bad8e4a61_1440w.jpg)现有工作主要使用MS-COCO 、Visual Genome 和YFCC100M三个数据集。虽然MS-COCO和Visual Genome是高质量的人群标记数据集，但按照现代标准，它们的规模很小，每个数据集大约有10万张训练照片。