# SketchCode

一个深度学习项目，旨在将手绘的网页线框图转换为可运行的 HTML 代码。以下是对该项目的详细介绍：

### 项目目标
项目的主要目标是使用最新的深度学习算法简化设计工作流程，帮助所有企业能够快速创建和测试网页。理想情况下，该模型可以接收网页设计的简单手绘原型，并立即将其转换为可运行的 HTML 网页。

### 项目背景和灵感来源
- **设计工作流程痛点**：在当今的设计工作流程中，通常涉及产品经理进行用户调研、设计师创建原型、工程师将设计转化为代码并部署产品。这一过程可能会受到瓶颈的影响，大型企业可能需要数周时间且涉及多个相关方，而小型企业可能因资源有限在用户界面设计上遇到困难。
- **灵感来源**：受到图像字幕（Image Captioning）领域的启发，特别是 [pix2code](https://arxiv.org/abs/1705.07962) 论文和 Emil Wallner 的相关 [项目](https://blog.floydhub.com/turning-design-mockups-into-code-with-deep-learning/?source=techstories.org)，作者决定将项目重构为图像字幕任务，以网页线框图作为输入图像，对应的 HTML 代码作为输出文本。

### 项目依赖和数据集
- **依赖**：项目依赖于 Python 3、pip 以及一系列 Python 库，如 Keras、TensorFlow、nltk 等，具体版本可参考 `requirements.txt` 文件。
- **数据集**：项目基于 [pix2code](https://github.com/tonybeltramelli/pix2code) 和 [Design Mockups](https://github.com/emilwallner/Screenshot-to-code-in-Keras) 项目合成生成的数据集。初始数据集包含 1,750 个合成生成的网页截图及其相关源代码，每个网页由按钮、文本框和 div 等简单的 Bootstrap 元素组合而成，源代码使用领域特定语言（DSL）编写，编译器用于将 DSL 转换为 HTML 代码。

### 模型架构和工作流程
- **模型架构**：使用图像字幕模型架构，图像通过 CNN 网络处理，文本处理基于起始序列。在每个步骤中，模型对序列的下一个令牌的预测会添加到当前输入序列中，并作为新的输入序列提供给模型，直到模型预测到 *END* 令牌或达到源代码的令牌数量限制。
- **工作流程**：模型预测生成的令牌集由编译器将 DSL 令牌转换为 HTML 代码，以便在任何浏览器中渲染。

### 项目功能和使用方法
- **功能**：
    - 将单张手绘图像转换为 HTML 代码。
    - 将文件夹中的一批图像转换为 HTML 代码。
    - 训练模型，可以从头开始训练或使用预训练模型进行训练。
    - 使用 BLEU 分数评估生成的预测结果。
- **使用方法**：具体的命令行使用方法可参考 `README.md` 文件，例如：
    - 下载数据和预训练权重：
```sh
git clone https://github.com/ashnkumar/sketch-code.git
cd sketch-code
cd scripts
sh get_data.sh
sh get_pretrained_model.sh
```
    - 将单张图像转换为 HTML 代码：
```sh
cd src
python convert_single_image.py --png_path {path/to/img.png} \
      --output_folder {folder/to/output/html} \
      --model_json_file {path/to/model/json_file.json} \
      --model_weights_file {path/to/model/weights.h5}
```

### 项目局限性和未来方向
- **局限性**：模型仅针对 16 种组件的词汇进行了训练，无法预测数据中未出现的令牌。
- **未来方向**：
    - 使用更多元素（如图像、下拉菜单和格式等）生成更多的网页示例。
    - 创建能更好反映实际网页变化性的训练数据集，例如抓取实际网页并捕获网站内容和 HTML/CSS 代码。
    - 使用生成对抗网络（GAN）为手绘草图数据生成更多变化，以创建更逼真的网页图像。

