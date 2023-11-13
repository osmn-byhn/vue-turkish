---
outline: deep
---

<script setup>
import { ref, onMounted } from 'vue'

const version = ref()

onMounted(async () => {
  const res = await fetch('https://api.github.com/repos/vuejs/core/releases/latest')
  version.value = (await res.json()).name
})
</script>

# Sürümler {#releases}

<p v-if="version">
Vue'nun şu anki en son kararlı sürümü sürüm <strong>{{ version }}</strong>.
</p>
<p v-else>
Son versiyon kontrol ediliyor...
</p>

Geçmiş sürümlerin tam bir değişiklik günlüğü şu adres üzerinde bulunabilir: [GitHub](https://github.com/vuejs/core/blob/main/CHANGELOG.md).
## Sürüm Döngüsü {#release-cycle}

Vue'un sabit bir sürüm döngüsü yoktur.

- Yama sürümleri ihtiyaç duyuldukça yayımlanır.

- Küçük sürümler her zaman yeni özellikler içerir ve genellikle 3 ila 6 ay arasında bir süre içerisinde çıkar. Küçük sürümler her zaman beta ön sürüm aşamasından geçer.

- Büyük sürümler önceden duyurulur ve erken tartışma ve alfa/beta ön sürüm aşamalarından geçer.

## Semantik Sürümleme Kenar Durumları {#semantic-versioning-edge-cases}

Vue sürümleri [Semantik Sürümleme](https://semver.org/) kurallarını izler ancak birkaç kenar durum vardır.

### TypeScript Tanımları {#typescript-definitions}

**Küçük** sürümler arasında TypeScript tanımlarında uyumsuz değişiklikler gönderebiliriz. Bu şu nedenlerden dolayıdır:

1. Bazen TypeScript kendisi küçük sürümler arasında uyumsuz değişiklikler gönderir ve biz de yeni TypeScript sürümlerini desteklemek için tipleri ayarlamak zorunda kalabiliriz.

2. Arada bir, yeni bir TypeScript sürümünde bulunan özellikleri benimsememiz gerekebilir, bu da TypeScript'in minimum gereken sürümünü yükseltmemize neden olur.

Eğer TypeScript kullanıyorsanız, Vue'un yeni bir küçük sürümü yayımlandığında, mevcut küçük sürümü kilitleyen bir semver aralığı kullanabilir ve yeni bir Vue küçük sürümü yayımlandığında manuel olarak güncelleme yapabilirsiniz.

### Eski Runtime ile Derlenmiş Kod Uyumluluğu {#compiled-code-compatibility-with-older-runtime}

Vue derleyicisinin yeni bir **küçük** sürümü, eski bir küçük sürümden gelen Vue runtime ile uyumlu olmayan bir kod üretebilir. Örneğin, Vue 3.2 derleyicisi tarafından üretilen kod, Vue 3.1 runtime tarafından tüketildiğinde tam olarak uyumlu olmayabilir.

Bu yalnızca kütüphane yazarları için bir endişe kaynağıdır, çünkü uygulamalarda derleyici sürümü ve runtime sürümü her zaman aynıdır. Bir uyumsuzluk, önceden derlenmiş Vue bileşen kodunu bir paket olarak gönderirken ve bir tüketici, Vue'nin eski bir sürümünü kullanan bir projede bunu kullanıyorsa ortaya çıkabilir. Sonuç olarak, paketiniz Vue'un minimum gereken küçük sürümünü açıkça belirtmelidir.

## Ön Sürümler {#pre-releases}

Küçük sürümler genellikle sabit bir beta sürüm sayısından geçer. Büyük sürümler alfa ve beta aşamalarından geçer.

Ayrıca, GitHub'daki `main` ve `minor` şubelerinden her hafta canary sürümleri yayımlıyoruz. Bunlar, kararlı kanalın npm meta verilerini şişirmemek için farklı paketler olarak yayımlanır. Onları `npx install-vue@canary` veya `npx install-vue@canary-minor` komutlarıyla kurabilirsiniz.

Ön-sürümler, entegrasyon / kararlılık testi ve kararsız özellikler için erken benimseyenlerin geri bildirim sağlaması amacıyla kullanılır. Lütfen ön-sürümleri üretimde kullanmayın. Tüm ön-sürümler kararsız kabul edilir ve aralarında kırıcı değişiklikler yapılabilir, bu nedenle ön-sürümleri kullanırken her zaman tam sürüme sabitleyin.

## Kaldırılmalar {#deprecations}

Bazı durumlarda, yeni ve daha iyi bir yerine geçen özellikleri küçük sürümlerde kaldırabiliriz. Kaldırılan özellikler çalışmaya devam edecek ve girişimde bulunulan durumdan sonraki büyük sürümde kaldırılacaktır.

## RFC'ler {#rfcs}

Vue'da API yüzeyi geniş olan ve Vue'a önemli değişiklikler getiren yeni özellikler **Talep Üzerine Yorumlar** (RFC) sürecinden geçer. RFC süreci, yeni özelliklerin çerçeveye girmesi için tutarlı ve kontrol edilebilir bir yol sağlamayı amaçlar ve kullanıcılara tasarım sürecinde katılım ve geri bildirim sağlama fırsatı sunar.

RFC süreci [vuejs/rfcs](https://github.com/vuejs/rfcs) deposunda yürütülür.

## Deneysel Özellikler {#experimental-features}

Bazı özellikler bir stabil Vue sürümünde gönderilir ve belgelenir, ancak deneysel olarak işaretlenir. Deneysel özellikler genellikle tasarım problemlerinin büyük çoğunluğu kağıt üzerinde çözülmüş, ancak gerçek dünya kullanımından geri bildirim eksik olduğu durumlarda kullanılır.

Deneysel özelliklerin amacı, kullanıcılara bunları bir üretim ortamında test ederek geri bildirim sağlama olanağı tanımaktır. Deneysel özellikler kendileri kararsız kabul edilir ve yalnızca kontrol edilen bir şekilde kullanılmalıdır; özellik değişebilir, bu nedenle deneysel özellikleri kullanırken her zaman bu durumu bekleyerek kullanın.
