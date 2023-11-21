<template>
  <div class="q-pa-md">
    <q-btn @click="openDialog" label="Agregar Nueva Fila" color="primary" />
    <q-dialog v-model="preguntaNombreValors">
      <q-card>
        <q-card-section>
          <q-input v-model="nCells" label="Numero de valores: (max 2)" />
        </q-card-section>
        <q-card-actions>
          <q-btn label="Cancelar" color="negative" @click="closeDialog" />
          <q-btn label="Agregar" color="positive" @click="manyCells" />
        </q-card-actions>
      </q-card>
    </q-dialog>
    <q-dialog v-model="dialogVisible">
      <q-card>
        <q-card-section>
          <q-input
            v-if="nCells == 1"
            v-model="newRow.value"
            label="Data value"
          />
          <div v-else>
            <q-input v-model="newRow.value1" label="Data value 1" />
            <q-input v-model="newRow.value2" label="Data value 2" />
          </div>
        </q-card-section>
        <q-card-actions>
          <q-btn label="Cancelar" color="negative" @click="closeDialog" />
          <q-btn label="Aceptar" color="positive" @click="addCustomRow" />
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
              title="Nuevo nombre"
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
          <q-td class="custom-row" key="value" :props="props">
            <template v-if="typeof props.row.value === 'object'">
              <q-tr>
                <q-td v-for="(val, key) in props.row.value" :key="key">
                  {{ `${key}: ${val}` }}
                </q-td>
              </q-tr>
            </template>
            <template v-else>
              {{ props.row.value }}
            </template>
            <q-popup-edit
              v-model="props.row.value"
              title="Nuevo valor"
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
<style scoped>
.custom-row {
  display: flex;
  align-items: center;
  justify-content: center;
}
</style>
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
    value: { A: 32, B: 54 },
  },
  {
    label: "value_3",
    value: 6343,
  },
];

export default {
  data() {
    return {
      preguntaNombreValors: false,
      dialogVisible: false,
      loading: false,
      filter: "",
      data,
      columns,
      nCells: 0,
      newRow: { label: 3, value: "" },
    };
  },
  methods: {
    openDialog() {
      this.preguntaNombreValors = true;
    },
    closeDialog() {
      this.preguntaNombreValors = false;
      this.dialogVisible = false;
    },
    manyCells() {
      this.dialogVisible = true;
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
