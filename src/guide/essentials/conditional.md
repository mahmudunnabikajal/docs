# Conditional Rendering {#conditional-rendering}

<div class="options-api">
  <VueSchoolLink href="https://vueschool.io/lessons/conditional-rendering-in-vue-3" title="বিনামূল্যে Vue.js Conditional Rendering পাঠ"/>
</div>

<div class="composition-api">
  <VueSchoolLink href="https://vueschool.io/lessons/vue-fundamentals-capi-conditionals-in-vue" title="বিনামূল্যে Vue.js Conditional Rendering পাঠ"/>
</div>

<script setup>
import { ref } from 'vue'
const awesome = ref(true)
</script>

## `v-if` {#v-if}

নির্দেশিকা `v-if` শর্তসাপেক্ষে একটি ব্লক রেন্ডার করতে ব্যবহৃত হয়। নির্দেশের অভিব্যক্তি একটি সত্য মান প্রদান করলেই ব্লকটি রেন্ডার করা হবে।

```vue-html
<h1 v-if="awesome">Vue is awesome!</h1>
```

## `v-else` {#v-else}

আপনি `v-if` এর জন্য একটি "অন্য ব্লক" নির্দেশ করতে `v-else` নির্দেশিকা ব্যবহার করতে পারেন:

```vue-html
<button @click="awesome = !awesome">Toggle</button>

<h1 v-if="awesome">Vue is awesome!</h1>
<h1 v-else>Oh no 😢</h1>
```

<div class="demo">
  <button @click="awesome = !awesome">Toggle</button>
  <h1 v-if="awesome">Vue is awesome!</h1>
  <h1 v-else>Oh no 😢</h1>
</div>

<div class="composition-api">

[চেষ্টা করুন](https://play.vuejs.org/#eNpFjkEOgjAQRa8ydIMulLA1hegJ3LnqBskAjdA27RQXhHu4M/GEHsEiKLv5mfdf/sBOxux7j+zAuCutNAQOyZtcKNkZbQkGsFjBCJXVHcQBjYUSqtTKERR3dLpDyCZmQ9bjViiezKKgCIGwM21BGBIAv3oireBYtrK8ZYKtgmg5BctJ13WLPJnhr0YQb1Lod7JaS4G8eATpfjMinjTphC8wtg7zcwNKw/v5eC1fnvwnsfEDwaha7w==)

</div>
<div class="options-api">

[চেষ্টা করুন](https://play.vuejs.org/#eNpFjj0OwjAMha9iMsEAFWuVVnACNqYsoXV/RJpEqVOQqt6DDYkTcgRSWoplWX7y56fXs6O1u84jixlvM1dbSoXGuzWOIMdCekXQCw2QS5LrzbQLckje6VEJglDyhq1pMAZyHidkGG9hhObRYh0EYWOVJAwKgF88kdFwyFSdXRPBZidIYDWvgqVkylIhjyb4ayOIV3votnXxfwrk2SPU7S/PikfVfsRnGFWL6akCbeD9fLzmK4+WSGz4AA5dYQY=)

</div>

একটি `v-else` কম্পোনেন্ট অবিলম্বে একটি `v-if` বা `v-else-if` কম্পোনেন্ট অনুসরণ করতে হবে - অন্যথায় এটি স্বীকৃত হবে না।

## `v-else-if` {#v-else-if}

`v-else-if`, নাম অনুসারে, `v-if`-এর জন্য একটি "else if block" হিসেবে কাজ করে। এটি একাধিকবার শৃঙ্খলিত হতে পারে:

```vue-html
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```

`v-else` এর মতো, একটি `v-else-if` কম্পোনেন্ট অবিলম্বে একটি `v-if` বা `v-else-if` কম্পোনেন্ট অনুসরণ করতে হবে।

## `v-if` on `<template>` {#v-if-on-template}

কারণ `v-if` একটি নির্দেশিকা, এটি একটি একক কম্পোনেন্টের সাথে সংযুক্ত করতে হবে। কিন্তু যদি আমরা একাধিক কম্পোনেন্ট টগল করতে চাই? এই ক্ষেত্রে আমরা একটি `<template>` কম্পোনেন্টে `v-if` ব্যবহার করতে পারি, যা একটি অদৃশ্য মোড়ক হিসেবে কাজ করে। চূড়ান্ত রেন্ডার করা ফলাফলে `<template>` কম্পোনেন্ট অন্তর্ভুক্ত হবে না।

```vue-html
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

`v-else` এবং `v-else-if` এছাড়াও `<template>` এ ব্যবহার করা যেতে পারে।

## `v-show` {#v-show}

শর্তসাপেক্ষে একটি কম্পোনেন্ট প্রদর্শনের জন্য আরেকটি বিকল্প হল `v-show` নির্দেশিকা। ব্যবহার মূলত একই:

```vue-html
<h1 v-show="ok">Hello!</h1>
```

পার্থক্য হল `v-show` সহ একটি কম্পোনেন্ট সর্বদা রেন্ডার করা হবে এবং DOM-এ থাকবে; `v-show` শুধুমাত্র এলিমেন্টের `display` CSS প্রপার্টি টগল করে।

`v-show` `<template>` কম্পোনেন্টকে সমর্থন করে না, বা এটি `v-else`-এর সাথে কাজ করে না।

## `v-if` vs. `v-show` {#v-if-vs-v-show}

`v-if` হল "বাস্তব" শর্তসাপেক্ষ রেন্ডারিং কারণ এটি নিশ্চিত করে যে কন্ডিশনাল ব্লকের মধ্যে ইভেন্ট শ্রোতা এবং child কম্পোনেন্টগুলি সঠিকভাবে ধ্বংস করা হয়েছে এবং টগল করার সময় পুনরায় তৈরি করা হয়েছে৷

`v-if`ও **অলস**: যদি শর্তটি প্রাথমিক রেন্ডারে মিথ্যা হয়, তবে এটি কিছুই করবে না - শর্তটি প্রথমবার সত্য না হওয়া পর্যন্ত শর্তযুক্ত ব্লক রেন্ডার করা হবে না।

তুলনামূলকভাবে, `v-show` অনেক সহজ - CSS-ভিত্তিক টগলিংয়ের সাথে প্রাথমিক অবস্থা নির্বিশেষে কম্পোনেন্টটি সর্বদা রেন্ডার করা হয়।

সাধারণভাবে বলতে গেলে, `v-if`-এর টগল ব্যয় বেশি থাকে যখন `v-show`-এর প্রাথমিক রেন্ডার ব্যয় বেশি থাকে। তাই আপনি যদি প্রায়শই কিছু টগল করতে চান তাহলে `v-show` পছন্দ করুন এবং রানটাইমে অবস্থার পরিবর্তনের সম্ভাবনা না থাকলে `v-if` পছন্দ করুন।

## `v-if` with `v-for` {#v-if-with-v-for}

::: warning Note
অন্তর্নিহিত অগ্রাধিকারের কারণে একই এলিমেন্টে `v-if` এবং `v-for` ব্যবহার করার পরামর্শ **নয়**। বিস্তারিত জানার জন্য [স্টাইল গাইড](/style-guide/rules-essential#avoid-v-if-with-v-for) পড়ুন।
:::

যখন `v-if` এবং `v-for` উভয়ই একই কম্পোনেন্টে ব্যবহৃত হয়, `v-if` প্রথমে মূল্যায়ন করা হবে। বিস্তারিত জানার জন্য [list rendering guide](list#v-for-with-v-if) দেখুন।
