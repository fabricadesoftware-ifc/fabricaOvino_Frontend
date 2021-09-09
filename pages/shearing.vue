<template>
  <div class="shearing">
    <title-bar :title-stack="titleStack" />
    <hero-bar>
      {{ $t('pages.admin.breed.title') }}
      <template v-slot:right>
        <router-link to="sheep" class="button btn-first"> Animais </router-link>
        <router-link to="/" class="button"> Dashboard </router-link>
      </template>
    </hero-bar>
    <section class="section is-main-section">
      <card-component title="Nova Tosquia" icon="ballot">
        <form-shear />
      </card-component>
      <card-component
        title="Histórico de Tosquias"
        icon="file-document-multiple-outline"
        class="has-table has-mobile-sort-spaced"
      >
        <list-shearing />
      </card-component>
    </section>
  </div>
</template>
<script>
import CardComponent from '@/components/templates/CardComponent.vue'
import HeroBar from '@/components/templates/HeroBar'
import TitleBar from '@/components/templates/TitleBar.vue'
import FormShear from '@/components/sheeps/FormShear.vue'
import ListShearing from '@/components/shearing/ListShearing.vue'

export default {
  components: { FormShear, TitleBar, HeroBar, CardComponent, ListShearing },
  data() {
    return {
      data: {}
    }
  },
  computed: {
    titleStack() {
      return ['Admin', 'Ovelhas', this.$t('pages.admin.shearing.title')]
    }
  }
}
</script>

<style>
.btn-first {
  margin-right: 0.5em;
}
</style>
