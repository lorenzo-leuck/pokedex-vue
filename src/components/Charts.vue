
<template>
  <div id="charts">

    <div>
     <Bar :chart-data="typesData" id="barchart"/>
</div>

      <div>

     <Line :chart-data="generationsData" id="linechart"/>
</div>

  </div>
</template>



<script>

import axios from 'axios';
import { Bar, Line } from 'vue-chartjs'
import { Chart as ChartJS,Title,Tooltip,Legend,BarElement,LineElement,LinearScale,PointElement,CategoryScale} from 'chart.js'

ChartJS.register(Title, Tooltip, Legend, BarElement,LineElement,PointElement, CategoryScale, LinearScale)


export default {
  name: "Pokedex",
  components: {Bar, Line},
  data:() => {
    return {

 typesData: {},
 generationsData: {}



    }
  },
  
async created(){

  var typesNamesData = await this.getTypeNames([])
  var typesCountsData = await this.getTypeCounts(typesNamesData,[])

  this.typesData =  
  { 
    labels: typesNamesData, 
    datasets: [ 
      { label: 'Pokémons por tipo', 
      backgroundColor: 'green', 
      data: typesCountsData 
      } 
    ]   
  }


  var GenerationNamesData = await this.getGenerationNames([])  
  var GenerationCountsData = await this.getGenerationCounts(GenerationNamesData,[]) 


  this.generationsData =  
  { 
    labels: GenerationNamesData, 
    datasets: [ 
      { label: 'Pokémons por geração', 
      backgroundColor: 'green', 
      data: GenerationCountsData
      } 
    ],
       
  }
},

methods: {

getTypeNames : async(arr) => {

let apiTypes =  await axios.get("https://pokeapi.co/api/v2/type")
apiTypes['data']['results'].forEach(element => {
  arr.push(element.name);
});
return arr;

},

getTypeCounts : async(types,arr) => {

  for (const type of types) {
    let apiCounts =  await axios.get(`https://pokeapi.co/api/v2/type/${type}`)
    // console.log(apiCounts['data']['pokemon'].length)
    arr.push(apiCounts['data']['pokemon'].length)
    // console.log(type)
  }
return arr;
},


getGenerationNames : async(arr) => {

let apiTypes =  await axios.get("https://pokeapi.co/api/v2/generation")
apiTypes['data']['results'].forEach(element => {
  arr.push(element.name);
});
return arr;

},

getGenerationCounts : async(generations,arr) => {

  for (const generation of generations) {
    let apiCounts =  await axios.get(`https://pokeapi.co/api/v2/generation/${generation}`)
    arr.push(apiCounts['data']['pokemon_species'].length)
  }
return arr;
}
  }

};
</script>
