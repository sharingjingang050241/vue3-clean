---
name: vue3-clean
description: Write clean, concise Vue 3 code with Composition API. Use when generating, refactoring, or reviewing Vue 3 components, composables, or pages. Prefer simplicity over over-encapsulation. Trigger on Vue3, Vue 3, script setup, clean Vue code, or requests for less nested/encapsulated frontend code.
---

# Vue3 Clean

Write Vue 3 code that is simple, readable, and free of unnecessary abstraction.

## Core Principles

1. **Prefer direct code over premature abstraction.**
   - Simple logic stays inside the component.
   - Only extract a composable when the same logic is used in 2+ places or has real complexity (multiple states + side effects + clear reusable interface).
   - Do not create a composable just because a function is "nice to extract".

2. **Keep components flat.**
   - Avoid deep method chains and intermediate helper functions for one-off logic.
   - Prefer inline expressions when they stay readable (e.g. `v-if="user?.role === 'admin'"`).
   - Group related reactive state together; do not scatter many single-purpose refs.

3. **Use `<script setup>` + Composition API by default.**
   - Prefer `ref` / `computed` / `watch` over Options API unless the project already uses Options.
   - Use `defineProps` / `defineEmits` with TypeScript when possible.
   - Keep the script section short and top-to-bottom readable.

4. **Naming and structure.**
   - Name variables and functions by what they do, not by technical role (`isLoading` not `loadingFlag`).
   - Order in `<script setup>`: imports → props/emits → state → computed → methods → lifecycle/watch.
   - Prefer early returns in functions.

5. **Avoid common over-engineering patterns.**
   - Do not wrap every API call in a custom hook if the project already has a simple request utility.
   - Do not create "service" classes or factories for simple CRUD.
   - Do not add unnecessary `readonly()`, `toRefs()`, or complex reactivity transforms for basic cases.
   - Avoid excessive `watch` + `nextTick` combinations when a computed or direct assignment works.

## Preferred Patterns

### Simple component example

```vue
<script setup lang="ts">
import { ref, computed } from 'vue'

const props = defineProps<{
  userId: number
}>()

const count = ref(0)
const double = computed(() => count.value * 2)

function increment() {
  count.value++
}
</script>

<template>
  <button @click="increment">{{ count }} ({{ double }})</button>
</template>
```

### When to extract a composable

Only extract when:

- Logic is shared by multiple components, **or**
- Logic has non-trivial state + side effects (e.g. debounced search, infinite scroll, form validation with multiple fields).

Example of justified composable:

```ts
// useSearch.ts – reused across list pages
export function useSearch(fetchFn: (q: string) => Promise<any[]>) {
  const query = ref('')
  const results = ref<any[]>([])
  const loading = ref(false)

  const search = useDebounceFn(async () => {
    if (!query.value) {
      results.value = []
      return
    }
    loading.value = true
    try {
      results.value = await fetchFn(query.value)
    } finally {
      loading.value = false
    }
  }, 300)

  watch(query, search)

  return { query, results, loading }
}
```

### What NOT to do

```ts
// ❌ Over-encapsulation for simple toggle
export function useToggle(initial = false) {
  const state = ref(initial)
  const toggle = () => { state.value = !state.value }
  const setTrue = () => { state.value = true }
  const setFalse = () => { state.value = false }
  return { state, toggle, setTrue, setFalse }
}

// ✅ Just write it in the component
const isOpen = ref(false)
```

```ts
// ❌ Unnecessary service layer
class UserService {
  static async getUser(id: number) { ... }
}

// ✅ Direct call or thin API module
export const getUser = (id: number) => request.get(`/users/${id}`)
```

## Style Rules

- Prefer `const` over `let`.
- Prefer template literals and optional chaining.
- Keep template logic light; move complex conditions to computed.
- Comments only for non-obvious intent, not for what the code already says.
- Consistent 2-space indent, single quotes or project convention.

## Output Expectations

When generating or refactoring Vue 3 code:

1. Start from the simplest working version.
2. Only introduce abstraction after the simple version is clear.
3. Prefer fewer files and fewer layers.
4. Make the code look like a competent human wrote it for a real product, not an architecture textbook.
