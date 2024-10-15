# Async Components {#async-components}

## Basic Usage {#basic-usage}

বড় অ্যাপ্লিকেশানগুলিতে, আমাদের অ্যাপটিকে ছোট ছোট খণ্ডে ভাগ করতে হবে এবং প্রয়োজন হলেই সার্ভার থেকে একটি কম্পোনেন্ট লোড করতে হবে। এটি সম্ভব করার জন্য, Vue এর একটি [`defineAsyncComponent`](/api/general#defineasynccomponent) ফাংশন রয়েছে:

```js
import { defineAsyncComponent } from 'vue'

const AsyncComp = defineAsyncComponent(() => {
  return new Promise((resolve, reject) => {
    // ...load component from server
    resolve(/* loaded component */)
  })
})
// ... use `AsyncComp` like a normal component
```

আপনি দেখতে পাচ্ছেন, `defineAsyncComponent` একটি লোডার ফাংশন গ্রহণ করে যা একটি প্রতিশ্রুতি প্রদান করে। যখন আপনি সার্ভার থেকে আপনার কম্পোনেন্ট সংজ্ঞা পুনরুদ্ধার করেছেন তখন প্রতিশ্রুতির `resolve` কলব্যাক কল করা উচিত। আপনি লোড ব্যর্থ হয়েছে নির্দেশ করতে `reject(reason)` কল করতে পারেন।

[ES মডিউল ডাইনামিক ইম্পোর্ট](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/import) এছাড়াও একটি প্রতিশ্রুতি প্রদান করে, তাই বেশিরভাগ সময় আমরা এটির সাথে একত্রে ব্যবহার করব `fineAsyncComponent`. Vite এবং ওয়েবপ্যাকের মতো বান্ডলারগুলিও সিনট্যাক্স সমর্থন করে (এবং এটিকে বান্ডেল স্প্লিট পয়েন্ট হিসাবে ব্যবহার করবে), তাই আমরা এটি Vue SFC আমদানি করতে ব্যবহার করতে পারি:

```js
import { defineAsyncComponent } from 'vue'

const AsyncComp = defineAsyncComponent(() =>
  import('./components/MyComponent.vue')
)
```

ফলস্বরূপ `AsyncComp` হল একটি র‍্যাপার কম্পোনেন্ট যা শুধুমাত্র লোডার ফাংশনকে কল করে যখন এটি আসলে পৃষ্ঠায় রেন্ডার করা হয়। উপরন্তু, এটি যেকোনো প্রপস এবং স্লট বরাবর অভ্যন্তরীণ কম্পোনেন্টে চলে যাবে, তাই আপনি অলস লোডিং অর্জনের সময় মূল কম্পোনেন্টটিকে নির্বিঘ্নে প্রতিস্থাপন করতে অ্যাসিঙ্ক র‍্যাপার ব্যবহার করতে পারেন।

সাধারণ কম্পোনেন্টগুলির মতো, async কম্পোনেন্টগুলি `app.component()` ব্যবহার করে [registered globally](/guide/components/registration#global-registration) হতে পারে:

```js
app.component(
  'MyComponent',
  defineAsyncComponent(() => import('./components/MyComponent.vue'))
)
```

<div class="options-api">

[স্থানীয়ভাবে একটি কম্পোনেন্ট নিবন্ধন করার সময়](/guide/components/registration#local-registration): আপনি `defineAsyncComponent` ব্যবহার করতে পারেন:

```vue
<script>
import { defineAsyncComponent } from 'vue'

export default {
  components: {
    AdminPage: defineAsyncComponent(() =>
      import('./components/AdminPageComponent.vue')
    )
  }
}
</script>

<template>
  <AdminPage />
</template>
```

</div>

<div class="composition-api">

এগুলি সরাসরি তাদের মূল কম্পোনেন্টের ভিতরেও সংজ্ঞায়িত করা যেতে পারে:

```vue
<script setup>
import { defineAsyncComponent } from 'vue'

const AdminPage = defineAsyncComponent(() =>
  import('./components/AdminPageComponent.vue')
)
</script>

<template>
  <AdminPage />
</template>
```

</div>

## Loading and Error States {#loading-and-error-states}

অ্যাসিঙ্ক্রোনাস অপারেশনে অনিবার্যভাবে লোডিং এবং ত্রুটির অবস্থা জড়িত - `defineAsyncComponent()` উন্নত বিকল্পগুলির মাধ্যমে এই অবস্থাগুলি পরিচালনা করতে সমর্থন করে:

```js
const AsyncComp = defineAsyncComponent({
  // the loader function
  loader: () => import('./Foo.vue'),

  // A component to use while the async component is loading
  loadingComponent: LoadingComponent,
  // Delay before showing the loading component. Default: 200ms.
  delay: 200,

  // A component to use if the load fails
  errorComponent: ErrorComponent,
  // The error component will be displayed if a timeout is
  // provided and exceeded. Default: Infinity.
  timeout: 3000
})
```

যদি একটি লোডিং কম্পোনেন্ট প্রদান করা হয়, এটি প্রথমে প্রদর্শিত হবে যখন ভিতরের কম্পোনেন্টটি লোড হচ্ছে৷ লোডিং কম্পোনেন্ট দেখানোর আগে একটি ডিফল্ট 200ms বিলম্ব আছে - এর কারণ হল দ্রুত নেটওয়ার্কগুলিতে, একটি তাত্ক্ষণিক লোডিং অবস্থা খুব দ্রুত প্রতিস্থাপিত হতে পারে এবং এটি একটি ঝাঁকুনির মতো দেখায়।

যদি একটি ত্রুটি কম্পোনেন্ট প্রদান করা হয়, এটি প্রদর্শিত হবে যখন লোডার ফাংশন দ্বারা প্রত্যাবর্তিত প্রতিশ্রুতি প্রত্যাখ্যান করা হয়। রিকোয়েস্ট টি খুব বেশি সময় নিলে আপনি ত্রুটি কম্পোনেন্টটি দেখানোর জন্য একটি সময়সীমা নির্দিষ্ট করতে পারেন।

## Lazy Hydration <sup class="vt-badge" data-text="3.5+" /> {#lazy-hydration}

> এই বিভাগটি শুধুমাত্র তখনই প্রযোজ্য যখন আপনি [সার্ভার-সাইড রেন্ডারিং](/guide/scaling-up/ssr) ব্যবহার করছেন।

Vue 3.5+ এ, অ্যাসিঙ্ক কম্পোনেন্টগুলি হাইড্রেশন কৌশল প্রদান করে হাইড্রেটেড হলে নিয়ন্ত্রণ করতে পারে।

- Vue বিল্ট-ইন হাইড্রেশন কৌশলগুলির একটি সংখ্যা প্রদান করে। এই অন্তর্নির্মিত কৌশলগুলি পৃথকভাবে আমদানি করা প্রয়োজন যাতে ব্যবহার না করা হলে সেগুলি গাছ-কাঁপানো যেতে পারে।

- নকশাটি ইচ্ছাকৃতভাবে ফ্লেক্সিবলর জন্য নিম্ন-স্তরের। কম্পাইলার সিনট্যাক্স সুগার সম্ভাব্যভাবে এটির উপরে ভবিষ্যতে মূল বা উচ্চ স্তরের সমাধানগুলিতে তৈরি করা যেতে পারে (যেমন Nuxt)।

### Hydrate on Idle {#hydrate-on-idle}

Hydrates via `requestIdleCallback`:

```js
import { defineAsyncComponent, hydrateOnIdle } from 'vue'

const AsyncComp = defineAsyncComponent({
  loader: () => import('./Comp.vue'),
  hydrate: hydrateOnIdle(/* optionally pass a max timeout */)
})
```

### Hydrate on Visible {#hydrate-on-visible}

যখন কম্পোনেন্ট(গুলি) `IntersectionObserver` এর মাধ্যমে দৃশ্যমান হয় তখন হাইড্রেট করুন৷

```js
import { defineAsyncComponent, hydrateOnVisible } from 'vue'

const AsyncComp = defineAsyncComponent({
  loader: () => import('./Comp.vue'),
  hydrate: hydrateOnVisible()
})
```

পর্যবেক্ষকের জন্য ঐচ্ছিকভাবে একটি বিকল্প অবজেক্ট মান পাস করতে পারে:

```js
hydrateOnVisible({ rootMargin: '100px' })
```

### Hydrate on Media Query {#hydrate-on-media-query}

নির্দিষ্ট মিডিয়া ক্যোয়ারী মিলে গেলে হাইড্রেট করে।

```js
import { defineAsyncComponent, hydrateOnMediaQuery } from 'vue'

const AsyncComp = defineAsyncComponent({
  loader: () => import('./Comp.vue'),
  hydrate: hydrateOnMediaQuery('(max-width:500px)')
})
```

### Hydrate on Interaction {#hydrate-on-interaction}

কম্পোনেন্ট এলিমেন্টে নির্দিষ্ট ঘটনা(গুলি) ট্রিগার হলে হাইড্রেট হয়। যে ইভেন্টটি হাইড্রেশনকে ট্রিগার করেছে তা হাইড্রেশন সম্পূর্ণ হলে পুনরায় প্লে করা হবে।

```js
import { defineAsyncComponent, hydrateOnInteraction } from 'vue'

const AsyncComp = defineAsyncComponent({
  loader: () => import('./Comp.vue'),
  hydrate: hydrateOnInteraction('click')
})
```

একাধিক ইভেন্ট প্রকারের একটি তালিকাও হতে পারে:

```js
hydrateOnInteraction(['wheel', 'mouseover'])
```

### Custom Strategy {#custom-strategy}

```ts
import { defineAsyncComponent, type HydrationStrategy } from 'vue'

const myStrategy: HydrationStrategy = (hydrate, forEachElement) => {
  // forEachElement is a helper to iterate through all the root elements
  // in the component's non-hydrated DOM, since the root can be a fragment
  // instead of a single element
  forEachElement(el => {
    // ...
  })
  // call `hydrate` when ready
  hydrate()
  return () => {
    // return a teardown function if needed
  }
}

const AsyncComp = defineAsyncComponent({
  loader: () => import('./Comp.vue'),
  hydrate: myStrategy
})
```

## Using with Suspense {#using-with-suspense}

Async কম্পোনেন্টগুলি `<Suspense>` বিল্ট-ইন কম্পোনেন্টের সাথে ব্যবহার করা যেতে পারে। `<Suspense>` এবং অ্যাসিঙ্ক কম্পোনেন্টগুলির মধ্যে মিথস্ক্রিয়া নথিভুক্ত করা হয়েছে [`<Suspense>`](/guide/built-ins/suspense)-এর জন্য নিবেদিত অধ্যায়ে।
