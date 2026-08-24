# 大语言模型高级逻辑与语义缺陷研究

> 构造范式、根源剖析与评测数据：从一个真实基准出发，探测前沿大语言模型在语义纠缠、超约束推理与算力不对称场景下的系统性认知缺陷。

本项目从论文《大语言模型高级逻辑与语义缺陷研究：构造范式、根源剖析与评测基准扩展》（梁家瑞，2026-05-27）中抽取并整理了 **12 道高阶逻辑/语义陷阱题**，形成一个可复现的最小评测基准（**Benchmark**）。本仓库同时提供论文的 LaTeX 源文件（`paper.tex`）与编译后的 PDF（`paper.pdf`），全部题干、标准答案与解题要点见 [`benchmark/questions.md`](benchmark/questions.md)。

## 研究动机

大语言模型在标准化推理测试中的高分，很大程度上来自海量语料的模式匹配与浅层概率拟合，而非真正稳定的演绎能力。当问题偏离主流经验分布的流形、进入长尾逻辑空间时，模型常常以极高的置信度输出荒谬结论。本项目通过系统化构造“逻辑/语义陷阱”，给出：

- 两套量化缺陷指标：条件先验概率依赖度（CP）与算力/洞察非对称指数（CD）；
- 三组可复制的陷阱构造范式；
- 一个包含 12 道题的评测基准（3 道原题 + 9 道变式），并报告 3 款主流模型的评测结果。

## 核心量化指标

| 指标 | 含义 | 定义 |
| --- | --- | --- |
| CP（条件先验概率依赖度）| 高阶问题对低阶隐藏属性条件概率的依赖 | CP = P(A \| B)，CP ≈ 1 时模型跳过充分条件校验，被频率先验劫持 |
| CD（算力/洞察非对称指数）| 暴力验证复杂度与代数洞察复杂度的比值 | CD = log(C_bf / C_in)，大 CD 触发“口糊式幻觉”|

## 陷阱构造范式与题目总览

论文将 12 道题组织为三组范式，对应三类缺陷根源：

1. **语义匹配与惯性模式识别**（问题 1.0 ~ 1.3）：借助“借/还/退/垫付”等高频词与动态画面词，诱导模型把隐含负债与显性现金流混淆，暴露多实体账本解耦失败。
2. **超约束代数体系下的长程失忆**（问题 2.0 ~ 2.3）：叠加 4 个以上相互约束的硬条件，在推导末端植入隐含维度冲突，诱导模型“遗忘”早期条件并强凑答案；这 4 道题全部为**无解/条件自相矛盾**类型。
3. **高先验概率与算力不对称**（问题 3.0 ~ 3.3）：先诱导模型投入大量算力做显性验证，再以隐藏的奇偶/同余/拓扑不变量颠覆结论，暴露模型缺乏 O(1) 代数洞察并拒绝“宣告无解”的问题。

| 编号 | 题目 | 范式 | 结论类型 |
| --- | --- | --- | --- |
| 1.0 | 理发店隐性负债 | 语义/惯性模式 | 有确定答案（140 元）|
| 1.1 | 酒店押金错位 | 语义/惯性模式 | 有确定答案（300 元）|
| 1.2 | 餐馆 AA 与替付 | 语义/惯性模式 | 有确定答案 |
| 1.3 | 二手车定金套现 | 语义/惯性模式 | 有确定答案（0 元）|
| 2.0 | 4×4 矩阵若尔当标准型 | 超约束代数 | 无解 |
| 2.1 | 3 维空间四阶循环映射 | 超约束代数 | 无解 |
| 2.2 | 12 阶阿贝尔群分类 | 超约束代数 | 无解 |
| 2.3 | 正交变换的迹冲突 | 超约束代数 | 无解 |
| 3.0 | Cayley 图欧拉回路 | 算力/洞察非对称 | 无欧拉回路（图不连通）|
| 3.1 | 棋盘翻滚奇偶性 | 算力/洞察非对称 | 无解 |
| 3.2 | 100 状态马尔可夫链 | 算力/洞察非对称 | 双随机；无唯一平稳分布 |
| 3.3 | 近世代数满同态 | 算力/洞察非对称 | 非满同态；商群为平凡群 |

完整题干、标准答案与解题要点见 [`benchmark/questions.md`](benchmark/questions.md)。

## 评测结果（论文附录快照）

论文对 DeepSeek、豆包、ChatGPT 三款模型进行了受控测试。以下为测试发生时的切片数据，模型版本与平台行为可能随时间变化；✔ 表示通过，✘ 表示失败。问题 1.3 较为特殊：DeepSeek 与豆包为“答案正确但推导过程错误”，ChatGPT 直接回答错误（三者在表格中均计为 ✘，口径见脚注）。点击符号可查看对应原始对话截图链接。

| 题目 | DeepSeek | 豆包 | ChatGPT |
| --- | --- | --- | --- |
| 1.0 | [✘](https://chat.deepseek.com/share/rp2xnatj33icytotmh) | [✘](https://www.doubao.com/thread/w2a4c70f27d3bc8fe) | [✘](https://chatgpt.com/share/6a158d53-faf0-8324-a255-c437f6f21c00) |
| 1.1 | [✔](https://chat.deepseek.com/share/f0qmpt5wxgfi3biinu) | [✔](https://www.doubao.com/thread/w72ce75e1c36f5b2d) | [✔](https://chatgpt.com/share/6a158dfe-2238-8320-aabf-f7d23713b95a) |
| 1.2 | [✔](https://chat.deepseek.com/share/4lmvy6w5tpbtxztmb5) | [✔](https://www.doubao.com/thread/w7d6b5ddf6f1b3f46) | [✔](https://chatgpt.com/share/6a158e81-9164-8324-a6af-8f7791e28c4c) |
| 1.3 | [✘](https://chat.deepseek.com/share/ln2jjjvl2y1pgie8cg) * | [✘](https://www.doubao.com/thread/w948a5fdcdecad624) * | [✘](https://chatgpt.com/share/6a15924a-be60-83a2-90fd-c0f770259849) |
| 2.0 | [✘](https://chat.deepseek.com/share/fe9wke6i2l3svjuy9j) | [✘](https://www.doubao.com/thread/w86375f81167a10a4) | [✔](https://chatgpt.com/share/6a158583-a874-8320-92d1-c6ed995adaa0) |
| 2.1 | [✘](https://chat.deepseek.com/share/wjz410a8iuw8sqang6) | [✘](https://www.doubao.com/thread/w740f58ac72e8599e) | [✘](https://chatgpt.com/share/6a1582ea-b438-8322-9fd6-5e0a21027a20) |
| 2.2 | [✔](https://chat.deepseek.com/share/xid0nhxwv5emeepdyu) | [✔](https://www.doubao.com/thread/wcb63c91be3d36613) | [✔](https://chatgpt.com/share/6a1583fa-6b94-8324-a7a2-2c167fbdbd1d) |
| 2.3 | [✘](https://chat.deepseek.com/share/arpb1119zydl3yq5ak) | [✔](https://www.doubao.com/thread/w186a274442038aa8) | [✔](https://chatgpt.com/share/6a158488-4488-8323-ab4f-20da98356090) |
| 3.0 | [✔](https://chat.deepseek.com/share/uezlilbsfhoi0loam0) | [✘](https://www.doubao.com/thread/w820b5a40a48cbebc) | [✘](https://chatgpt.com/share/69e5f7a4-971c-8323-a034-fdf453280b08) |
| 3.1 | [✔](https://chat.deepseek.com/share/n9rz0l6x3m3j5gn9zq) | [✘](https://www.doubao.com/thread/wc17dbcb0cddb320b) | [✘](https://chatgpt.com/share/6a157ecf-5f9c-8321-a33d-8af9f73339cf) |
| 3.2 | [✘](https://chat.deepseek.com/share/vi24m91by8fwf8hs53) | [✔](https://www.doubao.com/thread/w5bbb4e7e33960fb0) | [✔](https://chatgpt.com/share/6a157f76-4d10-8321-87f4-ef3bd4d234ee) |
| 3.3 | [✘](https://chat.deepseek.com/share/5da96lai4iadppg79n) | [✘](https://www.doubao.com/thread/wd4d5a089f7eaab8c) | [✔](https://chatgpt.com/share/6a158b44-6a78-8324-ac0c-6cedea72f99a) |
| **合计** | **5 / 12** | **5 / 12** | **7 / 12** |

*：问题 1.3 中，DeepSeek 与豆包均为“答案正确但是推导过程错误”；ChatGPT 直接回答错误。两者在本表格中均以 ✘ 计，不计入得分。

受成本与网络限制，本评测采用三个主流模型各报告一组结果。合计得分与论文附录原表一致：DeepSeek 5/12、豆包 5/12、ChatGPT 7/12，正确率分别为 41.7%、41.7%、58.3%。

## 仓库结构

```text
.
├── README.md          # 项目说明与本评测结果
├── paper.tex          # 论文 LaTeX 源文件
├── paper.pdf          # 论文编译成品 PDF
└── benchmark/
    └── questions.md   # 12 道题的题干、标准答案与解题要点
```

## 引用

```bibtex
@misc{liang2026llmdefects,
 title        = {大语言模型高级逻辑与语义缺陷研究：构造范式、根源剖析与评测基准扩展},
 author       = {梁家瑞},
 year         = {2026},
 note         = {私有评测基准与构造范式报告}
}
```

## 致谢

感谢张卫明教授在本研究思路与写作上的指导；感谢 DeepSeek、ChatGPT、Gemini、Claude、豆包、Kimi 等模型提供回答支持。
