# Dahili direktifler {#built-in-directives}

## v-text {#v-text}

Elementin metin içeriğini güncelle.

- **Parametre olarak alır:** `string`

- **Detaylar**

  `v-text`, elementin [textContent](https://developer.mozilla.org/en-US/docs/Web/API/Node/textContent) özelliğini ayarlayarak çalışır, bu nedenle elementin içindeki mevcut içeriği üzerine yazar. Eğer `textContent`'in bir kısmını güncellemeniz gerekiyorsa, bunun yerine [mustache interpolations](/guide/essentials/template-syntax#text-interpolation) kullanmalısınız.

- **Örneğin**

  ```vue-html
  <span v-text="msg"></span>
  <!-- benzeri -->
  <span>{{msg}}</span>
  ```

- **Ayrıca Bakınız** [Şablon Sözdizimi - Metin İnterpolasyonu](/guide/essentials/template-syntax#text-interpolation)

## v-html {#v-html}

Elementin  [innerHTML](https://developer.mozilla.org/en-US/docs/Web/API/Element/innerHTML) özelliğini güncelle.

- **Parametre olarak alır:** `string`

- **Detaylar**

  `v-html` içeriği düz HTML olarak eklenir - Vue şablon sözdizimi işlenmez. Eğer `v-html` kullanarak şablonlar oluşturmaya çalışıyorsanız, bunun yerine bileşenler kullanarak çözümü tekrar düşünmeye çalışın.

::: warning Güvenlik Notu
Websitesinde dinamik olarak istediğiniz HTML'i oluşturmak çok tehlikeli olabilir çünkü bu, kolayca [XSS saldırılarına](https://en.wikipedia.org/wiki/Cross-site_scripting) yol açabilir. Sadece `v-html`'i güvenilir içerikte kullanın ve asla kullanıcı tarafından sağlanan içerikte kullanmayın.
:::

[Tek Dosya Bileşenleri](/guide/scaling-up/sfc) içinde, `scoped` stiller, `v-html` içindeki içeriğe uygulanmaz, çünkü bu HTML, Vue'un şablon derleyicisi tarafından işlenmez. `v-html` içeriğini scoped CSS ile hedeflemek istiyorsanız, bunun yerine [CSS modülleri](./sfc-css-features#css-modules) veya BEM gibi manuel bir kapsam stratejisi ile ek bir global `<style>` öğesi kullanabilirsiniz.

- **Örneğin**

  ```vue-html
  <div v-html="html"></div>
  ```

- **Ayrıca Bakınız** [Şablon Sözdizimi - Raw HTML](/guide/essentials/template-syntax#raw-html)

## v-show {#v-show}

Ifade değerinin doğruluğuna bağlı olarak elementin görünürlüğünü açıp kapatır.

- **Parametre olarak alır:** `any`

- **Detaylar**

  `v-show`, iç içe stiller aracılığıyla `display` CSS özelliğini ayarlayarak çalışır ve element görünür olduğunda başlangıçtaki `display` değerini korumaya çalışır. Aynı zamanda durumu değiştiğinde geçişleri tetikler.

- **See also** [Koşullu Renderlama - v-show](/guide/essentials/conditional#v-show)

## v-if {#v-if}

İfade değerinin doğruluğuna bağlı olarak bir öğe veya şablon parçasını koşullu olarak renderla.

- **Parametre olarak alır:** `any`

- **Detaylar**

  `v-if` öğesi devre dışı bırakıldığında, öğe ve içindeki direktifler / bileşenler yok edilir ve yeniden oluşturulur. Başlangıç durumu falsy ise, içerik hiç render edilmez.

  Text veya birden fazla öğe içeren bir koşullu bloğu belirtmek için `<template>` üzerinde kullanılabilir.

  Bu direktif, durumu değiştikçe geçişleri tetikler.

  Birlikte kullanıldığında, `v-if`, `v-for`'dan daha yüksek önceliğe sahiptir. Bu iki direktifi bir eleman üzerinde birlikte kullanmayı önermiyoruz — detaylar için [liste renderleme rehberine](/guide/essentials/list#v-for-with-v-if) bakın.

- **Ayrıca Bakınız** [Koşullu Renderlama - v-if](/guide/essentials/conditional#v-if)

## v-else {#v-else}

`v-if` veya `v-if` / `v-else-if` zinciri için "else bloğu"nu belirtir.

- **İfade beklemiyor**

- **Detaylar**

  - Kısıtlama: Önceki kardeş öğenin `v-if` veya `v-else-if` içermesi gerekir.

- `<template>` üzerinde kullanılarak, yalnızca metin veya birden fazla öğe içeren bir koşullu bloğu belirtmek için kullanılabilir.

- **Örneğin**

  ```vue-html
  <div v-if="Math.random() > 0.5">
    Şu an beni görüyorsun
  </div>
  <div v-else>
    Şu an görmüyorsun cano
  </div>
  ```

- **Ayrıca Bakınız** [Koşullu Renderlama - v-else](/guide/essentials/conditional#v-else)

## v-else-if {#v-else-if}

`v-if` için "else if bloğu"nu belirtir. Zincirlenebilir.

- **Parametre olarak alır:** `any`

- **Detaylar**

  - Kısıtlama: Önceki kardeş öğenin `v-if` veya `v-else-if` içermesi gerekir.

- `<template>` üzerinde kullanılarak, yalnızca metin veya birden fazla öğe içeren bir koşullu bloğu belirtmek için kullanılabilir.

- **Örneğin**

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
    A/B/C Yok gardaş
  </div>
  ```

- **Ayrıca Bakınız** [Koşullu Renderlama - v-else-if](/guide/essentials/conditional#v-else-if)

## v-for {#v-for}

Elementi veya şablon bloğunu kaynak verilerine bağlı olarak birden çok kez renderlar.

- **Parametre olarak alır:** `Array | Object | number | string | Iterable`

- **Detaylar**

  Directivenin değeri, üzerinde yineleme yapılan geçerli öğe için bir takma ad sağlamak için `alias in expression` özel sözdizimini kullanmalıdır:

  ```vue-html
  <div v-for="item in items">
    {{ item.text }}
  </div>
  ```

  Alternatif olarak, dizin için bir takma ad da belirtebilirsiniz (veya bir nesne üzerinde kullanılıyorsa anahtar):

  ```vue-html
  <div v-for="(item, index) in items"></div>
  <div v-for="(value, key) in object"></div>
  <div v-for="(value, name, index) in object"></div>
  ```

  `v-for`'un varsayılan davranışı, öğeleri yerinde yamalamaya çalışır ve onları taşımaz. Öğeleri yeniden sıralamak için, `key` özel özniteliği ile bir sıralama ipucu sağlamalısınız:

  ```vue-html
  <div v-for="item in items" :key="item.id">
    {{ item.text }}
  </div>
  ```

  `v-for`, [İterasyon Protokolü](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols#The_iterable_protocol)'ü uygulayan değerler üzerinde de çalışabilir, bunlar arasında yerel `Map` ve `Set` de bulunur.

- **Ayrıca Bakınız**
  - [Liste Renderlama](/guide/essentials/list)

## v-on {#v-on}

Attach an event listener to the element.

- **Shorthand:** `@`

- **Parametre olarak alır:** `Function | Inline Statement | Object (without argument)`

- **Argument:** `event` (optional if using Object syntax)

- **Modifiers**

  - `.stop` - call `event.stopPropagation()`.
  - `.prevent` - call `event.preventDefault()`.
  - `.capture` - add event listener in capture mode.
  - `.self` - only trigger handler if event was dispatched from this element.
  - `.{keyAlias}` - only trigger handler on certain keys.
  - `.once` - trigger handler at most once.
  - `.left` - only trigger handler for left button mouse events.
  - `.right` - only trigger handler for right button mouse events.
  - `.middle` - only trigger handler for middle button mouse events.
  - `.passive` - attaches a DOM event with `{ passive: true }`.

- **Details**

  The event type is denoted by the argument. The expression can be a method name, an inline statement, or omitted if there are modifiers present.

  When used on a normal element, it listens to [**native DOM events**](https://developer.mozilla.org/en-US/docs/Web/Events) only. When used on a custom element component, it listens to **custom events** emitted on that child component.

  When listening to native DOM events, the method receives the native event as the only argument. If using inline statement, the statement has access to the special `$event` property: `v-on:click="handle('ok', $event)"`.

  `v-on` also supports binding to an object of event / listener pairs without an argument. Note when using the object syntax, it does not support any modifiers.

- **Example**

  ```vue-html
  <!-- method handler -->
  <button v-on:click="doThis"></button>

  <!-- dynamic event -->
  <button v-on:[event]="doThis"></button>

  <!-- inline statement -->
  <button v-on:click="doThat('hello', $event)"></button>

  <!-- shorthand -->
  <button @click="doThis"></button>

  <!-- shorthand dynamic event -->
  <button @[event]="doThis"></button>

  <!-- stop propagation -->
  <button @click.stop="doThis"></button>

  <!-- prevent default -->
  <button @click.prevent="doThis"></button>

  <!-- prevent default without expression -->
  <form @submit.prevent></form>

  <!-- chain modifiers -->
  <button @click.stop.prevent="doThis"></button>

  <!-- key modifier using keyAlias -->
  <input @keyup.enter="onEnter" />

  <!-- the click event will be triggered at most once -->
  <button v-on:click.once="doThis"></button>

  <!-- object syntax -->
  <button v-on="{ mousedown: doThis, mouseup: doThat }"></button>
  ```

  Listening to custom events on a child component (the handler is called when "my-event" is emitted on the child):

  ```vue-html
  <MyComponent @my-event="handleThis" />

  <!-- inline statement -->
  <MyComponent @my-event="handleThis(123, $event)" />
  ```

- **See also**
  - [Event Handling](/guide/essentials/event-handling)
  - [Components - Custom Events](/guide/essentials/component-basics#listening-to-events)

## v-bind {#v-bind}

Dynamically bind one or more attributes, or a component prop to an expression.

- **Shorthand:** `:` or `.` (when using `.prop` modifier)

- **Parametre olarak alır:** `any (with argument) | Object (without argument)`

- **Argument:** `attrOrProp (optional)`

- **Modifiers**

  - `.camel` - transform the kebab-case attribute name into camelCase.
  - `.prop` - force a binding to be set as a DOM property. <sup class="vt-badge">3.2+</sup>
  - `.attr` - force a binding to be set as a DOM attribute. <sup class="vt-badge">3.2+</sup>

- **Usage**

  When used to bind the `class` or `style` attribute, `v-bind` supports additional value types such as Array or Objects. See linked guide section below for more details.

  When setting a binding on an element, Vue by default checks whether the element has the key defined as a property using an `in` operator check. If the property is defined, Vue will set the value as a DOM property instead of an attribute. This should work in most cases, but you can override this behavior by explicitly using `.prop` or `.attr` modifiers. This is sometimes necessary, especially when [working with custom elements](/guide/extras/web-components#passing-dom-properties).

  When used for component prop binding, the prop must be properly declared in the child component.

  When used without an argument, can be used to bind an object containing attribute name-value pairs.

- **Example**

  ```vue-html
  <!-- bind an attribute -->
  <img v-bind:src="imageSrc" />

  <!-- dynamic attribute name -->
  <button v-bind:[key]="value"></button>

  <!-- shorthand -->
  <img :src="imageSrc" />

  <!-- shorthand dynamic attribute name -->
  <button :[key]="value"></button>

  <!-- with inline string concatenation -->
  <img :src="'/path/to/images/' + fileName" />

  <!-- class binding -->
  <div :class="{ red: isRed }"></div>
  <div :class="[classA, classB]"></div>
  <div :class="[classA, { classB: isB, classC: isC }]"></div>

  <!-- style binding -->
  <div :style="{ fontSize: size + 'px' }"></div>
  <div :style="[styleObjectA, styleObjectB]"></div>

  <!-- binding an object of attributes -->
  <div v-bind="{ id: someProp, 'other-attr': otherProp }"></div>

  <!-- prop binding. "prop" must be declared in the child component. -->
  <MyComponent :prop="someThing" />

  <!-- pass down parent props in common with a child component -->
  <MyComponent v-bind="$props" />

  <!-- XLink -->
  <svg><a :xlink:special="foo"></a></svg>
  ```

  The `.prop` modifier also has a dedicated shorthand, `.`:

  ```vue-html
  <div :someProperty.prop="someObject"></div>

  <!-- equivalent to -->
  <div .someProperty="someObject"></div>
  ```

  The `.camel` modifier allows camelizing a `v-bind` attribute name when using in-DOM templates, e.g. the SVG `viewBox` attribute:

  ```vue-html
  <svg :view-box.camel="viewBox"></svg>
  ```

  `.camel` is not needed if you are using string templates, or pre-compiling the template with a build step.

- **See also**
  - [Class and Style Bindings](/guide/essentials/class-and-style)
  - [Components - Prop Passing Details](/guide/components/props#prop-passing-details)

## v-model {#v-model}

Create a two-way binding on a form input element or a component.

- **Parametre olarak alır:** varies based on value of form inputs element or output of components

- **Limited to:**

  - `<input>`
  - `<select>`
  - `<textarea>`
  - components

- **Modifiers**

  - [`.lazy`](/guide/essentials/forms#lazy) - listen to `change` events instead of `input`
  - [`.number`](/guide/essentials/forms#number) - cast valid input string to numbers
  - [`.trim`](/guide/essentials/forms#trim) - trim input

- **See also**

  - [Form Input Bindings](/guide/essentials/forms)
  - [Component Events - Usage with `v-model`](/guide/components/v-model)

## v-slot {#v-slot}

Denote named slots or scoped slots that expect to receive props.

- **Shorthand:** `#`

- **Parametre olarak alır:** JavaScript expression that is valid in a function argument position, including support for destructuring. Optional - only needed if expecting props to be passed to the slot.

- **Argument:** slot name (optional, defaults to `default`)

- **Limited to:**

  - `<template>`
  - [components](/guide/components/slots#scoped-slots) (for a lone default slot with props)

- **Example**

  ```vue-html
  <!-- Named slots -->
  <BaseLayout>
    <template v-slot:header>
      Header content
    </template>

    <template v-slot:default>
      Default slot content
    </template>

    <template v-slot:footer>
      Footer content
    </template>
  </BaseLayout>

  <!-- Named slot that receives props -->
  <InfiniteScroll>
    <template v-slot:item="slotProps">
      <div class="item">
        {{ slotProps.item.text }}
      </div>
    </template>
  </InfiniteScroll>

  <!-- Default slot that receive props, with destructuring -->
  <Mouse v-slot="{ x, y }">
    Mouse position: {{ x }}, {{ y }}
  </Mouse>
  ```

- **See also**
  - [Components - Slots](/guide/components/slots)

## v-pre {#v-pre}

Skip compilation for this element and all its children.

- **Does not expect expression**

- **Details**

  Inside the element with `v-pre`, all Vue template syntax will be preserved and rendered as-is. The most common use case of this is displaying raw mustache tags.

- **Example**

  ```vue-html
  <span v-pre>{{ this will not be compiled }}</span>
  ```

## v-once {#v-once}

Render the element and component once only, and skip future updates.

- **Does not expect expression**

- **Details**

  On subsequent re-renders, the element/component and all its children will be treated as static content and skipped. This can be used to optimize update performance.

  ```vue-html
  <!-- single element -->
  <span v-once>This will never change: {{msg}}</span>
  <!-- the element have children -->
  <div v-once>
    <h1>comment</h1>
    <p>{{msg}}</p>
  </div>
  <!-- component -->
  <MyComponent v-once :comment="msg"></MyComponent>
  <!-- `v-for` directive -->
  <ul>
    <li v-for="i in list" v-once>{{i}}</li>
  </ul>
  ```

  Since 3.2, you can also memoize part of the template with invalidation conditions using [`v-memo`](#v-memo).

- **See also**
  - [Data Binding Syntax - interpolations](/guide/essentials/template-syntax#text-interpolation)
  - [v-memo](#v-memo)

## v-memo <sup class="vt-badge" data-text="3.2+" /> {#v-memo}

- **Parametre olarak alır:** `any[]`

- **Details**

  Memoize a sub-tree of the template. Can be used on both elements and components. The directive Parametre olarak alır a fixed-length array of dependency values to compare for the memoization. If every value in the array was the same as last render, then updates for the entire sub-tree will be skipped. For example:

  ```vue-html
  <div v-memo="[valueA, valueB]">
    ...
  </div>
  ```

  When the component re-renders, if both `valueA` and `valueB` remain the same, all updates for this `<div>` and its children will be skipped. In fact, even the Virtual DOM VNode creation will also be skipped since the memoized copy of the sub-tree can be reused.

  It is important to specify the memoization array correctly, otherwise we may skip updates that should indeed be applied. `v-memo` with an empty dependency array (`v-memo="[]"`) would be functionally equivalent to `v-once`.

  **Usage with `v-for`**

  `v-memo` is provided solely for micro optimizations in performance-critical scenarios and should be rarely needed. The most common case where this may prove helpful is when rendering large `v-for` lists (where `length > 1000`):

  ```vue-html
  <div v-for="item in list" :key="item.id" v-memo="[item.id === selected]">
    <p>ID: {{ item.id }} - selected: {{ item.id === selected }}</p>
    <p>...more child nodes</p>
  </div>
  ```

  When the component's `selected` state changes, a large amount of VNodes will be created even though most of the items remained exactly the same. The `v-memo` usage here is essentially saying "only update this item if it went from non-selected to selected, or the other way around". This allows every unaffected item to reuse its previous VNode and skip diffing entirely. Note we don't need to include `item.id` in the memo dependency array here since Vue automatically infers it from the item's `:key`.

  :::warning
  When using `v-memo` with `v-for`, make sure they are used on the same element. **`v-memo` does not work inside `v-for`.**
  :::

  `v-memo` can also be used on components to manually prevent unwanted updates in certain edge cases where the child component update check has been de-optimized. But again, it is the developer's responsibility to specify correct dependency arrays to avoid skipping necessary updates.

- **See also**
  - [v-once](#v-once)

## v-cloak {#v-cloak}

Used to hide un-compiled template until it is ready.

- **Does not expect expression**

- **Details**

  **This directive is only needed in no-build-step setups.**

  When using in-DOM templates, there can be a "flash of un-compiled templates": the user may see raw mustache tags until the mounted component replaces them with rendered content.

  `v-cloak` will remain on the element until the associated component instance is mounted. Combined with CSS rules such as `[v-cloak] { display: none }`, it can be used to hide the raw templates until the component is ready.

- **Example**

  ```css
  [v-cloak] {
    display: none;
  }
  ```

  ```vue-html
  <div v-cloak>
    {{ message }}
  </div>
  ```

  The `<div>` will not be visible until the compilation is done.
