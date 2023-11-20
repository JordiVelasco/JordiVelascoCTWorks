<template>
  <div class="q-pa-md">
    <q-btn @click="openDialog" label="Agregar Nueva Fila" color="primary" />
    <q-dialog v-model="dialogVisible">
      <q-card>
        <q-card-section>
          <q-input v-model="newRow.value" label="Data value" />
        </q-card-section>
        <q-card-actions>
          <q-btn label="Cancelar" color="negative" @click="closeDialog" />
          <q-btn label="Agregar" color="positive" @click="addCustomRow" />
        </q-card-actions>
      </q-card>
    </q-dialog>

    <q-table
      :data="data"
      :columns="columns"
      virtual-scroll
      :rows-per-page-options="[0]"
      row-key="name"
      separator="cell"
    >
      <template v-slot:body="props">
        <q-tr :props="props">
          <q-td key="label" :props="props">
            {{ props.row.label }}
            <q-popup-edit
              v-model="props.row.label"
              title="Edit the Name"
              auto-save
              v-slot="scope"
            >
              <q-input
                v-model="scope.value"
                dense
                autofocus
                counter
                @keyup.enter="scope.set"
              />
            </q-popup-edit>
          </q-td>
          <q-td key="value" :props="props">
            {{ props.row.value }}
            <q-popup-edit
              v-model="props.row.value"
              title="Edit the Name"
              auto-save
              v-slot="scope"
            >
              <q-input
                v-model="scope.value"
                dense
                autofocus
                counter
                @keyup.enter="scope.set"
              />
            </q-popup-edit>
          </q-td>
        </q-tr>
      </template>
    </q-table>
  </div>
</template>

<script>
const columns = [
  {
    name: "label",
    align: "center",
    label: "Field label",
    field: "label",
  },
  {
    name: "value",
    align: "center",
    label: "Data value",
    field: "value",
  },
];

const data = [
  {
    label: "value_1",
    value: 343,
  },
  {
    label: "value_2",
    value: "A: 32 B: 54",
  },
  {
    label: "value_3",
    value: 6343,
  },
];

export default {
  data() {
    return {
      dialogVisible: false,
      loading: false,
      filter: "",
      data,
      columns,
      newRow: { label: 3, value: "" },
    };
  },
  methods: {
    openDialog() {
      this.dialogVisible = true;
    },
    closeDialog() {
      this.dialogVisible = false;
    },
    addCustomRow() {
      if (this.newRow.value !== undefined) {
        const newRow = {
          label: "value_" + ++this.newRow.label,
          value: this.newRow.value,
        };
        this.data.push(newRow);
        this.closeDialog();
      }
    },
  },
};
</script>
