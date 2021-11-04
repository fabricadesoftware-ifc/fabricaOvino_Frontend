<template>
  <form @submit.prevent="submit">
    <input id="feed-id" v-model="feed.id" type="hidden" />
    <b-field
      :label="$t('pages.admin.forms.name.label')"
      message="Nome da ração"
      horizontal
    >
      <b-input
        v-model="feed.name"
        :placeholder="$t('pages.admin.forms.name.placeholder')"
        type="text"
        icon="tag"
        required
      />
    </b-field>
    <b-field
      :label="$t('pages.admin.forms.description.label')"
      horizontal
    >
      <b-input
        v-model="feed.description"
        :placeholder="$t('pages.admin.forms.description.placeholder')"
        type="text"
        required
      />
    </b-field>
    <b-field horizontal>
      <b-field grouped>
        <div class="control">
          <b-button native-type="submit" type="is-primary">{{
            $t('buttons.save')
          }}</b-button>
        </div>
        <div class="control">
          <b-button type="is-primary is-outlined" @click="reset">{{
            $t('buttons.reset')
          }}</b-button>
        </div>
      </b-field>
    </b-field>
  </form>
</template>

<script>
import { mapActions } from 'vuex'

export default {
  props: {
    value: {
      type: Object,
      default() {
        return {}
      }
    }
  },
  data() {
    return {
      original: {},
      feed: {}
    }
  },
  watch: {
    value(newValue) {
      this.original = { ...newValue }
      this.feed = newValue
    },
    feed(newValue) {
      this.$emit('input', newValue)
    }
  },
  created() {
    if (this.value) {
      this.original = { ...this.value }
      this.feed = this.value
    }
  },
  methods: {
    ...mapActions('feeds', ['getFeeds']),
    async submit() {
      try {
        const method = this.value.id ? 'put' : 'post'
        const id = this.value.id ? `/${this.value.id}` : ''
        const url = `/api/v1/feeds${id}/`
        await this.$axios[method](url, this.value)
        this.$toasted.global.defaultSuccess()
        this.getFeeds()
        this.reset('reload')
      } catch (err) {
        for (const item in err.response.data) {
          this.$toast.error(item + ': ' + err.response.data[item])
        }
      }
    },
    reset(mode) {
      this.feed = this.feed.id && mode != 'reload' ? this.original : {}
    }
  }
}
</script>

<style></style>
