# Uygulama API'si {#application-api}

## createApp() {#createapp}

Bir uygulama örneği oluşturur.

- **Tür**

  ```ts
  function createApp(rootComponent: Component, rootProps?: object): App
  ```

- **Detaylar**

  İlk argüman, kök bileşendir. İkinci, isteğe bağlı argüman, kök bileşene iletilmesi gereken özelliklerdir.

- **Örnek**

  İç içe geçmiş kök bileşen ile:
  ```js
  import { createApp } from 'vue'

  const app = createApp({
    /* root component options */
  })
  ```

  İçeri aktarılan bileşen ile:
  
  ```js
  import { createApp } from 'vue'
  import App from './App.vue'

  const app = createApp(App)
  ```
- **Ayrıca bakınız** [Rehber - Bir Vue Uygulaması Oluşturma](/guide/essentials/application)

## createSSRApp() {#createssrapp}

[Uygulama SSR Hidrasyon](/guide/scaling-up/ssr#client-hydration) modunda bir uygulama örneği oluşturur. Kullanımı tam olarak `createApp()` ile aynıdır.

## app.mount() {#app-mount}

Uygulama örneğini bir konteyner öğesine monte eder.

- **Tür**

  ```ts
  interface App {
    mount(rootContainer: Element | string): ComponentPublicInstance
  }
  ```
- **Detaylar**

  Argüman, gerçek bir DOM öğesi veya bir CSS seçici olabilir (eşleşen ilk öğe kullanılır). Kök bileşen örneğini döndürür.

  Bileşenin bir şablonu veya bir render işlevi tanımlanmışsa, konteyner içindeki mevcut DOM düğümlerini değiştirir. Aksi takdirde, çalışma zamanı derleyicisi kullanılabilirse, konteynerın `innerHTML`'i şablon olarak kullanılır.

  SSR hidrasyon modunda, konteyner içindeki mevcut DOM düğümlerini hidrate eder. [Uyuşmazlıklar](/guide/scaling-up/ssr#hydration-mismatch) varsa, mevcut DOM düğümleri beklenen çıktıya uymak üzere değiştirilir.

  Her uygulama örneği için, `mount()` yalnızca bir kez çağrılabilir.

- **Örnek**
  ```js
  import { createApp } from 'vue'
  const app = createApp(/* ... */)

  app.mount('#app')
  ```

  Gerçek bir DOM öğesine de monte edilebilir:

  ```js
  app.mount(document.body.firstChild)
  ```

## app.unmount() {#app-unmount}

Monte edilmiş bir uygulama örneğini kaldırır ve uygulamanın bileşen ağacındaki tüm bileşenler için unmount yaşam döngü kancalarını tetikler.

- **İpucu**

  ```ts
  interface App {
    unmount(): void
  }
  ```

## app.component() {#app-component}

Eğer hem bir ad dizesi hem de bir bileşen tanımı iletiliyorsa, global bir bileşeni kaydeder; sadece ad geçiyorsa, zaten kaydedilmiş bir bileşeni alır.

- **İpucu**

  ```ts
  interface App {
    component(name: string): Component | undefined
    component(name: string, component: Component): this
  }
  ```

- **Örnek**

  ```js
  import { createApp } from 'vue'

  const app = createApp({})

  // register an options object
  app.component('my-component', {
    /* ... */
  })

  // retrieve a registered component
  const MyComponent = app.component('my-component')
  ```

- **Ayrıca bakınız** [Bileşen Kaydı](/guide/components/registration)

## app.directive() {#app-directive}

Eğer hem bir ad dizesi hem de bir yönerge tanımı iletiliyorsa, global bir özel yönerge kaydedilir; sadece ad geçiyorsa, zaten kaydedilmiş bir yönergeyi alır.

- **İpucu**

  ```ts
  interface App {
    directive(name: string): Directive | undefined
    directive(name: string, directive: Directive): this
  }
  ```

- **Örnek**

  ```js
  import { createApp } from 'vue'

  const app = createApp({
    /* ... */
  })

  // register (object directive)
  app.directive('my-directive', {
    /* custom directive hooks */
  })

  // register (function directive shorthand)
  app.directive('my-directive', () => {
    /* ... */
  })

  // retrieve a registered directive
  const myDirective = app.directive('my-directive')
  ```

- **Ayrıca bakınız** [Özel Yönergeler](/guide/reusability/custom-directives)

## app.use() {#app-use}

Bir eklenti yükler [plugin](/guide/reusability/plugins).

- **İpucu**

  ```ts
  interface App {
    use(plugin: Plugin, ...options: any[]): this
  }
  ```

- **Detay**

  İlk argüman olarak eklentiyi ve isteğe bağlı eklenti seçeneklerini ikinci argüman olarak bekler.

  Eklenti, install() yöntemine sahip bir nesne olabilir veya yalnızca install() yöntemi olarak kullanılacak bir işlev olabilir. Seçenekler (app.use()'nin ikinci argümanı) eklentinin install() yöntemine iletilir.

  Aynı eklenti üzerinde app.use() birden fazla kez çağrılırsa, eklenti yalnızca bir kez yüklenir.

- **Örnek**

  ```js
  import { createApp } from 'vue'
  import MyPlugin from './plugins/MyPlugin'

  const app = createApp({
    /* ... */
  })

  app.use(MyPlugin)
  ```

- **Ayrıca Bakınız** [Plugins](/guide/reusability/plugins)

## app.mixin() {#app-mixin}

Global bir mixin uygular (uygulama kapsamında). Bir global mixin, içerdiği seçenekleri uygulamadaki her bileşen örneğine uygular.

:::warning Tavsiye Edilmez
Mixin'ler, özellikle global mixin'ler, genellikle ekosistem kütüphanelerinde yaygın olarak kullanıldığından, Vue 3'te özellikle geriye dönük uyumluluk için desteklenmektedir. Mixin'lerin kullanımı, özellikle global mixin'lerin kullanımı, uygulama kodunda kaçınılmalıdır.

Mantık yeniden kullanımı için [Composables](/guide/reusability/composables) tercih edilmelidir
:::

- **İpucu**

  ```ts
  interface App {
    mixin(mixin: ComponentOptions): this
  }
  ```

## app.provide() {#app-provide}

Uygulama içindeki tüm alt bileşenlere enjekte edilebilecek bir değer sağlar.

- **İpucu**

  ```ts
  interface App {
    provide<T>(key: InjectionKey<T> | symbol | string, value: T): this
  }
  ```

- **Detaylar**

  Enjeksiyon anahtarını birinci argüman olarak bekler ve sağlanan değeri ikinci argüman olarak alır. Uygulama örneğini kendisi döndürür.

- **Örnek**

  ```js
  import { createApp } from 'vue'

  const app = createApp(/* ... */)

  app.provide('message', 'hello')
  ```

  Uygulama içinde bir bileşenin içinde:

  <div class="composition-api">

  ```js
  import { inject } from 'vue'

  export default {
    setup() {
      console.log(inject('message')) // 'hello'
    }
  }
  ```

  </div>
  <div class="options-api">

  ```js
  export default {
    inject: ['message'],
    created() {
      console.log(this.message) // 'hello'
    }
  }
  ```

  </div>

- **Ayrıca Bakınız**
  - [Provide / Inject](/guide/components/provide-inject)
  - [App-level Provide](/guide/components/provide-inject#app-level-provide)
  - [app.runWithContext()](#app-runwithcontext)

## app.runWithContext()<sup class="vt-badge" data-text="3.3+" /> {#app-runwithcontext}

Mevcut uygulamayı enjeksiyon bağlamı olarak kullanarak bir geri çağrıyı yürütür.

- **İpucu**

  ```ts
  interface App {
    runWithContext<T>(fn: () => T): T
  }
  ```

- **Detaylar**

  Bir geri çağrı işlevini bekler ve geri çağrıyı hemen çalıştırır. Geri çağrının senkron çağrısı sırasında, `inject()` çağrıları mevcut etkin bileşen örneği olmadığında bile mevcut uygulama tarafından sağlanan değerlerden enjeksiyonları arayabilir. Geri çağrının dönüş değeri de döndürülür.

- **Örnek**

  ```js
  import { inject } from 'vue'

  app.provide('id', 1)

  const injected = app.runWithContext(() => {
    return inject('id')
  })

  console.log(injected) // 1
  ```

## app.version {#app-version}

Uygulamanın oluşturulduğu Vue sürümünü sağlar. Bu, [plugins](/guide/reusability/plugins), kullanışlıdır, farklı Vue sürümlerine dayalı koşullu mantık gerekebileceği durumlarda.

- **İpucu**

  ```ts
  interface App {
    version: string
  }
  ```

- **Örnek**

  Bir eklenti içinde sürüm kontrolü yapma:

  ```js
  export default {
    install(app) {
      const version = Number(app.version.split('.')[0])
      if (version < 3) {
        console.warn('This plugin requires Vue 3')
      }
    }
  }
  ```

- **Ayrıca Bakınız** [Global API - version](/api/general#version)

## app.config {#app-config}

Her uygulama örneği, uygulama için yapılandırma ayarlarını içeren bir `config` nesnesini ortaya çıkarır. Uygulamanızı monte etmeden önce bu özelliklerini (aşağıda belgelenmiştir) değiştirebilirsiniz.

```js
import { createApp } from 'vue'

const app = createApp(/* ... */)

console.log(app.config)
```

## app.config.errorHandler {#app-config-errorhandler}

Uygulama içinden yayılan yakalanmamış hatalar için global bir işleyici atar.

- **İpucu**

  ```ts
  interface AppConfig {
    errorHandler?: (
      err: unknown,
      instance: ComponentPublicInstance | null,
      // `info` is a Vue-specific error info,
      // e.g. which lifecycle hook the error was thrown in
      info: string
    ) => void
  }
  ```

- **Detaylar**

  Hata işleyici, üç argüman alır: hata, hatayı tetikleyen bileşen örneği ve hatanın kaynak türünü belirten bir bilgi dizesi.

Aşağıdaki kaynaklardan hataları yakalayabilir:

- Bileşen render işlemleri
- Olay işleyicileri
- Yaşam döngü kancaları
- `setup()` fonksiyonu
- İzleyiciler (watchers)
- Özel yönerge kancaları
- Geçiş kancaları

- **Örnek**

  ```js
  app.config.errorHandler = (err, instance, info) => {
    // handle error, e.g. report to a service
  }
  ```

## app.config.warnHandler {#app-config-warnhandler}

Vue'dan gelen çalışma zamanı uyarıları için özel bir işleyici atar.

- **İpucu**

  ```ts
  interface AppConfig {
    warnHandler?: (
      msg: string,
      instance: ComponentPublicInstance | null,
      trace: string
    ) => void
  }
  ```

- **Detaylar**

  Uyarı işleyicisi, uyarı mesajını birinci argüman olarak, kaynak bileşen örneğini ikinci argüman olarak ve bileşen izini dizesini üçüncü argüman olarak alır.

Belirli uyarıları filtrelemek ve konsol karmaşıklığını azaltmak için kullanılabilir. Tüm Vue uyarıları geliştirme sırasında ele alınmalıdır, bu nedenle bu yalnızca bir hata ayıklama oturumları sırasında birçok uyarı arasında belirli uyarılara odaklanmak için önerilir ve hata ayıklama işlemi tamamlandıktan sonra kaldırılmalıdır.

  :::İpucu
  Uyarılar yalnızca geliştirme sırasında çalışır, bu nedenle bu yapılandırma üretim modunda görmezden gelinir.
  :::

- **Örnek**

  ```js
  app.config.warnHandler = (msg, instance, trace) => {
    // `trace` is the component hierarchy trace
  }
  ```

## app.config.performance {#app-config-performance}

Tarayıcı geliştirici aracı performans/timeline panelinde bileşen başlatma, derleme, render ve yama performans izleme özelliğini etkinleştirmek için bunu true olarak ayarlayın. Yalnızca geliştirme modunda ve [performance.mark](https://developer.mozilla.org/en-US/docs/Web/API/Performance/mark) API'sini destekleyen tarayıcılarda çalışır.

- **İpucu:** `boolean`

- **Ayrıca Bakınız** [Guide - Performance](/guide/best-practices/performance)

## app.config.compilerOptions {#app-config-compileroptions}

Çalışma zamanı derleyici seçeneklerini yapılandırın. Bu nesneye ayarlanan değerler, tarayıcıda çalışan şablondan derleyiciye iletilir ve yapılandırılmış uygulamadaki her bileşeni etkiler. Ayrıca, bu seçenekleri her bir bileşen için [`compilerOptions` option](/api/options-rendering#compileroptions) kullanarak geçersiz kılabilirsiniz.

::: warning Önemli
Bu yapılandırma seçeneği, tam derleme kullanıldığında (yani, tarayıcıda şablonları derleyebilen bağımsız `vue.js`), sadece saygı görmektedir. Runtime-only derleme ile bir yapı kurulumu kullanıyorsanız, derleyici seçenekleri yerine derleme aracı yapılandırmaları ile `@vue/compiler-dom`'a iletilmelidir.

- `vue-loader` için: [pass via the `compilerOptions` loader option](https://vue-loader.vuejs.org/options.html#compileroptions). Also see [how to configure it in `vue-cli`](https://cli.vuejs.org/guide/webpack.html#modifying-options-of-a-loader).

- `vite` içim: [pass via `@vitejs/plugin-vue` options](https://github.com/vitejs/vite-plugin-vue/tree/main/packages/plugin-vue#options).
  :::

### app.config.compilerOptions.isCustomElement {#app-config-compileroptions-iscustomelement}

Yerel özel öğeleri tanımak için bir kontrol yöntemi belirtir.

- **İpucu:** `(tag: string) => boolean`

- **Detaylar**

  Eğer etiketin bir yerel özel öğe olarak ele alınması gerekiyorsa, `true` değerini döndürmelidir. Eşleşen bir etiket için, Vue, onu bir Vue bileşeni olarak çözmeye çalışmak yerine bir yerel öğe olarak render eder.

  Bu fonksiyonda HTML ve SVG'nin yerel etiketleri eşleştirilmek zorunda değildir - Vue'nun ayrıştırıcısı bunları otomatik olarak tanır.

- **Örnek**

  ```js
  // treat all tags starting with 'ion-' as custom elements
  app.config.compilerOptions.isCustomElement = (tag) => {
    return tag.startsWith('ion-')
  }
  ```

- **Ayrıca Bakınız** [Vue ve Web Components](/guide/extras/web-components)

### app.config.compilerOptions.whitespace {#app-config-compileroptions-whitespace}

Şablon boşluk işleme davranışını ayarlar.

- **İpucu:** `'condense' | 'preserve'`

- **Varsayılan:** `'condense'`

- **Detaylar**

  Vue, şablonlardaki boşluk karakterlerini daha verimli derlenmiş çıktı üretmek için kaldırır / sıkıştırır. Varsayılan strateji "condense" olup, şu davranışları içerir:

  1. Bir öğenin içindeki baştaki / sondaki boşluk karakterleri tek bir boşluğa sıkıştırılır.
  2. Yeni satırlar içeren öğeler arasındaki boşluk karakterleri kaldırılır.
  3. Metin düğümlerindeki ardışık boşluk karakterleri tek bir boşluğa sıkıştırılır.

  Bu seçeneği `'preserve'` olarak ayarlamak, (2) ve (3)'ü devre dışı bırakacaktır.

- **Örnek**

  ```js
  app.config.compilerOptions.whitespace = 'preserve'
  ```

### app.config.compilerOptions.delimiters {#app-config-compileroptions-delimiters}

Şablonda metin interpolasyonu için kullanılan ayraçları ayarlar.

- **İpucu:** `[string, string]`

- **Varsayılan:** `{{ "['\u007b\u007b', '\u007d\u007d']" }}`

- **Detaylar**

  Bu genellikle mustache sözdizimini kullanan sunucu taraflı çerçevelerle çakışmayı önlemek için kullanılır.

- **Örnek**

  ```js
  // Delimiters changed to ES6 template string style
  app.config.compilerOptions.delimiters = ['${', '}']
  ```

### app.config.compilerOptions.comments {#app-config-compileroptions-comments}

Şablonlardaki HTML yorumlarının işlemesini ayarlar.

- **İpucu:** `boolean`

- **Varsayılan:** `false`

- **Detaylar**

  Vue, varsayılan olarak üretimde yorumları kaldırır. Bu seçeneği `true` olarak ayarlamak, Vue'nun üretimde bile yorumları korumasını zorlar. Yorumlar her zaman geliştirme sırasında korunur. Bu seçenek, Vue'nun HTML yorumlarına dayanan diğer kütüphanelerle kullanıldığında genellikle kullanılır.

- **Örnek**

  ```js
  app.config.compilerOptions.comments = true
  ```

## app.config.globalProperties {#app-config-globalproperties}

Herhangi bir bileşen örneğinde erişilebilen global özellikleri kaydetmek için kullanılabilen bir nesne.

- **İpucu**

  ```ts
  interface AppConfig {
    globalProperties: Record<string, any>
  }
  ```

- **Detaylar**

  Bu, Vue 3'te artık bulunmayan Vue 2'nin `Vue.prototype`'nin yerine geçer. Her şey gibi, bu global özelliklerin kullanımı dikkatlice yapılmalıdır.

  Eğer global bir özellik, bir bileşenin kendi özelliği ile çakışıyorsa, bileşenin kendi özelliği öncelikli olacaktır.

- **Kullanımı**

  ```js
  app.config.globalProperties.msg = 'hello'
  ```

  Bu, `msg`'yi uygulamadaki herhangi bir bileşen şablonunda ve aynı zamanda herhangi bir bileşen örneğinin `this`'inde kullanılabilir hale getirir:

  ```js
  export default {
    mounted() {
      console.log(this.msg) // 'hello'
    }
  }
  ```

- **Ayrıca Bakınız** [Rehber - Global Özellikleri Artırma](/guide/typescript/options-api#augmenting-global-properties) <sup class="vt-badge ts" />

## app.config.optionMergeStrategies {#app-config-optionmergestrategies}

Özel bileşen seçenekleri için birleştirme stratejilerini tanımlamak için bir nesne.

- **İpucu**

  ```ts
  interface AppConfig {
    optionMergeStrategies: Record<string, OptionMergeFunction>
  }

  type OptionMergeFunction = (to: unknown, from: unknown) => any
  ```

- **Detylar**

  Bazı eklentiler / kütüphaneler, özel bileşen seçeneklerini desteklemek için (global mixin'ler enjekte ederek) ekler. Bu seçenekler, aynı seçenek birden çok kaynaktan "birleştirilmelİ" olduğunda (örneğin, mixin'ler veya bileşen mirası), özel birleştirme mantığına ihtiyaç duyabilir.

  Bir özel seçenek için birleştirme stratejisi fonksiyonu, bu fonksiyonu seçeneğin adını anahtar olarak kullanarak `app.config.optionMergeStrategies` nesnesine atayarak kaydedilebilir.

  Birleştirme stratejisi fonksiyonu, sırasıyla birinci ve ikinci argümanlar olarak, bu seçeneğin değerini tanımlayan üst örnekler üzerinde tanımlanmış olan değeri alır.

- **Örnek**

  ```js
  const app = createApp({
    // option from self
    msg: 'Vue',
    // option from a mixin
    mixins: [
      {
        msg: 'Hello '
      }
    ],
    mounted() {
      // merged options exposed on this.$options
      console.log(this.$options.msg)
    }
  })

  // define a custom merge strategy for `msg`
  app.config.optionMergeStrategies.msg = (parent, child) => {
    return (parent || '') + (child || '')
  }

  app.mount('#app')
  // logs 'Hello Vue'
  ```

- **Ayrıca Bakınız** [Component Instance - `$options`](/api/component-instance#options)
