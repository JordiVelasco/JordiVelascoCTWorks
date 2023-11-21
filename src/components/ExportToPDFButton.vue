<!-- ExportToPDFButton.vue -->
<template>
  <div>
    <q-btn @click="exportToPDF" label="Exportar a PDF" color="positive" />
  </div>
</template>

<script>
import pdfMake from 'pdfmake/build/pdfmake';
import pdfFonts from 'pdfmake/build/vfs_fonts';

pdfMake.vfs = pdfFonts.pdfMake.vfs;

export default {
  props: {
    data: {
      type: Array,
      required: true,
    },
  },
  methods: {
    exportToPDF() {
      // Asegúrate de que data sea un array antes de usar map
      if (Array.isArray(this.data)) {
        const content = this.data.map(row => [
          { text: row.label, style: 'tableHeader' },
          { text: row.value, style: 'tableData' },
        ]);

        const docDefinition = {
          content: [
            {
              text: 'Tabla de Datos',
              style: 'header',
            },
            {
              table: {
                headerRows: 1,
                widths: ['*', '*'],
                body: [
                  [{ text: 'Campo', style: 'tableHeader' }, { text: 'Valor', style: 'tableHeader' }],
                  ...content,
                ],
              },
            },
          ],
          styles: {
            header: {
              fontSize: 18,
              bold: true,
              margin: [0, 0, 0, 10],
            },
            tableHeader: {
              bold: true,
              fontSize: 13,
              color: 'black',
            },
            tableData: {
              fontSize: 12,
              color: 'black',
            },
          },
        };

        pdfMake.createPdf(docDefinition).open();
      } else {
        console.error('El prop "data" no es un array:', this.data);
      }
    },
  },
};
</script>
