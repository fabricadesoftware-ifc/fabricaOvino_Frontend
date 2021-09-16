<template>
  <b-table
    striped
    :data="shearing"
    default-sort="date"
    default-sort-direction="desc"
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
        :label="column.label"
        :sortable="column.sortable"
      >
        <span v-if="column.field == 'date'">
          {{ new Date(props.row[column.field]).toLocaleDateString() }}
        </span>

        <span v-else>
          {{ props.row[column.field] }}
        </span>

        <span v-if="column.field == 'actions'">
          <b-button 
            type="is-small is-primary" 
            icon-left="pencil" 
            @click="editShear(props.row)"
          ></b-button>
          <b-button
            type="is-small is-danger"
            icon-left="trash-can-outline"
            @click="confirmRemove(props.row)"
          ></b-button>
        </span>
      </b-table-column>
    </template>
  </b-table>
</template>

<script>
import { mapActions, mapState } from 'vuex'

export default {
  name: 'ListShearing',
  data() {
    return {
      columns: [
        {
          field: 'id',
          label: this.$t('pages.admin.shearing.table.id'),
          sortable: true
        },
        {
          field: 'sheep',
          label: this.$t('pages.admin.shearing.table.sheep'),
          sortable: true
        },
        {
          field: 'date',
          label: this.$t('pages.admin.shearing.table.date'),
          sortable: true
        },
        {
          field: 'amountOfWool',
          label: this.$t('pages.admin.shearing.table.amountOfWool'),
          sortable: true
        },
        {
          field: 'actions',
          label: this.$t('pages.admin.shearing.table.actions'),
          sortable: false
        }
      ],
    }
  },
  computed: {
    ...mapState('shearing', ['shearing'])
  },
  created() {
    this.getShearing()
  },
  methods: {
    ...mapActions('shearing', ['getShearing']),
    editShear(shearing) {
      this.$router.push({
        name: `sheeps-editShear___${this.$i18n.locale}`,
        params: { shearing }
      })
    },
    confirmRemove(shearing) {
      this.$buefy.dialog.confirm({
        title: 'Excluir tosquia?',
        message: 'Você tem certeza que deseja excluir este item?',
        confirmText: 'Excluir',
        cancelText: 'Cancelar',
        type: 'is-danger',
        hasIcon: true,
        onConfirm: () => this.remove(shearing)
      })
    },
    async remove(shearing) {
      const id = shearing.id
      const url = `api/v1/shearing/${id}`
      try {
        await this.$axios.delete(url)
        this.$toasted.global.defaultSuccess()
        this.getShearing()
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
