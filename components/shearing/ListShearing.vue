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
      </b-table-column>
      <b-table-colunm
        custom-key="actions"
        class="is-actions-cell"
        label="Ações"
        @click="editShear(props.row)"
      >
        <b-button type="is-small is-primary" icon-left="pencil"></b-button>
        <b-button
          type="is-small is-danger"
          icon-left="trash-can-outline"
          @click="confirmRemove(props.row)"
        ></b-button>
      </b-table-colunm>
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
          label: 'Nº Tosquia',
          sortable: true
        },
        {
          field: 'sheep',
          label: 'Nº Brinco',
          sortable: true
        },
        {
          field: 'date',
          label: 'Data',
          sortable: true
        },
        {
          field: 'amountOfWool',
          label: 'Quantidade de lã',
          sortable: true
        }
      ]
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
    }
  }
}
</script>

<style></style>
