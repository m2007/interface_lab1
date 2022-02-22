<template>
  <div>
    <b-table striped hover :items="table" :fields="fields" v-if="this.table && this.table[0]">
      <template #cell(delete)="data">
         <b-button size="sm" class="mr-2 del-btn" @click="del(data.index)" variant="danger">
           Удалить
        </b-button>
      </template>
    </b-table>
    <b-table striped hover :items="table" v-else>
      <template #cell(delete)="data">
         <b-button size="sm" class="mr-2" @click="del(data.index)" variant="danger">
           Удалить
        </b-button>
      </template>
    </b-table>

    <b-input-group class="mb-2" v-if="table.length && !procedure">
      <b-form-input v-for="k in keys" :key="k" :placeholder="k" v-model="values[k]"></b-form-input>
      <b-input-group-append>
        <b-button size="m" text="Добавить" variant="success" @click="add()">Добавить</b-button>
      </b-input-group-append>
    </b-input-group>
    
  </div>
</template>

<script>
export default {
  name: 'MyTable',
  props: {
    idt: Number,
    table: Array,
    procedure: Boolean
  },
  data: () => ({
    values: []
  }),
  computed: {
    keys() {
      return Object.keys(this.table[0])
    },
    fields() {
      let f = Object.keys(this.table[0])
      if (this.idt && f) {
        f.push({key: 'delete', label: ""})
      }
      return f
    }
  },
  methods: {
    del(index) {
      this.$emit('delete', {index, idt: this.idt})
    },
    add() {
      let row = {}
      for (let k of this.keys) {
        row[k] = this.values[k]
      }
      this.$emit('add', {row, idt: this.idt})
    }
  }
}
</script>

<style scoped>
.del-btn {
  transform: translate(-50%, 0);
  margin-left: 50%
}
</style>
