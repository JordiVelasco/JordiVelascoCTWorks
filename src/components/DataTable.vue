<template>
  <div class="q-pa-md">
    <q-table title="Treats" :data="data" :columns="columns" row-key="id">
      <template v-slot:top>
        <q-btn
          color="primary"
          :disable="loading"
          label="Add row"
          @click="openDialog"
        />
        <q-space />
      </template>
    </q-table>

    <q-dialog v-model="dialog" persistent>
      <q-card>
        <q-card-section>
          <q-input v-model="newRow.value" label="Data value" />
        </q-card-section>
        <q-card-actions align="right">
          <q-btn label="Cancel" color="primary" @click="closeDialog" />
          <q-btn label="Add" color="positive" @click="addCustomRow" />
        </q-card-actions>
      </q-card>
    </q-dialog>
  </div>
</template>

<script>
export default {
  data() {
    return {
      loading: false,
      filter: "",
      rowCount: 10,
      columns: [
        {
          name: "desc",
          required: true,
          label: "Field Label",
          align: "left",
          field: (row) => "value_" + row.label,
          format: (val) => `${val}`,
          sortable: true,
        },
        {
          name: "desc",
          required: true,
          label: "Data Value",
          align: "left",
          field: (row) => row.value,
          format: (val) => `${val}`,
          sortable: true,
        },
      ],
      data: [
        { id: 1, label: 1, value: '343' },
        { id: 2, label: 2, value: 'A: 32 B: 54' },
        { id: 3, label: 3, value: '6343' },
      ],
      dialog: false,
      newRow: { label: 3, value: "" },
    };
  },
  methods: {
    openDialog() {
      this.dialog = true;
    },
    closeDialog() {
      this.dialog = false;
    },
    addCustomRow() {
      if (this.newRow.value) {
        const newRow = {
          id: ++this.rowCount,
          label: ++this.newRow.label,
          value: this.newRow.value,
        };
        this.data.push(newRow);
        this.closeDialog();
      }
    },
    removeRow() {
      this.loading = true;
      setTimeout(() => {
        const index = Math.floor(Math.random() * this.data.length);
        this.data = [
          ...this.data.slice(0, index),
          ...this.data.slice(index + 1),
        ];
        this.loading = false;
      }, 500);
    },
  },
};
</script>
