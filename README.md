# vue3-clean

干净、克制的 Vue 3 代码风格 skill。

强制 AI 写出简洁、直接、不过度封装的 Vue 3 代码。简单业务逻辑直接写在组件里，只有真正可复用或复杂度够高时才抽离 composable。

## 安装

### 方式一：使用 skill-installer（推荐）

```bash
bash /root/.grok/skills/skill-installer/scripts/install-skill.sh \
  --repo 你的用户名/vue3-clean \
  --path .
```

或使用完整 URL：

```bash
bash /root/.grok/skills/skill-installer/scripts/install-skill.sh \
  --url https://github.com/你的用户名/vue3-clean/tree/main
```

### 方式二：手动复制

把本仓库整个目录复制到你的 `.grok/skills/vue3-clean/` 即可。

安装后请**新开一个会话**，skill 才会生效。

## 使用

直接让 AI 写 Vue3 组件，或明确说：

- 「用 vue3-clean skill」
- 「写干净一点的 Vue3 代码」
- 「不要过度封装」

## 核心原则

1. **简单逻辑直接写在组件里**，不轻易抽 composable
2. 只有真正可复用（2+ 处使用）或复杂度够高时才封装
3. 默认使用 `<script setup>` + Composition API
4. 拒绝无意义的 service 层、工厂、过多中间函数
5. 代码扁平、直接、可读，像有经验的人写的产品代码

## License

MIT
