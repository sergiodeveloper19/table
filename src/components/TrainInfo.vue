<template>
  <div>
    <q-btn @click="exportToPDF">Export PDF</q-btn>
  </div>
</template>
<script>
import pdfMake from "pdfmake/build/pdfmake";
import pdfFonts from "pdfmake/build/vfs_fonts";
import ctImage from "src/assets/ct.png";

pdfMake.vfs = pdfFonts.pdfMake.vfs;

export default {
  props: {
    tableData: Array,
  },
  methods: {
    exportToPDF() {
      // Formatea la fecha actual con el formato deseado
      const currentDate = new Date();
      const day = currentDate.getDate();
      const monthNames = [
        "Enero",
        "Febrero",
        "Marzo",
        "Abril",
        "Mayo",
        "Junio",
        "Julio",
        "Agosto",
        "Septiembre",
        "Octubre",
        "Noviembre",
        "Diciembre",
      ];
      const month = monthNames[currentDate.getMonth()];
      const year = currentDate.getFullYear();

      const formattedDate = `Madrid, ${day} ${month} de ${year}`;

      const tableRows = this.tableData.map((item) => [
        item.fieldLabel,
        item.dataValue,
      ]);

      const documentDefinition = {
        header: {
          columns: [
            {
              image: ctImage,
              width: 100,
              height: 100,
            },
            {
              text: "Proceso de Selección Octubre 2023",
              fontSize: 16,
              alignment: "right",
              margin: [40, 10, 10, 0],
            },
          ],
        },

        content: [
          {
            margin: [80, 120, 0, 0],
            table: {
              headerRows: 1,
              widths: ["*", "*", "*"],
              body: [
                // Header background blue and text-white
                [
                  { text: "Field Label", fillColor: "black", color: "white" },
                  { text: "Data Value", fillColor: "black", color: "white" },
                ],
                // Content with background white and text black
                ...tableRows.map((row) => {
                  if (Array.isArray(row[1]) && row[1].length === 2) {
                    // Si row[1] es un arreglo con dos valores, mostrarlos en dos columnas
                    return [
                      { text: row[0], fillColor: "white", color: "black" },
                      [
                        {
                          text: row[1][0] + "    |    " + row[1][1],
                          fillColor: "white",
                          color: "black",
                          border: [false, true, false, true],
                        },
                      ],
                    ];
                  } else {
                    // Si row[1] no cumple con los requisitos, mostrarlo en una sola columna
                    return [
                      { text: row[0], fillColor: "white", color: "black" },
                      {
                        text: row[1] || "",
                        fillColor: "white",
                        color: "black",
                      },
                    ];
                  }
                }),
              ],
            },
            
          },
        ],

        footer: function () {
          return {
            stack: [
              {
                canvas: [
                  { type: "line", x1: 10, y1: 0, x2: 595 - 2 * 40, y2: 0 },
                ],
              },
              {
                columns: [
                  { text: "Pepito Grillo", alignment: "left" },
                  { text: formattedDate, alignment: "right" },
                ],
              },
            ],
            margin: [40, 0],
          };
        },
      };

      pdfMake.createPdf(documentDefinition).download("tableData.pdf");
    },
  },
};
</script>
