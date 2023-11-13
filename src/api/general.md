# Global API: General {#global-api-general}

## version {#version}

Mevcut Vue sürümünü açığa çıkarır.

- **Tür:** `string`

- **Örnek**

  ```js
  import { version } from 'vue'

  console.log(version)
  ```

## nextTick() {#nexttick}

Sonraki DOM güncelleme işleminin tamamlanmasını beklemek için kullanılan bir yardımcı fonksiyon.

- **Type**

  ```ts
  function nextTick(callback?: () => void): Promise<void>
  ```

- **Detaylar**

  Vue'da reaktif durumu değiştirdiğinizde, oluşan DOM güncellemeleri eşzamanlı olarak uygulanmaz. Bunun yerine, Vue, her bir bileşenin yalnızca bir kez güncellenmesini sağlamak için bunları "bir sonraki tick"e kadar arabelleğe alır.

  `nextTick()`, bir durum değişikliğinden hemen sonra DOM güncellemelerinin tamamlanmasını beklemek için kullanılabilir. Bir geri çağrıyı argüman olarak iletebilir veya dönen Promise'i bekleyebilirsiniz.

- **Örnek**

  <div class="composition-api">

  ```vue
  <script setup>
  import { ref, nextTick } from 'vue'

  const count = ref(0)

  async function increment() {
    count.value++

    // DOM not yet updated
    console.log(document.getElementById('counter').textContent) // 0

    await nextTick()
    // DOM is now updated
    console.log(document.getElementById('counter').textContent) // 1
  }
  </script>

  <template>
    <button id="counter" @click="increment">{{ count }}</button>
  </template>
  ```

  </div>
  <div class="options-api">

  ```vue
  <script>
  import { nextTick } from 'vue'

  export default {
    data() {
      return {
        count: 0
      }
    },
    methods: {
      async increment() {
        this.count++

        // DOM not yet updated
        console.log(document.getElementById('counter').textContent) // 0

        await nextTick()
        // DOM is now updated
        console.log(document.getElementById('counter').textContent) // 1
      }
    }
  }
  </script>

  <template>
    <button id="counter" @click="increment">{{ count }}</button>
  </template>
  ```

  </div>
- **Ayrıca bakınız** [`this.$nextTick()`](/api/component-instance#nexttick)

## defineComponent() {#definecomponent}

Tür çıkarımı ile bir Vue bileşeni tanımlamak için bir tür yardımcısı.

- **Tür**


  ```ts
  // options syntax
  function defineComponent(
    component: ComponentOptions
  ): ComponentConstructor

  // function syntax (requires 3.3+)
  function defineComponent(
    setup: ComponentOptions['setup'],
    extraOptions?: ComponentOptions
  ): () => any
  ```

  > Type is simplified for readability.

- **Detaylar**

  İlk argüman, bir bileşen seçenekleri nesnesini bekler. Dönüş değeri, işlev aslında tür çıkarımı amaçlı çalışan bir işleve dayandığından, aynı seçenek nesnesi olacaktır.

  Dönüş türü biraz özeldir: İlk etapta, seçeneklere dayanarak çıkarılan bileşen örneği türüne sahip bir yapılandırıcı türü olacaktır. Bu, döndürülen tür, TSX içinde bir etiket olarak kullanıldığında tür çıkarımı için kullanılır.

  Bir bileşenin örnek türünü (seçeneklerindeki `this` türüne eşdeğer) `defineComponent()`'in dönüş türünden şu şekilde çıkarabilirsiniz:

  ```ts
  const Foo = defineComponent(/* ... */)

  type FooInstance = InstanceType<typeof Foo>
  ```

### İşlev İmzası <sup class="vt-badge" data-text="3.3+" /> {#function-signature}

`defineComponent()`, Composition API ve [render fonksiyonları veya JSX](/guide/extras/render-function.html) ile kullanılmak üzere tasarlanmış alternatif bir imzaya sahiptir.

Seçenekler nesnesini geçirmek yerine, beklenti bir işlevdir. Bu işlev, Composition API [`setup()`](/api/composition-api-setup.html#composition-api-setup) işleviyle aynı şekilde çalışır: props ve setup bağlamını alır. Dönüş değeri bir render fonksiyonu olmalıdır - hem `h()` hem de JSX desteklenir:

  ```js
  import { ref, h } from 'vue'

  const Comp = defineComponent(
    (props) => {
      // use Composition API here like in <script setup>
      const count = ref(0)

      return () => {
        // render function or JSX
        return h('div', count.value)
      }
    },
    // extra options, e.g. declare props and emits
    {
      props: {
        /* ... */
      }
    }
  )
  ```

  Bu imzanın ana kullanım durumu, genellikle TypeScript (ve özellikle TSX ile) ile birlikte kullanıldığında, çünkü generikleri destekler:


  ```tsx
  const Comp = defineComponent(
    <T extends string | number>(props: { msg: T; list: T[] }) => {
      // use Composition API here like in <script setup>
      const count = ref(0)

      return () => {
        // render function or JSX
        return <div>{count.value}</div>
      }
    },
    // manual runtime props declaration is currently still needed.
    {
      props: ['msg', 'list']
    }
  )
  ```

  Gelecekte, çalışma zamanı prop bildirimlerini (`defineProps` için SFC'lerde olduğu gibi) atlamaya olanak tanıyan ve otomatik olarak çıkarıp enjekte eden bir Babel eklentisi sağlamayı planlıyoruz.

  ### Webpack Treeshaking Hakkında Not {#note-on-webpack-treeshaking}

  Çünkü `defineComponent()`, bir işlev çağrısı olduğu için, bazı yapı araçlarına, örneğin webpack'e yan etkiler üretebilecekmiş gibi görünebilir. Bu, bileşenin asla kullanılmadığı durumda bile bileşenin tree-shaken olmasını engelleyebilir.

  Webpack'e bu işlev çağrısının tree-shaken için güvenli olduğunu söylemek için işlev çağrısından önce `/*#__PURE__*/` yorum notasyonunu ekleyebilirsiniz:

  ```js
  export default /*#__PURE__*/ defineComponent(/* ... */)
  ```

  Bu, Vite kullanıyorsanız gerekli değildir, çünkü Vite tarafından kullanılan temel üretim sarmalayıcısı olan Rollup, `defineComponent()`'in gerçekte yan etki içermeyen bir işlev olduğunu belirlemekte yeterince akıllıdır ve manuel açıklamalara ihtiyaç duymaz.

- **Ayrıca bakınız** [Rehber - Vue'u TypeScript ile Kullanma](/guide/typescript/overview#general-usage-notes)

## defineAsyncComponent() {#defineasynccomponent}

Yalnızca render edildiğinde yüklenen bir asenkron bileşeni tanımlar. Argüman, ya bir yükleyici işlevi ya da yükleme davranışını daha fazla kontrol etmek için daha gelişmiş seçenekler içeren bir seçenek nesnesi olabilir.

- **Tür**

  ```ts
  function defineAsyncComponent(
    source: AsyncComponentLoader | AsyncComponentOptions
  ): Component

  type AsyncComponentLoader = () => Promise<Component>

  interface AsyncComponentOptions {
    loader: AsyncComponentLoader
    loadingComponent?: Component
    errorComponent?: Component
    delay?: number
    timeout?: number
    suspensible?: boolean
    onError?: (
      error: Error,
      retry: () => void,
      fail: () => void,
      attempts: number
    ) => any
  }
  ```

- **Ayrıca bakınız** [Rehber - Asenkron Bileşenler](/guide/components/async)

## defineCustomElement() {#definecustomelement}

Bu yöntem, [`defineComponent`](#definecomponent) ile aynı argümanı kabul eder, ancak yerine bir yerel [Özel Element](https://developer.mozilla.org/en-US/docs/Web/Web_Components/Using_custom_elements) sınıf yapıcısını döndürür.

- **Tür**

  ```ts
  function defineCustomElement(
    component:
      | (ComponentOptions & { styles?: string[] })
      | ComponentOptions['setup']
  ): {
    new (props?: object): HTMLElement
  }
  ```

  > Okunabilirlik için tür basitleştirilmiştir.

- **Detaylar**

  Normal bileşen seçeneklerine ek olarak, `defineCustomElement()` ayrıca, öğenin gölge köküne enjekte edilmesi gereken bir dizi iç içe geçmiş CSS dizesi sağlamak için `styles` adlı özel bir seçeneği de destekler.

  Dönüş değeri, [`customElements.define()`](https://developer.mozilla.org/en-US/docs/Web/API/CustomElementRegistry/define) kullanılarak kaydedilebilen bir özel öğe yapıcısıdır.

- **Örnek**

  ```js
  import { defineCustomElement } from 'vue'

  const MyVueElement = defineCustomElement({
    /* component options */
  })

  // Register the custom element.
  customElements.define('my-vue-element', MyVueElement)
  ```
  
- **Ayrıca bakınız**

  - [Rehber - Vue ile Özel Elementler Oluşturma](/guide/extras/web-components#building-custom-elements-with-vue)

  - Ayrıca, `defineCustomElement()`, Tek Dosya Bileşenleri ile kullanıldığında [özel yapılandırmayı](/guide/extras/web-components#sfc-as-custom-element) gerektirir.
