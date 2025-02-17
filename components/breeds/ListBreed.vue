<template>
  <b-table
    striped
    :data="breeds"
    paginated
    paginate-position="bottom"
    aria-next-label="Next page"
    aria-previous-label="Previous page"
    aria-page-label="Page"
    aria-current-label="Current page"
    per-page="10"
  >
    <template v-slot="props">
      <b-table-column
        v-for="(column, index) in columns"
        :key="index"
        :field="column.field"
        :sortable="column.sortable"
        :label="column.label"
      >
        {{ props.row[column.field] }}

        <span v-if="column.field == 'actions'">
          <div class="buttons">
          <b-button
            type="is-small is-primary"
            icon-left="pencil"
            @click="loadBreed(props.row)"
          ></b-button>
          <b-button
            type="is-small is-danger"
            icon-left="trash-can-outline"
            @click="confirmRemove(props.row)"
          ></b-button>
        </div>
        </span>
      </b-table-column>
    </template>
  </b-table>
</template>

<script>
import { mapActions, mapState } from 'vuex'
export default {
  data() {
    return {
      columns: [
        {
          field: 'id',
          label: this.$t('pages.admin.breed.table.id'),
          sortable: true
        },
        {
          field: 'name',
          label: this.$t('pages.admin.breed.table.name'),
          sortable: true
        },
        {
          field: 'actions',
          label: this.$t('pages.admin.breed.table.actions'),
          sortable: false
        },
      ]
    }
  },
  computed: {
    ...mapState('breeds', ['breeds'])
  },
  created() {
    this.getBreeds()
  },
  methods: {
    ...mapActions('breeds', ['getBreeds']),
    loadBreed(breed) {
      this.$emit('loadBreed', breed)
    },
    confirmRemove(breed) {
      this.$buefy.dialog.confirm({
        title: 'Delete breed?',
        message:
          'Are you sure to <b>delete</b> this breed? This action cannot be undone.',
        confirmText: 'Delete',
        cancelText: 'Cancel',
        type: 'is-danger',
        hasIcon: true,
        onConfirm: () => this.remove(breed)
      })
    },
    async remove(breed) {
      const id = breed.id
      const url = `/api/v1/breeds/${id}`
      try {
        await this.$axios.delete(url)
        this.$toasted.global.defaultSuccess()
        this.getBreeds()
      } catch (err) {
        for (const item in err.response.data) {
          this.$toast.error(item + ': ' + err.response.data[item])
        }
      }
    }
  }
}
</script>

<style></style>
