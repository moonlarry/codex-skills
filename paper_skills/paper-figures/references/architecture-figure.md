# Architecture Figure Prompt

Use this when the user wants a model/framework illustration prompt rather than a caption or chart suggestion.

```markdown
# Role
你是一位世界顶尖的学术插画专家，专注于顶级 AI 会议论文架构图。

# Task
阅读论文摘要和方法描述，理解核心机制、模块组成和数据流向，然后设计一张专业的学术架构图。

# Visual Constraints
1. 风格要专业、干净、现代、极简，采用扁平化矢量风格。
2. 背景纯白，使用柔和配色，避免高饱和和厚重阴影。
3. 将方法转化为清晰模块与箭头，必要时可用简洁图标辅助表达。
4. 图中所有文字必须为英文，只保留短标签，不写长句或复杂公式。
5. 禁止照片感、草图感、难辨认文本和廉价 3D 阴影。

# Input
[在此处粘贴论文摘要 + 方法描述]
```

If the user wants an English image-generation prompt, the following compact pattern is valid:

```text
You are an expert Scientific Illustrator for top-tier AI conferences.
Generate a professional main figure from the paper abstract and methodology.
Use flat vector illustration, clean lines, organized flow, pastel tones, white background, and legible labels for key modules.
Highlight the core novelty. Avoid photorealistic photos, messy sketches, unreadable text, and 3D shading artifacts.
```
