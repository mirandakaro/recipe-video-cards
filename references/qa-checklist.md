# Final image acceptance checklist

Review the generated PNG with a multimodal model and verify the file exists and is readable.

## Recipe fidelity

- [ ] Title and dish identity are correct.
- [ ] Yield/servings match the structured recipe.
- [ ] Every displayed gram, count, ratio, time and temperature exists in evidence or is visibly labeled as a suggestion.
- [ ] Ingredient groups and sauces are complete.
- [ ] STEP order matches the real method.
- [ ] Doneness cues and important equipment are retained.
- [ ] No absent ingredient, fake garnish, invented nutrition, or made-up quantity appears.
- [ ] Creator calorie claims are labeled `作者口径` with relevant yield/partial-use context.

## Visual fidelity

- [ ] Warm cream/parchment background and reddish-brown stitched border.
- [ ] Large independent watercolor food hero; text does not cover food.
- [ ] Left ingredient/tools/tips boxes are present.
- [ ] Right STEP cards are individually numbered and readable.
- [ ] Bottom serving/kcal badge is complete and not clipped.
- [ ] No six-grid, PPT, web screenshot, dense report, or commercial-photo drift.
- [ ] Chinese has no gibberish, misspelling, collision, truncation, or tiny unreadable text.
- [ ] Bottom edge and corners have safe margins.

## Decision rule

Any wrong/invented number, missing critical step, unreadable Chinese, or golden-layout failure means **reject and regenerate**. Cosmetic imperfections may pass only if they do not reduce cooking usability or visual identity.

Suggested review prompt:

> 菜名不同正常。请同时对照结构化食谱与 golden 手账版式验收：核对全部克数、数量、时间、温度、步骤顺序和热量口径；再检查奶油纸底、红棕缝线、中央大水彩成品、左侧食材/工具/贴士、右侧 STEP 1–6、底部徽章，以及乱码、截断、重叠。任何编造数值或关键缺失直接判不通过。输出分数、问题列表和通过/不通过。
