<template>
  <form @submit.prevent="save">
    <b-field :label="$t('pages.admin.shearing.forms.sheep.label')" horizontal>
      <b-select
        v-model="shearing.sheep"
        :placeholder="$t('pages.admin.shearing.forms.sheep.placeholder')"
        icon="sheep"
        required
      >
        <option v-for="sheep in listSheeps" :key="sheep.id" :value="sheep.id">
          {{ sheep.earringNumber }}
        </option>
      </b-select>
    </b-field>
    <v-date
      v-model="date"
      :label="$t('pages.admin.shearing.forms.date.label')"
    />

    <b-field :label="$t('pages.admin.shearing.forms.amountOfWool')" horizontal>
      <b-numberinput
        v-model="shearing.amountOfWool"
        step="0.01"
        required
      ></b-numberinput>
    </b-field>

    <b-field horizontal>
      <b-field grouped>
        <div class="control">
          <b-button native-type="submit" type="is-primary">
            {{ $t('buttons.save') }}</b-button
          >
        </div>
        <div class="control">
          <b-button type="is-primary is-outlined" @click.prevent="reset()">{{
            $t('buttons.reset')
          }}</b-button>
        </div>
      </b-field>
    </b-field>
  </form>
</template>
<script>
import VDate from '@/components/templates/VDate'
import { mapActions, mapState, mapGetters } from 'vuex'
export default {
  components: { VDate },
  data() {
    return {
      date: new Date(),
      shearing: {
        amountOfWool: 0
      }
    }
  },
  computed: {
    ...mapState('auth', ['user']),
    ...mapGetters('sheeps', ['listSheeps'])
  },
  created() {
    this.getSheeps()
  },
  methods: {
    ...mapActions('sheeps', ['getSheeps']),
    reset() {
      this.shearing = {
        amountOfWool: 0
      }
    },
    async save() {
      this.shearing.date = (this.date.getFullYear() + "-" + (this.date.getMonth() + 1) + "-" + (this.date.getDate() + 1))
      this.shearing.user = this.user.id
      try {
        await this.$axios.$post('/api/v1/shearing/', this.shearing)
        this.$toasted.global.defaultSuccess()
        this.getSheeps()
        this.$root.$emit('reloadGetShearing')
        this.reset()
      } catch (err) {
        for (const item in err.response.data) {
          this.$toast.error(item + ': ' + err.response.data[item])
        }
      }
    }
  }
}
</script>
