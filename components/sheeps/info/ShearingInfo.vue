<template>
  <section>
    <section class="section is-main-section">
      <div style="text-align: right">
        <router-link :to="localePath({ name: 'sheep' })" class="button">
          Voltar
        </router-link>
        <router-link :to="localePath({ name: 'shearing' })" class="button">
          Nova Tosquia
        </router-link>
        <router-link to="/" class="button"> Dashboard </router-link>
      </div>
      <card-component
        title="Histórico de Tosquias"
        icon="file-document-multiple-outline"
        class="has-table has-mobile-sort-spaced"
      >
        <b-table
          striped
          :data="sheep.shearing"
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
                {{ new Date(props.row[column.field]).toLocaleDateString() }} -
                {{ new Date(props.row[column.field]).toLocaleTimeString() }}
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
              <b-button
                type="is-small is-primary"
                icon-left="pencil"
              ></b-button>
              <b-button
                type="is-small is-danger"
                icon-left="trash-can-outline"
                @click="confirmRemove(props.row)"
              ></b-button>
            </b-table-colunm>
          </template>
        </b-table>
      </card-component>
    </section>
  </section>
</template>

<script>
import { mapState } from 'vuex'
import CardComponent from '@/components/templates/CardComponent.vue'
export default {
  components: { CardComponent },
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
    this.sheep = {
      id: this.value.id,
      earringNumber: this.value.earringNumber,
      breed: this.breeds.find(elem => elem.name == this.value.breed).id,
      category: this.categories.find(elem => elem.name == this.value.category)
        .id,
      birthday: this.value.birthday,
      sex: this.value.sex == 'Male' ? 'M' : 'F',
      teethQuantity: this.value.teethQuantity,
      lactating: this.value.lactating,
      shearing: this.value.shearing
    }
    this.sheep.shearing = {
      id: this.shearing.id,
      date: this.shearing.date,
      amountOfWool: this.shearing.amountOfWool
    }
    this.edit = false
  },
  data() {
    return {
      columns: [
        {
          field: 'id',
          label: 'Nº Tosquia',
          sortable: true
        },
        {
          field: 'date',
          label: 'Data e Hora',
          sortable: true
        },
        {
          field: 'amountOfWool',
          label: 'Quantidade de lã',
          sortable: true
        }
      ],
      sheep: {}
    }
  },
  computed: {
    ...mapState('categories', ['categories']),
    ...mapState('breeds', ['breeds'])
    //...mapState('shearing', ['shearing'])
  },
  /*created() {
    this.getShearing()
  },*/
  methods: {
    //...mapActions('shearing', ['getShearing']),
    editShear(shearing) {
      this.$router.push({
        name: `sheeps-editShear___${this.$i18n.locale}`,
        params: { shearing, back: 'history', sheep: this.value }
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
