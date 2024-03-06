---
outline: deep
---

# Using Vue with TypeScript {#using-vue-with-typescript}

টাইপস্ক্রিপ্টের মতো একটি টাইপ সিস্টেম বিল্ড টাইমে স্ট্যাটিক বিশ্লেষণের মাধ্যমে অনেক সাধারণ ত্রুটি সনাক্ত করতে পারে। এটি উত্পাদনে রানটাইম ত্রুটির সম্ভাবনাকে হ্রাস করে, এবং আমাদেরকে বড় আকারের অ্যাপ্লিকেশনগুলিতে আরও আত্মবিশ্বাসের সাথে রিফ্যাক্টর কোডের অনুমতি দেয়। টাইপস্ক্রিপ্ট IDE-তে টাইপ-ভিত্তিক স্বয়ংক্রিয়-সম্পূর্ণতার মাধ্যমে বিকাশকারীর এর্গোনমিক্সকেও উন্নত করে।

Vue টাইপস্ক্রিপ্টে লেখা হয় এবং প্রথম শ্রেণীর টাইপস্ক্রিপ্ট সমর্থন প্রদান করে। সমস্ত অফিসিয়াল Vue প্যাকেজ বান্ডিল টাইপ ডিক্লেয়ারেশনের সাথে আসে যা বাক্সের বাইরে কাজ করা উচিত।

## Project Setup {#project-setup}

[`create-vue`](https://github.com/vuejs/create-vue), অফিসিয়াল প্রজেক্ট স্ক্যাফোল্ডিং টুল, একটি [Vite](https://vitejs.dev/)-চালিত স্ক্যাফোল্ড করার বিকল্পগুলি অফার করে , TypeScript-প্রস্তুত Vue প্রকল্প।

### Overview {#overview}

একটি Vite-ভিত্তিক সেটআপের সাথে, dev সার্ভার এবং বান্ডলার শুধুমাত্র ট্রান্সপিলেশন হয় এবং কোনো প্রকার-চেকিং করে না। এটি নিশ্চিত করে যে Vite dev সার্ভারটি টাইপস্ক্রিপ্ট ব্যবহার করার সময়ও দ্রুত জ্বলন্ত থাকে।

- বিকাশের সময়, আমরা টাইপ ত্রুটির বিষয়ে তাত্ক্ষণিক প্রতিক্রিয়ার জন্য একটি ভাল [IDE সেটআপ](#ide-support) এর উপর নির্ভর করার পরামর্শ দিই।

- SFC ব্যবহার করলে, কমান্ড লাইন টাইপ চেকিং এবং টাইপ ডিক্লেয়ারেশন জেনারেশনের জন্য [`vue-tsc`](https://github.com/vuejs/language-tools/tree/master/packages/tsc) ইউটিলিটি ব্যবহার করুন। `vue-tsc` হল `tsc` এর চারপাশে একটি মোড়ক, টাইপস্ক্রিপ্টের নিজস্ব কমান্ড লাইন ইন্টারফেস। এটি টাইপস্ক্রিপ্ট ফাইলগুলি ছাড়াও Vue SFC সমর্থন করে তা ছাড়া এটি মূলত `tsc` এর মতোই কাজ করে। আপনি Vite dev সার্ভারের সমান্তরালে ওয়াচ মোডে `vue-tsc` চালাতে পারেন অথবা [vite-plugin-checker](https://vite-plugin-checker.netlify.app/) এর মতো একটি Vite প্লাগইন ব্যবহার করতে পারেন যা চলে একটি পৃথক কর্মী থ্রেড চেক.

- Vue CLI এছাড়াও TypeScript সমর্থন প্রদান করে, কিন্তু আর সুপারিশ করা হয় না। দেখুন [নীচের নোট](#note-on-vue-cli-and-ts-loader).

### IDE Support {#ide-support}

- [ভিজ্যুয়াল স্টুডিও কোড](https://code.visualstudio.com/) (VSCode) এর TypeScript-এর জন্য দুর্দান্ত আউট-অফ-দ্য-বক্স সমর্থনের জন্য দৃঢ়ভাবে সুপারিশ করা হয়।

- [Vue - অফিসিয়াল](https://marketplace.visualstudio.com/items?itemName=Vue.volar) (আগে Volar) হল অফিসিয়াল VSCode এক্সটেনশন যা Vue SFC-এর মধ্যে TypeScript সমর্থন প্রদান করে, সাথে আরও অনেক দুর্দান্ত বৈশিষ্ট্য রয়েছে।

     :::tip
     Vue - অফিসিয়াল এক্সটেনশন [Vetur](https://marketplace.visualstudio.com/items?itemName=octref.vetur) প্রতিস্থাপন করে, Vue 2-এর জন্য আমাদের পূর্ববর্তী অফিসিয়াল VSCode এক্সটেনশন। আপনার যদি বর্তমানে Vetur ইনস্টল করা থাকে, তাহলে এটিকে অক্ষম করতে ভুলবেন না। Vue 3 প্রকল্প।
     :::

- [ওয়েবস্টর্ম](https://www.jetbrains.com/webstorm/) এছাড়াও TypeScript এবং Vue উভয়ের জন্য আউট-অফ-দ্য-বক্স সমর্থন প্রদান করে। অন্যান্য JetBrains IDE গুলিও তাদের সমর্থন করে, হয় বাক্সের বাইরে বা [একটি বিনামূল্যের প্লাগইন](https://plugins.jetbrains.com/plugin/9442-vue-js) এর মাধ্যমে। 2023.2 সংস্করণ অনুসারে, WebStorm এবং Vue প্লাগইন Vue ভাষা সার্ভারের জন্য অন্তর্নির্মিত সমর্থন সহ আসে। আপনি সেটিংস > ভাষা এবং ফ্রেমওয়ার্ক > টাইপস্ক্রিপ্ট > Vue-এর অধীনে সমস্ত TypeScript সংস্করণে Volar ইন্টিগ্রেশন ব্যবহার করার জন্য Vue পরিষেবা সেট করতে পারেন। ডিফল্টরূপে, Volar টাইপস্ক্রিপ্ট সংস্করণ 5.0 এবং উচ্চতর জন্য ব্যবহার করা হবে।

### Configuring `tsconfig.json` {#configuring-tsconfig-json}

`create-vue` এর মাধ্যমে স্ক্যাফোল্ড করা প্রজেক্টের মধ্যে পূর্ব-কনফিগার করা `tsconfig.json` অন্তর্ভুক্ত থাকে। বেস কনফিগারেশনটি [`@vue/tsconfig`](https://github.com/vuejs/tsconfig) প্যাকেজে বিমূর্ত করা হয়েছে। প্রজেক্টের ভিতরে, আমরা [প্রকল্প রেফারেন্স](https://www.typescriptlang.org/docs/handbook/project-references.html) ব্যবহার করি যাতে বিভিন্ন পরিবেশে কোড চলার জন্য সঠিক প্রকার নিশ্চিত করা যায় (যেমন অ্যাপ কোড এবং টেস্ট কোড থাকা উচিত বিভিন্ন গ্লোবাল ভেরিয়েবল)।

ম্যানুয়ালি `tsconfig.json` কনফিগার করার সময়, কিছু উল্লেখযোগ্য বিকল্পের মধ্যে রয়েছে:

- [`compilerOptions.isolatedModules`](https://www.typescriptlang.org/tsconfig#isolatedModules) `true`-তে সেট করা আছে কারণ Vite TypeScript ট্রান্সপিলিং করার জন্য [esbuild](https://esbuild.github.io/) ব্যবহার করে এবং একক-ফাইল ট্রান্সপিল সীমাবদ্ধতার বিষয়। [`compilerOptions.verbatimModuleSyntax`](https://www.typescriptlang.org/tsconfig#verbatimModuleSyntax) হল [`isolatedModules`-এর একটি সুপারসেট](https://github.com/microsoft/TypeScript/issues/53601) এবং একটি ভাল পছন্দও - এটি [`@vue/tsconfig`](https://github.com/vuejs/tsconfig) ব্যবহার করে।

- আপনি যদি Options API ব্যবহার করেন, তাহলে আপনাকে [`compilerOptions.strict`](https://www.typescriptlang.org/tsconfig#strict) সেট করতে হবে `true` (অথবা অন্তত [`compilerOptions.noImplicitThis` সক্ষম করুন। ](https://www.typescriptlang.org/tsconfig#noImplicitThis), যা `strict` ফ্ল্যাগের একটি অংশ) কম্পোনেন্ট বিকল্পে `this`-এর টাইপ চেকিংয়ের সুবিধা নিতে। অন্যথায় `this`টিকে `any` হিসাবে গণ্য করা হবে।

- আপনি যদি আপনার বিল্ড টুলে সমাধানকারী উপনামগুলি কনফিগার করে থাকেন, উদাহরণস্বরূপ `@/*` উপনাম একটি `create-vue` প্রজেক্টে ডিফল্টরূপে কনফিগার করা থাকে, তাহলে আপনাকে [`compilerOptions.paths`](এর মাধ্যমে TypeScript-এর জন্যও কনফিগার করতে হবে https://www.typescriptlang.org/tsconfig#paths)।

- আপনি যদি Vue-এর সাথে TSX ব্যবহার করতে চান, তাহলে [`compilerOptions.jsx`](https://www.typescriptlang.org/tsconfig#jsx) সেট করুন `"সংরক্ষণ করুন"`, এবং সেট করুন [`compilerOptions.jsxImportSource`]( https://www.typescriptlang.org/tsconfig#jsxImportSource) থেকে `"vue"`।

See also:

- [অফিসিয়াল টাইপস্ক্রিপ্ট কম্পাইলার অপশন ডক্স](https://www.typescriptlang.org/docs/handbook/compiler-options.html)
- [এসবিল্ড টাইপস্ক্রিপ্ট সংকলন সতর্কতা](https://esbuild.github.io/content-types/#typescript-caveats)

### Note on Vue CLI and `ts-loader` {#note-on-vue-cli-and-ts-loader}

Vue CLI-এর মতো ওয়েবপ্যাক-ভিত্তিক সেটআপগুলিতে, মডিউল ট্রান্সফর্ম পাইপলাইনের অংশ হিসাবে টাইপ চেকিং করা সাধারণ, উদাহরণস্বরূপ `ts-loader` এর সাথে। যাইহোক, এটি একটি পরিষ্কার সমাধান নয় কারণ টাইপ সিস্টেমের টাইপ চেক করার জন্য সম্পূর্ণ মডিউল গ্রাফের জ্ঞান প্রয়োজন। স্বতন্ত্র মডিউলের রূপান্তর পদক্ষেপটি কাজের জন্য সঠিক জায়গা নয়। এটি নিম্নলিখিত সমস্যার দিকে পরিচালিত করে:

- `ts-loader` শুধুমাত্র চেক পোস্ট-ট্রান্সফর্ম কোড টাইপ করতে পারে। এটি আইডিই বা `vue-tsc` থেকে আমরা যে ত্রুটিগুলি দেখি তার সাথে সারিবদ্ধ নয়, যা সরাসরি উৎস কোডে ফিরে আসে।

- টাইপ চেকিং ধীর হতে পারে। যখন এটি একই থ্রেড/প্রসেসে কোড ট্রান্সফরমেশনের সাথে সঞ্চালিত হয়, তখন এটি সমগ্র অ্যাপ্লিকেশনের বিল্ড গতিকে উল্লেখযোগ্যভাবে প্রভাবিত করে।

- আমাদের ইতিমধ্যেই আলাদা প্রক্রিয়ায় আমাদের IDE-তে টাইপ চেকিং চলছে, তাই ডেভ এক্সপেরিয়েন্সের খরচ মন্থর হয়ে যাওয়াটা ভালো ট্রেড-অফ নয়।

আপনি যদি বর্তমানে Vue CLI এর মাধ্যমে Vue 3 + TypeScript ব্যবহার করছেন, আমরা দৃঢ়ভাবে Vite-এ স্থানান্তরিত করার পরামর্শ দিই। আমরা ট্রান্সপিল-শুধু TS সমর্থন সক্ষম করার জন্য CLI বিকল্পগুলিতেও কাজ করছি, যাতে আপনি টাইপ চেকিংয়ের জন্য `vue-tsc`-এ স্যুইচ করতে পারেন।

## General Usage Notes {#general-usage-notes}

### `defineComponent()` {#definecomponent}

টাইপস্ক্রিপ্ট সঠিকভাবে কম্পোনেন্ট অপশনের অভ্যন্তরে প্রকারগুলি অনুমান করতে দিতে, আমাদের এর সাথে কম্পোনেন্টগুলি সংজ্ঞায়িত করতে হবে[`defineComponent()`](/api/general#definecomponent):

```ts
import { defineComponent } from 'vue'

export default defineComponent({
  // type inference enabled
  props: {
    name: String,
    msg: { type: String, required: true }
  },
  data() {
    return {
      count: 1
    }
  },
  mounted() {
    this.name // type: string | undefined
    this.msg // type: string
    this.count // type: number
  }
})
```

`<script setup>` ছাড়া Composition API ব্যবহার করার সময় `defineComponent()` `setup()`-এ পাস করা প্রপস অনুমান করাও সমর্থন করে:

```ts
import { defineComponent } from 'vue'

export default defineComponent({
  // type inference enabled
  props: {
    message: String
  },
  setup(props) {
    props.message // type: string | undefined
  }
})
```

আরো দেখুন:

- [ওয়েবপ্যাক ট্রিশেকিং-এ নোট](/api/general#note-on-webpack-treeshaking)
- [`defineComponent` এর জন্য টাইপ টেস্ট](https://github.com/vuejs/core/blob/main/packages/dts-test/defineComponent.test-d.tsx)

:::tip
`defineComponent()` প্লেইন জাভাস্ক্রিপ্টে সংজ্ঞায়িত কম্পোনেন্টগুলির জন্য টাইপ ইনফারেন্স সক্ষম করে।
:::

### Usage in Single-File Components {#usage-in-single-file-components}

SFC-তে TypeScript ব্যবহার করতে, `<script>` ট্যাগে `lang="ts"` অ্যাট্রিবিউট যোগ করুন। যখন `lang="ts"` উপস্থিত থাকে, তখন সমস্ত টেমপ্লেট এক্সপ্রেশন কঠোর টাইপ চেকিং উপভোগ করে।

```vue
<script lang="ts">
import { defineComponent } from 'vue'

export default defineComponent({
  data() {
    return {
      count: 1
    }
  }
})
</script>

<template>
  <!-- type checking and auto-completion enabled -->
  {{ count.toFixed(2) }}
</template>
```

`lang="ts"` can also be used with `<script setup>`:

```vue
<script setup lang="ts">
// TypeScript enabled
import { ref } from 'vue'

const count = ref(1)
</script>

<template>
  <!-- type checking and auto-completion enabled -->
  {{ count.toFixed(2) }}
</template>
```

### TypeScript in Templates {#typescript-in-templates}

যখন `<script lang="ts">` বা `<script setup lang="ts">` ব্যবহার করা হয় তখন `<template>` টাইপস্ক্রিপ্টকে বাইন্ডিং এক্সপ্রেশনে সমর্থন করে। এটি এমন ক্ষেত্রে দরকারী যেখানে আপনাকে টেমপ্লেট এক্সপ্রেশনে টাইপ কাস্টিং করতে হবে।

এখানে একটি কল্পিত উদাহরণ:

```vue
<script setup lang="ts">
let x: string | number = 1
</script>

<template>
  <!-- error because x could be a string -->
  {{ x.toFixed(2) }}
</template>
```

এটি একটি ইনলাইন টাইপ কাস্ট দিয়ে কাজ করা যেতে পারে:

```vue{6}
<script setup lang="ts">
let x: string | number = 1
</script>

<template>
  {{ (x as number).toFixed(2) }}
</template>
```

:::tip
Vue CLI বা একটি ওয়েবপ্যাক-ভিত্তিক সেটআপ ব্যবহার করলে, টেমপ্লেট এক্সপ্রেশনে TypeScript-এর প্রয়োজন `vue-loader@^16.8.0`।
:::

### Usage with TSX {#usage-with-tsx}

Vue also supports authoring components with JSX / TSX. Details are covered in the [Render Function & JSX](/guide/extras/render-function.html#jsx-tsx) guide.

## Generic Components {#generic-components}

Generic components are supported in two cases:

- In SFCs: [`<script setup>` with the `generic` attribute](/api/sfc-script-setup.html#generics)
- Render function / JSX components: [`defineComponent()`'s function signature](/api/general.html#function-signature)

## API-Specific Recipes {#api-specific-recipes}

- [TS with Composition API](./composition-api)
- [TS with Options API](./options-api)
