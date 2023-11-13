<script lang="ts">
const shuffleMembers = (members: Member[], pinTheFirstMember = false): void => {
  let offset = pinTheFirstMember ? 1 : 0
  // `i` is between `1` and `length - offset`
  // `j` is between `0` and `length - offset - 1`
  // `offset + i - 1` is between `offset` and `length - 1`
  // `offset + j` is between `offset` and `length - 1`
  let i = members.length - offset
  while (i > 0) {
    const j = Math.floor(Math.random() * i);
    [
      members[offset + i - 1],
      members[offset + j]
    ] = [
      members[offset + j],
      members[offset + i - 1]
    ]
    i--
  }
}
</script>

<script setup lang="ts">
import { VTLink } from '@vue/theme'
import membersCoreData from './members-core.json'
import membersEmeritiData from './members-emeriti.json'
import membersPartnerData from './members-partner.json'
import TeamHero from './TeamHero.vue'
import TeamList from './TeamList.vue'
import type { Member } from './Member'
shuffleMembers(membersCoreData as Member[], true)
shuffleMembers(membersEmeritiData as Member[])
shuffleMembers(membersPartnerData as Member[])
</script>

<template>
  <div class="TeamPage">
    <TeamHero>
      <template #title>Takımı Tanı</template>
      <template #lead
        >Vue ve ekosisteminin gelişimi, uluslararası bir ekip tarafından yönlendirilmektedir. İşte bazıları 
        <span class="nowrap">aşağıda yer alan ekip üyeleri:</span></template
      >

      <template #action>
        <VTLink
          href="https://github.com/vuejs/governance/blob/master/Team-Charter.md"
          >Takımlar hakkında daha fazlası için</VTLink
        >
      </template>
    </TeamHero>

    <TeamList :members="membersCoreData as Member[]">
      <template #title>Core Team Members</template>
      <template #lead
        >Çekirdek ekip üyeleri, bir veya daha fazla temel proje bakımında aktif olarak yer alan kişilerdir. Vue ekosistemine önemli katkılarda bulunmuş, projenin ve kullanıcılarının uzun vadeli başarısına bağlı olan kişilerdir.</template
      >
    </TeamList>

    <TeamList :members="membersEmeritiData as Member[]">
      <template #title>Çekirdek Ekip Emeritusları</template>
      <template #lead
        >Burada, geçmişte değerli katkılarda bulunmuş ancak artık aktif olmayan bazı çekirdek ekip üyelerine saygı gösteriyoruz.</template
      >
    </TeamList>

    <TeamList :members="membersPartnerData as Member[]">
      <template #title>Topluluk Ortakları</template>
      <template #lead
        >Vue topluluğunun bazı üyeleri, onu o kadar zenginleştirdi ki, özel bir bahsi hak ediyorlar. Bu önemli ortaklarla daha samimi bir ilişki geliştirdik ve genellikle yaklaşan özellikler ve haberler konusunda onlarla koordinasyon sağladık.</template
      >
    </TeamList>
  </div>
</template>

<style scoped>
.TeamPage {
  padding-bottom: 16px;
}

@media (min-width: 768px) {
  .TeamPage {
    padding-bottom: 96px;
  }
}

.TeamList + .TeamList {
  padding-top: 64px;
}
</style>
