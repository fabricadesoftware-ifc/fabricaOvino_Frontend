<template>
  <section>
    <PageTitle
      icon="scissors-cutting"
      :main="$t('pages.pagetitle.shearing_edit.title')"
      :sub="$t('pages.pagetitle.sheep_edit.subtitle')"
    />
    <b-tabs>
      <b-tab-item :label="$t('pages.pagetitle.shearing_edit.title')">
        <b-button
          v-if="edit"
          type="is-dark"
          icon-left="redo"
          @click="reset()"
          >{{ $t('buttons.reset') }}</b-button
        >
        <b-button
          v-else
          type="is-warning"
          icon-left="pencil"
          @click="edit = !edit"
          >{{ $t('buttons.edit') }}</b-button
        >
        <span class="button" @click="back()">
          Voltar
        </span>
        <hr />
        <div class="form">
          <input id="shearing-id" v-model="shearing" type="hidden" />
          <b-field :label="$t('pages.admin.shearing.forms.amountOfWool')">
            <b-numberinput
              v-model="shearing.amountOfWool"
              controls-position="compact"
              controls-rounded
              step="0.01"
              :disabled="!edit"
              required
            ></b-numberinput>
          </b-field>

          <b-field :label="$t('pages.admin.shearing.forms.date.label')">
            <b-datepicker
              v-model="date"
              icon="calendar-today"
              :disabled="!edit"
            ></b-datepicker>
          </b-field>

          <div v-if="edit">
            <b-button type="is-info" icon-left="check" @click="save">{{
              $t('buttons.save')
            }}</b-button>
            <b-button type="is-dark" icon-left="redo" @click="reset">{{
              $t('buttons.reset')
            }}</b-button>
          </div>
        </div>
      </b-tab-item>
    </b-tabs>
  </section>
</template>

<script>
import { showError } from '@/plugins/global'
import PageTitle from '@/components/templates/PageTitle'
export default {
  components: { PageTitle },
  props: {
    value: {
      type: Object,
      default() {
        return {}
      }
    },
    edit: {
      type: Boolean,
      default() {
        return false
      }
    }
  },
  async fetch() {
    this.date = new Date(this.shearing.date)
  },
  asyncData({ params }) {
    return {
      shearing: params.shearing
    }
  },
  data() {
    return {
      date: ''
    }
  },
  methods: {
    back() {
      const pBack = this.$route.params.back
      const url = pBack != undefined ? `sheeps-${pBack}___${this.$i18n.locale}` : `shearing___${this.$i18n.locale}`
      const pSheep = this.$route.params.sheep != undefined ? this.$route.params.sheep : null
      this.$router.push({
        name: url,
        params: {sheep: pSheep}
      })
    },
    reset() {
      this.shearing = {
        id: this.value.id,
        amountOfWool: this.value.amountOfWool,
        date: this.value.date
      }
      this.date = new Date(this.shearing.date)
      this.edit = false
    },
    async save() {
      const url = `/api/v1/shearing/${this.shearing.id}/`
      this.shearing.date = this.date.toLocaleDateString('fr-CA')
      try {
        await this.$axios['patch'](url, this.shearing)
        this.$toasted.global.defaultSuccess()
        this.edit = false
      } catch (e) {
        showError(e)
      }
    }
  }
}
</script>

<style></style>
