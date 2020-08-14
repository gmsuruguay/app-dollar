<template>
  <v-row>
    <v-col xs="12">
      <v-card>
        <v-date-picker 
        full-width
        locale="es-AR"
        :min = "minimo"
        :max = "maximo"
        @change="getDolar(fecha)"
        v-model="fecha"></v-date-picker>
      </v-card>
      <v-card color="#26c6da" dark>
        <v-card-text class="display-1 text-center">
           {{valor}}
        </v-card-text>
      </v-card>
      
    </v-col>
  </v-row>
</template>

<script>
// @ is an alias to /src
import axios from "axios";

export default {
  data(){
    return {
      fecha : new Date().toISOString().substring(0,10),
      minimo : '2000',
      maximo : new Date().toISOString().substring(0,10),
      valor : null
    }
  },
  methods:{
    async getDolar(dia){
      let arrayFecha = dia.split("-")
      let newFecha = arrayFecha[2]+'-'+arrayFecha[1]+'-'+arrayFecha[0]
      try {        
        let datos = await axios.get(`https://mindicador.cl/api/dolar/${newFecha}`)
        
        if (datos.data.serie.length > 0) {
          this.valor = await datos.data.serie[0].valor
          
        } else {
          this.valor = "Sin resultado"
        }
      } catch (error) {
        console.log(error)
      }
    }
  },
  created(){    
    this.getDolar(this.fecha)
  }
}
</script>