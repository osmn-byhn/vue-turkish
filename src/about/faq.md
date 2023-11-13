# Sıkça Sorulan Sorular {#frequently-asked-questions}

## Vue'yu kim yönetiyor? {#who-maintains-vue}

Vue bağımsız, topluluk destekli bir projedir. 2014 yılında [Evan You](https://twitter.com/youyuxi) tarafından kişisel bir yan proje olarak oluşturulmuştur. Bugün, Vue, [dünyanın dört bir yanından tam zamanlı ve gönüllü üyelerden oluşan bir ekip tarafından aktif bir şekilde sürdürülmektedir](/hakkinda/ekip), Evan ise proje lideri olarak görev yapmaktadır. Vue'nun hikayesi hakkında daha fazla bilgiyi bu [belgeselde](https://www.youtube.com/watch?v=OrxmtDw4pVI) bulabilirsiniz.

Vue'nun gelişimi sponsorluklar aracılığıyla finanse edilmektedir ve 2016 yılından bu yana mali açıdan sürdürülebilirdir. Eğer siz veya işletmeniz Vue'dan faydalanıyorsa, Vue'nun gelişimini desteklemek için [bizi sponsorlamayı](/sponsor/) düşünün!

## Vue 2 ve Vue 3 arasındaki fark nedir? {#what-s-the-difference-between-vue-2-and-vue-3}

Vue 3, mevcut ve en son büyük sürümüdür. Teleport, Suspense ve şablonda birden fazla kök öğe gibi Vue 2'de bulunmayan yeni özelliklere sahiptir. Ayrıca, Vue 2 ile uyumsuz kılan kesin değişiklikler içerir. Ayrıntılı bilgiler [Vue 3 Geçiş Rehberi'nde](https://v3-migration.vuejs.org/) belgelenmiştir.

Farklara rağmen, Vue'nun çoğu API'si iki büyük sürüm arasında paylaşılır, bu nedenle Vue 2 bilgilerinizin çoğu, Vue 3'te de çalışmaya devam edecektir. Özellikle, Kompozisyon API başlangıçta sadece Vue 3 için bir özellikken, şu anda Vue 2'ye geri taşınmış ve [Vue 2.7](https://github.com/vuejs/vue/blob/main/CHANGELOG.md#270-2022-07-01) sürümünde kullanılabilir.

Genel olarak, Vue 3 daha küçük paket boyutları, daha iyi performans, daha iyi ölçeklenebilirlik ve daha iyi TypeScript / IDE desteği sağlar. Eğer bugün yeni bir proje başlatıyorsanız, Vue 3'ü önerilen seçenek olarak düşünebilirsiniz. Şu anda Vue 2'yi düşünmeniz için sadece birkaç neden vardır:

- IE11'i desteklemeniz gerekiyorsa. Vue 3, modern JavaScript özelliklerinden yararlanır ve IE11'i desteklemez.

Mevcut bir Vue 2 uygulamasını Vue 3'e taşımak istiyorsanız, [geçiş rehberine](https://v3-migration.vuejs.org/) danışın.

## Vue 2 hala destekleniyor mu? {#is-vue-2-still-supported}

Temmuz 2022'de piyasaya sürülen Vue 2.7, Vue 2 sürüm aralığının final küçük sürümüdür. Vue 2 şimdi bakım moduna girmiştir: yeni özellikler eklenmeyecek, ancak 2.7 sürüm tarihinden itibaren kritik hata düzeltmeleri ve güvenlik güncellemeleri almaya devam edecektir. Bu, **Vue 2'nin 2023 yılının 31 Aralık'ta Son Kullanma Tarihine ulaşacağı** anlamına gelir.

Bu süreç, ekosistemin çoğunluğunun Vue 3'e geçmesi için yeterli zaman sağlayacağını düşünüyoruz. Ancak, güvenlik ve uyumluluk gereksinimlerini karşılamak zorunda olan ancak bu süre zarfında yükselme yapamayan takımlar veya projeler olabileceğini anlıyoruz. Vue 2'yi 2023 yılı sonrasında kullanmayı bekleyen takımlar için genişletilmiş destek sağlamak için endüstri uzmanlarıyla işbirliği yapmaktayız - eğer takımınız 2023 yılı sonrasında Vue 2 kullanmayı planlıyorsa, [Vue 2 Genişletilmiş LTS](https://v2.vuejs.org/lts/) hakkında daha fazla bilgi edinmek için plan yapın.

## Vue hangi lisansı kullanıyor? {#what-license-does-vue-use}

Vue, [MIT Lisansı](https://opensource.org/licenses/MIT) altında yayınlanan ücretsiz ve açık kaynaklı bir projedir.

## Vue hangi tarayıcıları destekliyor? {#what-browsers-does-vue-support}

Vue'nun en son sürümü (3.x), sadece [native ES2015 destekli tarayıcıları](https://caniuse.com/es6) destekler. Bu, IE11'i içermez. Vue 3.x, ES2015 özelliklerini kullanır ve eski tarayıcılarda polyfill yapılamaz, bu nedenle eski tarayıcıları desteklemeniz gerekiyorsa, Vue 2.x'i kullanmanız gerekecektir.

## Vue güvenilir mi? {#is-vue-reliable}

Vue, olgun ve sıkça test edilmiş bir çerçevedir. Bugün, dünya çapında 1.5 milyonun üzerinde kullanıcısı olan ve npm üzerinden aylık yaklaşık 10 milyon kez indirilen en yaygın kullanılan JavaScript çerçevelerinden biridir.

Vue, dünya genelinde Wikimedia Foundation, NASA, Apple, Google, Microsoft, GitLab, Zoom, Tencent, Weibo, Bilibili, Kuaishou ve birçok ünlü organizasyon tarafından çeşitli kapasitelerde üretimde kullanılmaktadır.

## Vue hızlı mı? {#is-vue-fast}

Vue 3, en performanslı mainstream frontend çerçevelerinden biridir ve çoğu web uygulama kullanım durumunu manuel optimizasyonlara ihtiyaç duymadan ele alır.

Stres testi senaryolarında, Vue [js-framework-benchmark](https://krausest.github.io/js-framework-benchmark/current.html) üzerinde React ve Angular'ı ciddi bir marjla geride bırakır. Ayrıca, benchmark'taki en hızlı üretim seviyesi non-Virtual-DOM çerçevelerinden bazılarına karşı başa baş bir performans sergiler.

Lütfen yukarıdaki gibi sentetik benchmark'lar, özel optimizasyonlarla ham rendering performansına odaklanır ve gerçek dünya performans sonuçlarını tam olarak temsil etmeyebilir. Sayfa yükleme performansına daha fazla önem veriyorsanız, bu web sitesini [WebPageTest](https://www.webpagetest.org/lighthouse) veya [PageSpeed Insights](https://pagespeed.web.dev/) kullanarak denetleyebilirsiniz. Bu web sitesi kendisi Vue tarafından desteklenmektedir, SSG ön-renderleme, tam sayfa hidrasyon ve SPA istemci tarafı gezinme ile. Yavaş 4G ağlarında 4x CPU kısıtlaması ile emüle edilen Moto G4 üzerinde performans üzerinde 100 alır.

Vue'nun çalışma zamanı performansını nasıl otomatik olarak optimize ettiği hakkında daha fazla bilgi için [Rendering Mekanizması](/rehber/ekstralar/rendering-mechanism) bölümüne ve özellikle zorlu durumlarda bir Vue uygulamasını nasıl optimize edeceğiniz hakkında bilgi için [Performans Optimizasyon Rehberi](/rehber/best-practices/performance) bölümüne göz atabilirsiniz.

## Vue hafif mi? {#is-vue-lightweight}

Bir yapı aracı kullandığınızda, Vue'nun birçok API'si ["tree-shakable"](https://developer.mozilla.org/en-US/docs/Glossary/Tree_shaking) (ağaç sarsılabilir) olarak işlev gösterir. Örneğin, yerleşik `<Transition>` bileşenini kullanmıyorsanız, bu, son üretim paketine dahil edilmeyeceği anlamına gelir.

Sadece mutlak minimum API'leri kullanan bir merhaba dünya Vue uygulamasının temel boyutu, minifikasyon ve brotli sıkıştırması ile sadece yaklaşık **16kb**'dir. Uygulamanın gerçek boyutu, çerçeveden kullanılan isteğe bağlı özelliklerin sayısına bağlı olacaktır. Eğer bir uygulama, Vue'nun sağladığı her özelliği kullanıyorsa, toplam çalışma zamanı boyutu yaklaşık **27kb**'dir.

Vue'yu bir yapı aracı olmadan kullanırken, ağaç sarsma yeteneğini kaybetmekle kalmaz, aynı zamanda şablom derleyicisini tarayıcıya göndermek zorundadır. Bu, boyutu yaklaşık **41kb**'ye çıkarır. Bu nedenle, Vue'yu genellikle bir yapı adımı olmadan aşamalı geliştirme için kullanıyorsanız, [petite-vue](https://github.com/vuejs/petite-vue) (sadece **6kb**) kullanmayı düşünün.

Bazı çerçeveler, örneğin Svelte, tek bileşen senaryolarında son derece hafif bir çıktı üreten derleme stratejisi kullanır. Ancak [araştırmamız](https://github.com/yyx990803/vue-svelte-size-analysis) gösteriyor ki boyut farkı uygulamadaki bileşen sayısına bağlı olarak büyük ölçüde değişmektedir. Vue, daha ağır bir başlangıç boyutuna sahip olmasına rağmen, bileşen başına daha az kod üretir. Gerçek dünya senaryolarında, bir Vue uygulamasının oldukça hafif olması mümkündür.

## Vue ölçeklenebilir mi? {#does-vue-scale}

Evet. Vue'nun yalnızca basit kullanım durumları için uygun olduğu yaygın bir yanılgıya rağmen, Vue büyük ölçekli uygulamaları başarıyla yönetebilir:

- [Tek Dosya Bileşenleri](/guide/scaling-up/sfc), uygulamanın farklı bölümlerinin izole bir şekilde geliştirilmesine olanak tanıyan modüler bir geliştirme modeli sunar.

- [Kompozisyon API](/guide/reusability/composables), birinci sınıf TypeScript entegrasyonu sağlar ve karmaşık mantığı düzenlemenizi, çıkarmanızı ve tekrar kullanmanızı mümkün kılar.

- [Kapsamlı araç desteği](/guide/scaling-up/tooling), uygulama büyüdükçe sorunsuz bir geliştirme deneyimi sağlar.

- Düşük başlangıç seviyesi ve mükemmel belgelendirme, yeni geliştiriciler için düşük onboarding ve eğitim maliyetlerine dönüşür.

## Vue'ye Nasıl Katkıda Bulunabilirim? {#how-do-i-contribute-to-vue}

İlgilendiğiniz için teşekkür ederiz! Lütfen [Topluluk Rehberi](/about/community-guide)'mize göz atın.

## Options API mı, Composition API mı Kullanmalıyım? {#should-i-use-options-api-or-composition-api}

Eğer Vue'a yeni başlıyorsanız, iki stil arasında yüksek seviyeli bir karşılaştırma [burada](/guide/introduction#which-to-choose) bulunmaktadır.

Eğer daha önce Options API kullanmış ve şu anda Composition API'yi değerlendiriyorsanız, [bu SSS](/guide/extras/composition-api-faq)'ya göz atın.

## Vue ile JavaScript mi, TypeScript mi Kullanmalıyım? {#should-i-use-javascript-or-typescript-with-vue}

Vue kendisi TypeScript ile uygulanmış olmasına rağmen, kullanıcı olarak TypeScript kullanıp kullanmamak konusunda bir görüşü zorlamaz.

TypeScript desteği, Vue'a yeni özellikler eklendiğinde önemli bir düşüncedir. TypeScript için tasarlanmış API'lar genellikle IDE'lerin ve linter'ların anlamasını kolaylaştırır, hatta siz TypeScript kullanmasanız bile. Herkes kazanır. Vue API'ları, mümkün olduğunca JavaScript ve TypeScript'te aynı şekilde çalışacak şekilde tasarlanmıştır.

TypeScript'e geçiş, onboarding karmaşıklığı ile uzun vadeli bakım kazanımları arasında bir takas gerektirir. Bu takasın haklı olup olmadığı, ekibinizin geçmişi ve proje ölçeğine bağlı olarak değişebilir, ancak Vue genellikle bu kararı vermekte etkili bir faktör değildir.

## Vue, Web Bileşenleri ile Nasıl Karşılaştırılır? {#how-does-vue-compare-to-web-components}

Vue, Web Bileşenleri'nin doğal olarak kullanılabilir olduğundan önce oluşturuldu, ve Vue'un tasarımının bazı yönleri (örneğin, slotlar) Web Bileşenleri modelinden esinlenilmiştir.

Web Bileşenleri spesifikasyonları, özel elementleri tanımlamak etrafında odaklanan düşük seviyeli spesifikasyonlardır. Bir çerçeve olarak, Vue etkili DOM renderlama, reaktif durum yönetimi, araçlar, istemci tarafı yönlendirme ve sunucu tarafı renderlama gibi ek yüksek seviyeli konulara odaklanır.

Vue aynı zamanda yerel özel elementlere tüketme veya bu özel elementlere dışa aktarma konusunu tamamen destekler - daha fazla ayrıntı için [Vue ve Web Bileşenleri Rehberi'ne](/guide/extras/web-components) göz atın.

<!-- ## TODO Vue, React ile Nasıl Karşılaştırılır? -->

<!-- ## TODO Vue, Angular ile Nasıl Karşılaştırılır? -->

