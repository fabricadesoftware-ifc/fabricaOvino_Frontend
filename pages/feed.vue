<template>
  <div>
    <title-bar :title-stack="titleStack" />
    <hero-bar>
      Gerenciamento de Rações
      <template v-slot:right>
        <router-link to="/" class="button"> Dashboard </router-link>
      </template>
    </hero-bar>
    <section class="section is-main-section">
      <card-component title="Adicionar / Editar" icon="ballot">
        <form-feed v-model="feed" />
      </card-component>
      <hr />
      <card-component
        title="Rações"
        icon="silverware"
        class="has-table has-mobile-sort-spaced"
      >
        <list-feed @loadFeed="loadFeed" />
      </card-component>
    </section>
  </div>
</template>

<script>
import CardComponent from '../components/templates/CardComponent.vue'
import TitleBar from '../components/templates/TitleBar.vue'
import HeroBar from '@/components/templates/HeroBar'
import FormFeed from '../components/feeds/FormFeed.vue'
import ListFeed from '../components/feeds/ListFeed.vue'

export default {
  auth: false,

  components: { TitleBar, HeroBar, CardComponent, ListFeed, FormFeed },
  data: function () {
    return {
      feed: {}
    }
  },
  computed: {
    titleStack() {
      return ['Admin', 'Rações']
    }
  },
  methods: {
    loadFeed(feed, mode = 'save') {
      this.mode = mode
      this.feed = {
        id: feed.id,
        name: feed.name,
        description: feed.description
      }
    }
  }
}
</script>

<style></style>
