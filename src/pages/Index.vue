<template>
  <q-page class="flex flex-center">
    <div>
      <h1>Hola Mundo con Quasar</h1>
      <p>Bienvenido a tu aplicación Quasar.</p>
      <q-btn @click="generarPDF">Generar PDF</q-btn>
    </div>
  </q-page>
</template>

<script>
import pdfMake from 'pdfmake/build/pdfmake';
import pdfFonts from 'pdfmake/build/vfs_fonts';

pdfMake.vfs = pdfFonts.pdfMake.vfs;

export default {
  name: 'PageIndex',
  methods: {
    generarPDF() {
      const documentDefinition = {
        content: [
          '¡Hola Mundo!',
          'Este es un documento PDF generado con pdfmake.',
        ],
      };

      pdfMake.createPdf(documentDefinition).open();
    },
  },
};
</script>
