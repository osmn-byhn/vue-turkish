---
pageClass: api
---

# Dahili Bileşenler {#built-in-components}

:::Bilgi Kaydı ve Kullanım
Dahili bileşenler, kaydedilmeye gerek olmadan doğrudan şablonlarda kullanılabilir. Ayrıca, tree-shakeable'dirler: yalnızca kullanıldıklarında derlemede yer alırlar.

Onları [render functions](/guide/extras/render-function),  içinde kullanırken, bunları açıkça içe aktarmak gerekmektedir. Örneğin:

```js
import { h, Transition } from 'vue'

h(Transition, {
  /* props */
})
```

:::

## `<Transition>` {#transition}

Bir **tek** öğeye veya bileşene animasyonlu geçiş efektleri sağlar.

- **Props**

  ```ts
  interface TransitionProps {
    /**
     * Otomatik olarak geçiş CSS sınıf adlarını oluşturmak için kullanılır.
     * Örneğin, `name: 'fade'` otomatik olarak `.fade-enter`,
     * `.fade-enter-active`, vb. şeklinde genişler.
     */
    name?: string
    /**
     * CSS geçiş sınıflarını uygulayıp uygulamamak.
     * Varsayılan: true
     */
    css?: boolean
    /**
     * Geçiş bitiş zamanını belirlemek için beklenen geçiş olaylarının türünü belirtir.
     * Varsayılan davranış, daha uzun süreli olan türü otomatik olarak algılamaktır.
     */
    type?: 'transition' | 'animation'
    /**
     * Geçişin açık sürelerini belirtir.
     * Varsayılan davranış, kök geçiş öğesinde ilk `transitionend`
     * veya `animationend` etkinliğini beklemektir.
     */
    duration?: number | { enter: number; leave: number }
    /**
     * Ayrılma/giriş geçişlerinin zamanlamasını kontrol eder.
     * Varsayılan davranış eş zamanlıdır.
     */
    mode?: 'in-out' | 'out-in' | 'default'
    /**
     * Başlangıç renderında geçişi uygulayıp uygulamamak.
     * Varsayılan: false
     */
    appear?: boolean

    /**
     * Geçiş sınıflarını özelleştirmek için props.
     * Şablonlarda kebab-case kullanın, örneğin enter-from-class="xxx"
     */
    enterFromClass?: string
    enterActiveClass?: string
    enterToClass?: string
    appearFromClass?: string
    appearActiveClass?: string
    appearToClass?: string
    leaveFromClass?: string
    leaveActiveClass?: string
    leaveToClass?: string
  }
  ```

- **Olaylar**

  - `@before-enter`
  - `@before-leave`
  - `@enter`
  - `@leave`
  - `@appear`
  - `@after-enter`
  - `@after-leave`
  - `@after-appear`
  - `@enter-cancelled`
  - `@leave-cancelled` (`v-show` only)
  - `@appear-cancelled`

- **Örneğin**

  Basit bir element:

  ```vue-html
  <Transition>
    <div v-if="ok">toggled content</div>
  </Transition>
  ```

  `key` özniteliğini değiştirerek bir geçişi zorlamak:

  ```vue-html
  <Transition>
    <div :key="text">{{ text }}</div>
  </Transition>
  ```

  Geçiş modlu + görünüşte animasyonlu dinamik bir bileşen:

  ```vue-html
  <Transition name="fade" mode="out-in" appear>
    <component :is="view"></component>
  </Transition>
  ```

  Geçiş olaylarını dinleme:

  ```vue-html
  <Transition @after-enter="onTransitionComplete">
    <div v-show="ok">toggled content</div>
  </Transition>
  ```

- **Ayrıca Bakınız** [`<Transition>` Rehberi](/guide/built-ins/transition)

## `<TransitionGroup>` {#transitiongroup}

Bir listedeki **birden fazla** öğe veya bileşen için geçiş efektleri sağlar.

- **Props**

  `<TransitionGroup>`, `<Transition>` ile aynı props'ları kabul eder (ancak `mode` hariç), bunun yanı sıra iki ekstra prop'u vardır:

  ```ts
  interface TransitionGroupProps extends Omit<TransitionProps, 'mode'> {
    /**
     * Tanımlanmamışsa, bir fragment olarak işlenir.
     */
    tag?: string
    /**
     * Hareket geçişleri sırasında uygulanan CSS sınıfını özelleştirmek için.
     * Şablonlarda kebab-case kullanın, örneğin move-class="xxx"
     */
    moveClass?: string
  }
  ```

- **Olaylar**

  `<TransitionGroup>`, `<Transition>` ile aynı olayları yayımlar.

- **Detaylar**

  By default, `<TransitionGroup>` doesn't render a wrapper DOM element, but one can be defined via the `tag` prop.

  Not edilmelidir ki, `<transition-group>` içindeki her çocuğun animasyonların düzgün çalışması için [**benzersiz bir anahtara**](/guide/essentials/list#maintaining-state-with-key) sahip olması gerekir.

`<TransitionGroup>`, CSS dönüşümü aracılığıyla hareketli geçişleri destekler. Bir güncelleme sonrasında bir çocuğun ekran üzerindeki konumu değişmişse, hareket eden bir CSS sınıfı uygulanır (bu sınıf `name` özniteliğinden otomatik olarak oluşturulur veya `move-class` prop'u ile yapılandırılır). Hareketli sınıf uygulandığında CSS `transform` özelliği "geçişe uygunsa", öğe [FLIP tekniği](https://aerotwist.com/blog/flip-your-animations/) kullanılarak sorunsuz bir şekilde hedefine animasyonlu bir şekilde taşınır.
- **Örneğin**

  ```vue-html
  <TransitionGroup tag="ul" name="slide">
    <li v-for="item in items" :key="item.id">
      {{ item.text }}
    </li>
  </TransitionGroup>
  ```

- **Ayrıca Bakınız** [Rehber - TransitionGroup](/guide/built-ins/transition-group)

## `<KeepAlive>` {#keepalive}

İçine sarılan dinamik olarak geçiş yapan bileşenleri önbelleğe alır.

- **Props**

  ```ts
  interface KeepAliveProps {
    /**
     * Belirtilirse, yalnızca `include` ile eşleşen isimlere sahip
     * bileşenler önbelleğe alınacaktır.
     */
    include?: MatchPattern
    /**
     * `exclude` ile eşleşen isme sahip herhangi bir bileşen
     * önbelleğe alınmayacaktır.
     */
    exclude?: MatchPattern
    /**
     * Önbelleğe alınacak bileşen örneklerinin maksimum sayısı.
     */
    max?: number | string
  }

  type MatchPattern = string | RegExp | (string | RegExp)[]
  ```

- **Detaylar**

  Dinamik bir bileşeni saran `<KeepAlive>`, etkisiz bileşen örneklerini yok etmeden önbelleğe alır.

  Her zaman yalnızca bir etkin bileşen örneği, `<KeepAlive>`'ın doğrudan çocuğu olarak bulunabilir.

  Bir bileşen `<KeepAlive>` içinde geçiş yaptığında, `activated` ve `deactivated` yaşam döngü kancaları sırasıyla çağrılır. Bu, `mounted` ve `unmounted` çağrılmadığı durumlar için bir alternatif sağlar. Bu durum, `<KeepAlive>`'ın doğrudan çocuğu kadar, tüm alt bileşenlere de uygulanır.
- **Örneğin**

  Basitçe Kullanımı:

  ```vue-html
  <KeepAlive>
    <component :is="view"></component>
  </KeepAlive>
  ```

  `v-if` / `v-else` dallarıyla kullanıldığında, her seferinde yalnızca bir bileşenin render edilmiş olması gerekir:

  ```vue-html
  <KeepAlive>
    <comp-a v-if="a > 1"></comp-a>
    <comp-b v-else></comp-b>
  </KeepAlive>
  ```

  `<Transition>` ile birlikte kullanıldığında:

  ```vue-html
  <Transition>
    <KeepAlive>
      <component :is="view"></component>
    </KeepAlive>
  </Transition>
  ```

  `include` / `exclude` kullanımı:

  ```vue-html
  <!-- comma-delimited string -->
  <KeepAlive include="a,b">
    <component :is="view"></component>
  </KeepAlive>

  <!-- regex (use `v-bind`) -->
  <KeepAlive :include="/a|b/">
    <component :is="view"></component>
  </KeepAlive>

  <!-- Array (use `v-bind`) -->
  <KeepAlive :include="['a', 'b']">
    <component :is="view"></component>
  </KeepAlive>
  ```

  `max` ile kullanımı:

  ```vue-html
  <KeepAlive :max="10">
    <component :is="view"></component>
  </KeepAlive>
  ```

- **Ayrıca Bakınız** [Rehber - KeepAlive](/guide/built-ins/keep-alive)

## `<Teleport>` {#teleport}

Slot içeriğini DOM'un başka bir bölümüne render eder.

- **Props**

  ```ts
  interface TeleportProps {
    /**
     * Gerekli. Hedef konteynırını belirtin.
     * Bir seçici veya gerçek bir öğe olabilir.
     */
    to: string | HTMLElement
    /**
     * `true` olduğunda içerik, hedef konteynıra taşınmak yerine
     * orijinal konumunda kalacaktır.
     * Dinamik olarak değiştirilebilir.
     */
    disabled?: boolean
  }
  ```

- **Örneğin**

  Hedef konteynırı belirtme:

  ```vue-html
  <Teleport to="#some-id" />
  <Teleport to=".some-class" />
  <Teleport to="[data-teleport]" />
  ```

  Koşula bağlı olarak devre dışı bırakma:

  ```vue-html
  <Teleport to="#popup" :disabled="displayVideoInline">
    <video src="./my-movie.mp4">
  </Teleport>
  ```

- **Ayrıca Bakınız** [Rehber - Teleport](/guide/built-ins/teleport)

## `<Suspense>` <sup class="vt-badge experimental" /> {#suspense}

Bileşen ağacında iç içe geçmiş asenkron bağımlılıkları düzenlemek için kullanılır.

- **Props**

  ```ts
  interface SuspenseProps {
    timeout?: string | number
  }
  ```

- **Olaylar**

  - `@resolve`
  - `@pending`
  - `@fallback`

- **Detaylar**

  `<Suspense>`, iki slot'u kabul eder: `#default` slot ve `#fallback` slot. Varsayılan slot bellekte render edilirken, yedek slot'un içeriğini gösterecektir.

Varsayılan slot'u render ederken asenkron bağımlılıklarla ([Asenkron Bileşenler](/guide/components/async) ve [`async setup()`](/guide/built-ins/suspense#async-setup) içeren bileşenler) karşılaşırsa, varsayılan slot'u göstermeden önce tüm bağımlılıklar çözülene kadar bekleyecektir.

- **Ayrıca Bakınız** [Guide - Suspense](/guide/built-ins/suspense)
