<template>
  <q-page>
    <div class="q-pa-md">
      <div class="q-pa-md">
        <div
          class="row justify-between text-bold text-center text-white q-pa-xs bg-primary"
          style="border: 1px solid #000"
        >
          <div class="col-4 q-pa-xs" style="border-right: 1px solid #000">
            Field Label
          </div>
          <div class="col-8 q-pa-xs">Data Value</div>
        </div>

        <div
          v-for="(item, index) in tableData"
          :key="index"
          class="row text-center"
          style="border: 1px solid #000"
        >
          <div class="col-4" style="border-right: 1px solid #000">
            <input v-model="item.fieldLabel" type="text" />
          </div>
          <div class="col-8">
            <!-- Campo editable -->
            <input  v-if="item.dataValue[0] !== undefined" v-model="item.dataValue[0]" />
            <input v-if="item.dataValue[1] !== undefined" v-model="item.dataValue[1]" />
          </div>
        </div>

        <div class="row text-center">
          <div class="col-12" style="border: 1px solid #000">
            <q-btn class="q-ma-xs" label="Add Row" @click="openDialog" />
          </div>
        </div>
        <div class="row text-right">
          <div class="col-12 q-mt-xs">
            <TrainInfo :tableData="tableData" />
          </div>
          
       
        </div>
    
        <!-- MODAL O Q-DIALOG -->
        <q-dialog v-model="dialog">
              <q-card class="q-ma-md">
                <div class="text-h5 q-ma-md">Selecciona la cantidad de valores para Data Value:</div>
                <div class="flex flex-center">
                  <q-radio v-model="selectedOption" val="one" label="Uno valor" />
                  <q-radio v-model="selectedOption" val="two" label="Dos valores" />
                </div>
        
                <div class="row justify-center q-pa-md">
                  <q-btn
                    class="q-ma-xs"
                    label="Aceptar"
                    color="primary"
                    @click="addRowWithOption"
                    dense
                  />
                  <q-btn
                    class="q-ma-xs"
                    label="Cancelar"
                    color="negative"
                    @click="dialog = false"
                    dense
                  />
                </div>
              </q-card>
            </q-dialog>
      </div>
    
    </div>

  </q-page>
  
</template>



<script>
import TrainInfo from "src/components/TrainInfo.vue";

export default {
  name: "PageIndex",
  components: {
    TrainInfo,
  },
  data() {
    return {
      tableData: [
        { fieldLabel: "Value_1", dataValue: ["343"] },
        { fieldLabel: "Value_2", dataValue: ["A:32", "B:54"] },
        { fieldLabel: "Value_3 ", dataValue: ["6343"] },
      ],
      dialog: false, // Controla la visibilidad del dialog
      selectedOption: "one", // Por defecto (inicializada a "one")
    };
  },
  methods: {
    addRowWithOption() {
      if (this.selectedOption === "one") {
        this.tableData.push({
          fieldLabel: "Nuevo Campo",
          dataValue: ["Nuevo Valor"],
        });
      } else if (this.selectedOption === "two") {
        this.tableData.push({
          fieldLabel: "Nuevo Campo",
          dataValue: ["Nuevo Valor 1", "Nuevo Valor 2"],
        });
      }

      this.dialog = false; // Cierra el dialog después de agregar la fila
    },
    openDialog() {
      this.dialog = true;
    },
  },
};
</script>

<style></style>
