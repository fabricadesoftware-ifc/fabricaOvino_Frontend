<template>
  <div>
    <b-button
      v-if="!deleteMode"
      type="is-primary"
      tag="nuxt-link"
      :to="localePath({ name: 'admin-user-add' })"
      icon-left="account-plus"
      >{{ $t('components.admin.useradmin.addNew') }}</b-button
    >
    <span v-else>
      <b-button type="is-danger" icon-left="account-group" @click="deleteUser()"
        >{{ $t('components.admin.useradmin.confirmDeletion') }} -
        {{ userToDelete.name }}</b-button
      >
    </span>
    <b-table striped :data="users">
      <template v-slot="props">
        <b-table-column
          v-for="(column, index) in columns"
          :key="index"
          :field="column.field"
          :sortable="column.sortable"
          :label="column.label"
          >{{ props.row[column.field] }}</b-table-column
        >

        <b-table-column
          field="actions"
          :label="$t('pages.admin.breed.table.actions')"
        >
          <b-button
            type="is-warning"
            icon-left="pencil"
            @click="editUser(props.row)"
          ></b-button>
          <b-button type="is-danger" icon-left="trash-can-outline"></b-button>
        </b-table-column>
      </template>
    </b-table>
  </div>
</template>

<script>
export default {
  async fetch() {
    this.users = await this.$axios.$get('/api/v1/users')
  },
  data() {
    return {
      users: [],
      columns: [
        {
          field: 'id',
          label: this.$t('components.admin.useradmin.table.id'),
          sortable: true
        },
        {
          field: 'email',
          label: this.$t('components.admin.useradmin.table.email'),
          sortable: true
        },
        {
          field: 'name',
          label: this.$t('components.admin.useradmin.table.name'),
          sortable: true
        }
      ]
    }
  },
  methods: {
    editUser(user) {
      this.$router.push({
        name: `admin-user-edit___${this.$i18n.locale}`,
        params: { user }
      })
    }
  }
}
</script>

<style></style>
