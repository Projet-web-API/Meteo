<template>
  <div>
    <div class="centre" id='detail_meteo' ></div>
    <div class='filtre' id='filtre_gris' v-on:click='clicSurFiltre'></div>
    <div class="row">
      <div class="col-12">
        <!--
        <card style="width: 17rem;">
          <div slot="header" >
            Meteo
          </div>

            <div id=jour0 v-on:click="affinfmeteo(0)" class='mini_cadre_jour'>
              <img id = "imeteo0"src="https://www.example.com/images/dinosaur.jpg" >
              <p id="meteo0"></p>
              <div id="infometeo0"></div>


            </div>
            <div id=jour1 v-on:click="affinfmeteo(1)" class='mini_cadre_jour'>
              <img id = "imeteo1"src="https://www.example.com/images/dinosaur.jpg" >
              <p id="meteo1"></p>
              <div id="infometeo1"></div>
            </div>


            <div id=jour2 v-on:click="affinfmeteo(2)" class='mini_cadre_jour'>
              <img id = "imeteo2"src="https://www.example.com/images/dinosaur.jpg" >
              <p id="meteo2"></p>
              <div id="infometeo2"></div>
            </div>


            <div id=jour3 v-on:click="affinfmeteo(3)" class='mini_cadre_jour'>
              <img id = "imeteo3"src="https://www.example.com/images/dinosaur.jpg" >
              <p id="meteo3"></p>
              <div id="infometeo3"></div>
            </div>


            <div id=jour4 v-on:click="affinfmeteo(4)" class='mini_cadre_jour'>
              <img id = "imeteo4"src="https://www.example.com/images/dinosaur.jpg" >
              <p id="meteo4"></p>
              <div id="infometeo4"></div>
          </div>

        </card>
        -->
        <!--
        <meteocard></meteocard>
        -->

      </div>
    </div>
    
  </div>

</template>


<script>
  import LineChart from '@/components/Charts/LineChart';
  import BarChart from '@/components/Charts/BarChart';
  import * as chartConfigs from '@/components/Charts/config';
  import TaskList from './Dashboard/TaskList';
  import UserTable from './Dashboard/UserTable';
  import config from '@/config';

  //import meteochart from '@/components/Charts/MeteoChart';

  export default {
    components: {
      LineChart,
      BarChart,
      TaskList,
      UserTable
    },
    data() {
      return {
        JsonMeteo : null,
        filtreOn : false,
        bigLineChart: {
          allData: [
            [100, 70, 90, 70, 85, 60, 75, 60, 90, 80, 110, 100],
            [80, 120, 105, 110, 95, 105, 90, 100, 80, 95, 70, 120],
            [60, 80, 65, 130, 80, 105, 90, 130, 70, 115, 60, 130]
          ],
          activeIndex: 0,
          chartData: null,
          extraOptions: chartConfigs.purpleChartOptions,
          gradientColors: config.colors.primaryGradient,
          gradientStops: [1, 0.4, 0],
          categories: []
        },
        purpleLineChart: {
          extraOptions: chartConfigs.purpleChartOptions,
          chartData: {
            labels: ['JUL', 'AUG', 'SEP', 'OCT', 'NOV', 'DEC'],
            datasets: [{
              label: "Data",
              fill: true,
              borderColor: config.colors.primary,
              borderWidth: 2,
              borderDash: [],
              borderDashOffset: 0.0,
              pointBackgroundColor: config.colors.primary,
              pointBorderColor: 'rgba(255,255,255,0)',
              pointHoverBackgroundColor: config.colors.primary,
              pointBorderWidth: 20,
              pointHoverRadius: 4,
              pointHoverBorderWidth: 15,
              pointRadius: 4,
              data: [80, 100, 70, 80, 120, 80],
            }]
          },
          gradientColors: config.colors.primaryGradient,
          gradientStops: [1, 0.2, 0],
        },
        greenLineChart: {
          extraOptions: chartConfigs.greenChartOptions,
          chartData: {
            labels: ['JUL', 'AUG', 'SEP', 'OCT', 'NOV'],
            datasets: [{
              label: "My First dataset",
              fill: true,
              borderColor: config.colors.danger,
              borderWidth: 2,
              borderDash: [],
              borderDashOffset: 0.0,
              pointBackgroundColor: config.colors.danger,
              pointBorderColor: 'rgba(255,255,255,0)',
              pointHoverBackgroundColor: config.colors.danger,
              pointBorderWidth: 20,
              pointHoverRadius: 4,
              pointHoverBorderWidth: 15,
              pointRadius: 4,
              data: [90, 27, 60, 12, 80],
            }]
          },
          gradientColors: ['rgba(66,134,121,0.15)', 'rgba(66,134,121,0.0)', 'rgba(66,134,121,0)'],
          gradientStops: [1, 0.4, 0],
        },
        blueBarChart: {
          extraOptions: chartConfigs.barChartOptions,
          chartData: {
            labels: ['USA', 'GER', 'AUS', 'UK', 'RO', 'BR'],
            datasets: [{
              label: "Countries",
              fill: true,
              borderColor: config.colors.info,
              borderWidth: 2,
              borderDash: [],
              borderDashOffset: 0.0,
              data: [53, 20, 10, 80, 100, 45],
            }]
          },
          gradientColors: config.colors.primaryGradient,
          gradientStops: [1, 0.4, 0],
        }
      }
    },
    computed: {
      enableRTL() {
        return this.$route.query.enableRTL;
      },
      isRTL() {
        return this.$rtl.isRTL;
      },
      bigLineChartCategories() {
        return this.$t('dashboard.chartCategories');
      }
    },
    methods: {
      initBigChart(index) {
        let chartData = {
          datasets: [{
            fill: true,
            borderColor: config.colors.primary,
            borderWidth: 2,
            borderDash: [],
            borderDashOffset: 0.0,
            pointBackgroundColor: config.colors.primary,
            pointBorderColor: 'rgba(255,255,255,0)',
            pointHoverBackgroundColor: config.colors.primary,
            pointBorderWidth: 20,
            pointHoverRadius: 4,
            pointHoverBorderWidth: 15,
            pointRadius: 4,
            data: this.bigLineChart.allData[index]
          }],
          labels: ['JAN', 'FEB', 'MAR', 'APR', 'MAY', 'JUN', 'JUL', 'AUG', 'SEP', 'OCT', 'NOV', 'DEC'],
        }
        this.$refs.bigChart.updateGradients(chartData);
        this.bigLineChart.chartData = chartData;
        this.bigLineChart.activeIndex = index;
      },
      httpGet(theUrl)
      {
          var xmlHttp = new XMLHttpRequest();
          xmlHttp.open( "GET", theUrl, false ); // false for synchronous request
          xmlHttp.send( null );
          return xmlHttp.responseText;
      },
      affinfmeteo : function(nb){
        if(this.filtreOn == false){
          if(this.JsonMeteo != null){
              var texte = "---------------------------------<br><span class='nom_jour'>"+this.JsonMeteo['fcst_day_'+ nb]['day_long']+"</span></br>---------------------------------</br>";
              texte +="</br>Min : " + this.JsonMeteo['fcst_day_'+ nb]['tmin'] + "°C  Max :" + this.JsonMeteo['fcst_day_'+ nb]['tmax'] + "°C</br>---------------------------------</br>";
              texte += "<table>";
              for(var i=0;i<24;i+=2){
                texte+="<tr><td>"+i+"H00</td><td><img src='" + this.JsonMeteo['fcst_day_'+nb]['hourly_data'][i+'H00']['ICON'] + "'></td><td>"+this.JsonMeteo['fcst_day_'+nb]['hourly_data'][i+'H00']['TMP2m']+"°C</td></tr>";

              }
              texte += "</table>";
              this.filtreOn=true

              document.getElementById('detail_meteo').innerHTML = texte +"<br>---------------------------------<br>";
              document.getElementById('filtre_gris').style.display = 'inline'
              document.getElementById('detail_meteo').style.display = 'inline'
          }


        }
      },
      clicSurFiltre : function(){
        if(this.filtreOn===true){
          this.filtreOn=false;
          document.getElementById('filtre_gris').style.display = 'none'
          document.getElementById('detail_meteo').style.display = 'none'
        }
      }

    },
    mounted() {
      //temporaire
      this.JsonMeteo = JSON.parse(this.httpGet("https://www.prevision-meteo.ch/services/json/lat=48.000lng=0.2"));

      //console.log(this.JsonMeteo);
      //estDeroule = [0,0,0,0,0]
      if(this.JsonMeteo != null){
          for(var i=0;i<5;i++){
              document.getElementById("meteo"+i).innerHTML = this.JsonMeteo['fcst_day_'+i]['day_short']/* + " : "+ json['fcst_day_'+i]['condition'] + " // Tmin : " + json['fcst_day_'+i]['tmin']+"°C Tmax : " + json['fcst_day_'+i]['tmax']+ "°C"*/;
              document.getElementById("imeteo"+i).setAttribute("src", this.JsonMeteo['fcst_day_'+i]['icon']);
          }
      }
      console.log("test");

      //
      this.i18n = this.$i18n;
      if (this.enableRTL) {
        this.i18n.locale = 'ar';
        this.$rtl.enableRTL();
      }
      this.initBigChart(0);
    },
    beforeDestroy() {
      if (this.$rtl.isRTL) {
        this.i18n.locale = 'en';
        this.$rtl.disableRTL();
      }
    }
  };
</script>
<style>
  div.centre {
      position:absolute;
      left: 50%;
      top: 15%;
      width: auto;
      height: auto;
      margin-left: -100px;
      margin-top: -100px;
      background-color: #FFFFFF;
      z-index: 100;
      display: none;
  }

  div.filtre{
    width: 100%;
    height: 100%;
    background-color: #000000;
    top: 0;
    left: 0;
    position: absolute;
    display: none;
    z-index: 20;
    -moz-opacity: 0.8;
    opacity: 0.6;
    filter: alpha(opacity=0);
  }

  span.nom_jour{
    text-align:center;
  }

  div.mini_cadre_jour{
    display: inline-block;
  }

</style>
