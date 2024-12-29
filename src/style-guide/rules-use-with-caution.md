# অগ্রাধিকার D নিয়ম: সতর্কতার সাথে ব্যবহার করুন {#priority-d-rules-use-with-caution}

::: warning Note
এই Vue.js স্টাইল গাইড পুরানো এবং পর্যালোচনা করা প্রয়োজন৷ আপনার কোন প্রশ্ন বা পরামর্শ থাকলে, অনুগ্রহ করে [একটি সমস্যা খুলুন](https://github.com/vuejs/docs/issues/new)।
:::

বিরল এজ কেস বা লিগ্যাসি কোড বেস থেকে মসৃণ মাইগ্রেশন মিটমাট করার জন্য Vue-এর কিছু বৈশিষ্ট্য বিদ্যমান। যদিও অত্যধিক ব্যবহার করা হয়, তারা আপনার কোড বজায় রাখা আরও কঠিন করে তুলতে পারে বা এমনকি বাগগুলির উৎস হয়ে উঠতে পারে। এই নিয়মগুলি কখন এবং কেন এড়ানো উচিত তা বর্ণনা করে সম্ভাব্য ঝুঁকিপূর্ণ বৈশিষ্ট্যগুলির উপর আলোকপাত করে৷

## `scoped` কম্পোনেন্ট নির্বাচক {#element-selectors-with-scoped}

**এলিমেন্ট নির্বাচকদের `scoped` এড়ানো উচিত।**

`scoped` কম্পোনেন্ট নির্বাচকদের চেয়ে শ্রেণী নির্বাচকদের পছন্দ করুন, কারণ বড় সংখ্যক কম্পোনেন্ট নির্বাচক ধীর।

::: details বিস্তারিত ব্যাখ্যা
স্কোপ স্টাইলতে, Vue কম্পোনেন্ট কম্পোনেন্টগুলিতে একটি অনন্য বৈশিষ্ট্য যোগ করে, যেমন `data-v-f3f3eg9`। তারপরে নির্বাচকদের সংশোধন করা হয় যাতে শুধুমাত্র এই বৈশিষ্ট্যের সাথে মিলে যাওয়া কম্পোনেন্টগুলি নির্বাচন করা হয় (যেমন `button[data-v-f3f3eg9]`)।

সমস্যা হল যে বড় সংখ্যক কম্পোনেন্ট-অ্যাট্রিবিউট নির্বাচক (যেমন `button[data-v-f3f3eg9]`) ক্লাস-অ্যাট্রিবিউট নির্বাচকদের তুলনায় যথেষ্ট ধীর হবে (যেমন `.btn-close[data-v-f3f3eg9]`), তাই যখনই সম্ভব ক্লাস নির্বাচকদের পছন্দ করা উচিত।
:::

<div class="style-example style-example-bad">
<h3>Bad</h3>

```vue-html
<template>
  <button>×</button>
</template>

<style scoped>
button {
  background-color: red;
}
</style>
```

</div>

<div class="style-example style-example-good">
<h3>Good</h3>

```vue-html
<template>
  <button class="btn btn-close">×</button>
</template>

<style scoped>
.btn-close {
  background-color: red;
}
</style>
```

</div>

## অন্তর্নিহিত parent-child যোগাযোগ {#implicit-parent-child-communication}

**parent-child কম্পোনেন্ট কমিউনিকেশনের জন্য প্রপস এবং ইভেন্টগুলিকে পছন্দ করা উচিত, এর পরিবর্তে `this.$parent` বা mutating props.**

একটি আদর্শ Vue অ্যাপ্লিকেশন প্রপস ডাউন, ইভেন্ট আপ। এই কনভেনশনে লেগে থাকা আপনার কম্পোনেন্টগুলিকে বুঝতে অনেক সহজ করে তোলে। যাইহোক, এমন কিছু এজ কেস রয়েছে যেখানে প্রপ মিউটেশন বা `this.$parent` দুটি কম্পোনেন্টকে সরল করতে পারে যেগুলি ইতিমধ্যেই গভীরভাবে মিলিত হয়েছে।

সমস্যা হল, এমন অনেক _সাধারণ_ ক্ষেত্রেও রয়েছে যেখানে এই নিদর্শনগুলি সুবিধা দিতে পারে। সতর্ক থাকুন: স্বল্পমেয়াদী সুবিধার জন্য (কম কোড লেখা) ট্রেডিং সরলতার (আপনার রাজ্যের প্রবাহ বুঝতে সক্ষম হওয়া) প্রলুব্ধ করবেন না।

<div class="options-api">

<div class="style-example style-example-bad">
<h3>Bad</h3>

```js
app.component('TodoItem', {
  props: {
    todo: {
      type: Object,
      required: true
    }
  },

  template: '<input v-model="todo.text">'
})
```

```js
app.component('TodoItem', {
  props: {
    todo: {
      type: Object,
      required: true
    }
  },

  methods: {
    removeTodo() {
      this.$parent.todos = this.$parent.todos.filter(
        (todo) => todo.id !== vm.todo.id
      )
    }
  },

  template: `
    <span>
      {{ todo.text }}
      <button @click="removeTodo">
        ×
      </button>
    </span>
  `
})
```

</div>

<div class="style-example style-example-good">
<h3>Good</h3>

```js
app.component('TodoItem', {
  props: {
    todo: {
      type: Object,
      required: true
    }
  },

  emits: ['input'],

  template: `
    <input
      :value="todo.text"
      @input="$emit('input', $event.target.value)"
    >
  `
})
```

```js
app.component('TodoItem', {
  props: {
    todo: {
      type: Object,
      required: true
    }
  },

  emits: ['delete'],

  template: `
    <span>
      {{ todo.text }}
      <button @click="$emit('delete')">
        ×
      </button>
    </span>
  `
})
```

</div>

</div>

<div class="composition-api">

<div class="style-example style-example-bad">
<h3>Bad</h3>

```vue
<script setup>
defineProps({
  todo: {
    type: Object,
    required: true
  }
})
</script>

<template>
  <input v-model="todo.text" />
</template>
```

```vue
<script setup>
import { getCurrentInstance } from 'vue'

const props = defineProps({
  todo: {
    type: Object,
    required: true
  }
})

const instance = getCurrentInstance()

function removeTodo() {
  const parent = instance.parent
  if (!parent) return

  parent.props.todos = parent.props.todos.filter((todo) => {
    return todo.id !== props.todo.id
  })
}
</script>

<template>
  <span>
    {{ todo.text }}
    <button @click="removeTodo">×</button>
  </span>
</template>
```

</div>

<div class="style-example style-example-good">
<h3>Good</h3>

```vue
<script setup>
defineProps({
  todo: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['input'])
</script>

<template>
  <input :value="todo.text" @input="emit('input', $event.target.value)" />
</template>
```

```vue
<script setup>
defineProps({
  todo: {
    type: Object,
    required: true
  }
})

const emit = defineEmits(['delete'])
</script>

<template>
  <span>
    {{ todo.text }}
    <button @click="emit('delete')">×</button>
  </span>
</template>
```

</div>

</div>
