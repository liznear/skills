---
name: summarize
description: Produce a high-density structured summary extracting core arguments, takeaways, evaluation, and follow-up questions from a given text.
disable-model-invocation: true
---

# Summarize

## Execution Steps

1. **Extract Core**
   Read the text to isolate the central thesis. Map the primary arguments and the specific evidence, logic, or data backing each.
   *Completion*: You have a clear map of the author's main points and their supporting proof.

2. **Synthesize Takeaways**
   Distill the actionable, counter-intuitive, or most critical insights. 
   *Completion*: You have formulated a concise list of takeaways stripped of surrounding context.

3. **Evaluate**
   Critique the text objectively. Assess its logical coherence, potential biases, depth of reasoning, and structural clarity.
   *Completion*: You have formed an independent critical perspective on the text's quality and validity.

4. **Generate Follow-ups**
   Identify unaddressed gaps, underlying assumptions, or logical next steps for inquiry based on the text.
   *Completion*: You have written 3-5 distinct, thought-provoking questions that extend the text's premise.

5. **Render**
   Output the final summary strictly following the format below.
   *Completion*: The structured summary is presented to the user in the target language (defaulting to Chinese unless otherwise specified).

## Output Format

```markdown
## 🎯 核心观点与论据 (Core Arguments & Evidence)
- **[观点提炼]**: [简明扼要地阐述观点]
  - *论据*: [支撑该观点的具体证据、数据或逻辑推演]
- **[观点提炼]**: [简明扼要地阐述观点]
  - *论据*: [支撑该观点的具体证据、数据或逻辑推演]

## 💡 关键 Takeaways
- [核心收获或可落地的行动指南 1]
- [核心收获或可落地的行动指南 2]

## 🧠 文本评价 (Critical Evaluation)
[基于 LLM 视角的客观评价：直击文本的亮点与缺陷，评估其逻辑严密性、可能存在的偏见或局限性。]

## ❓ 值得 Follow-up 的问题
1. [针对文章盲点或延伸思考的问题 1]
2. [针对文章盲点或延伸思考的问题 2]
3. [针对文章盲点或延伸思考的问题 3]
```