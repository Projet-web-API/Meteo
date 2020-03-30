<template>
  <div>
    <div class="centre" id='detail_meteo' >Test</div>
    <div class='filtre' id='filtre_gris' v-on:click='clicSurFiltre'></div>
    <div class="row">
      <div class="col-lg-3" :class="{'text-right': isRTL}"> <!-- Meteo -->
      
        <card style="width: 17rem;">
          <div slot="header" >
            Meteo
          </div>

            <div id="jour0" v-on:click="affinfmeteo(0)" class='mini_cadre_jour'>
              <img id = "imeteo0"src="https://www.example.com/images/dinosaur.jpg" >
              <p id="meteo0"></p>
              <div id="infometeo0"></div>


            </div>
            <div id="jour1" v-on:click="affinfmeteo(1)" class='mini_cadre_jour'>
              <img id = "imeteo1"src="https://www.example.com/images/dinosaur.jpg" >
              <p id="meteo1"></p>
              <div id="infometeo1"></div>
            </div>


            <div id="jour2" v-on:click="affinfmeteo(2)" class='mini_cadre_jour'>
              <img id = "imeteo2"src="https://www.example.com/images/dinosaur.jpg" >
              <p id="meteo2"></p>
              <div id="infometeo2"></div>
            </div>


            <div id="jour3" v-on:click="affinfmeteo(3)" class='mini_cadre_jour'>
              <img id = "imeteo3"src="https://www.example.com/images/dinosaur.jpg" >
              <p id="meteo3"></p>
              <div id="infometeo3"></div>
            </div>


            <div id="jour4" v-on:click="affinfmeteo(4)" class='mini_cadre_jour'>
              <img id = "imeteo4"src="https://www.example.com/images/dinosaur.jpg" >
              <p id="meteo4"></p>
              <div id="infometeo4"></div>
          </div>

        </card>
      </div>
      <div class="col-lg-4" :class="{'text-right': isRTL}"> <!-- Twitter -->

        <card type="chart">
            <template slot="header">
              <h5 class="card-category">{{$t('dashboard.twitter')}}</h5>
                <twitter>
                  <a class="twitter-timeline" data-height="300" data-theme="dark" href="https://twitter.com/QuipoV/lists/vos-news-le-mans?ref_src=twsrc%5Etfw">A Twitter List by QuipoV</a>  
                </twitter>
            </template>
          </card>
        <!--
        <meteocard></meteocard>
        -->
      </div>
    
      <div class="col-lg-5" :class="{'text-right': isRTL}"> <!-- Blablacar -->
        <card style="width: 28rem;">
          <div id="choix1">Ville départ</div>
          <input type="text" id="villed" name="ville départ">
          <br/><br/>
          <div id="choix2"> Ville arrivé</div>
          <select name="ville arrivé" id="this.villea">
              <option name="Peu importe">Peu importe</option>
              <option name="Paris">Paris</option>
              <option name="Nantes">Nantes</option>
              <option name="Lyon">Lyon</option>
              <option name="Lille">Lille</option>
              <option name="Le Mans">Le Mans</option>
          </select>
          <img src="../../public/img/fleches1.png" alt="échanger ville départ et arrivée" height="16" width="16" v-on:click="inverseVille()" v-on:mouseover="animation1()" v-on:mouseout="animation2()" id="fleches">
          
          <p id="test">Testeu</p>
          <button id="bouton" type="button" v-on:click="choix()">Chercher</button>

        </card>
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
        Jsonlemans : null,
        filtreOn : false,
        villed : "Peu importe",
        villea : "Peu importe",
        theUrl : null,
        inverser : false,
        plus : new Array(),
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
              var texte = "<div class='texteinfo'><h3> Meteo de "+this.JsonMeteo['fcst_day_'+ nb]['day_long']+"</h3>";
              //var texte = " <card style='width: 17rem;'><div slot='header' >Meteo de " + this.JsonMeteo['fcst_day_'+ nb]['day_long'] + "</div>";
              texte +="Min : " + this.JsonMeteo['fcst_day_'+ nb]['tmin'] + "°C  Max :" + this.JsonMeteo['fcst_day_'+ nb]['tmax'] + "°C<br></br>";
              texte += "<table>";
              for(var i=0;i<24;i+=2){
                texte+="<tr><td>"+i+"H00</td><td><img src='" + this.JsonMeteo['fcst_day_'+nb]['hourly_data'][i+'H00']['ICON'] + "'></td><td>"+this.JsonMeteo['fcst_day_'+nb]['hourly_data'][i+'H00']['TMP2m']+"°C</td></tr>";

              }
              texte += "</table>";
              this.filtreOn=true

              document.getElementById('detail_meteo').innerHTML = texte +"<br></div>";
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
      },
      chargerMeteo(ville){
        this.JsonMeteo = JSON.parse(this.Jsonlemans);
        //console.log(this.JsonMeteo);
        //estDeroule = [0,0,0,0,0]
        if(this.JsonMeteo != null){
            for(var i=0;i<5;i++){
              document.getElementById("meteo"+i).innerHTML = this.JsonMeteo['fcst_day_'+i]['day_short']/* + " : "+ json['fcst_day_'+i]['condition'] + " // Tmin : " + json['fcst_day_'+i]['tmin']+"°C Tmax : " + json['fcst_day_'+i]['tmax']+ "°C"*/;
              document.getElementById("imeteo"+i).setAttribute("src", this.JsonMeteo['fcst_day_'+i]['icon']);
            }
        }
      console.log("test");
      },
      choix(){
          this.villed = document.getElementById("villed").value;
          this.villea = document.getElementById("villea").value;
          console.log("ville départ = "+this.villed+"ville arrivé = "+this.villea)
          if(this.villea == this.villed){
              alert("veuillez choisir deux villes différentes");
          }
          else if(this.inverser==true){
              if(this.villea == "Peu importe"){
                  lancea(this.villed);
              }
              else{
                  lance(this.villea, this.villed);
              }
          }
          else{
              if(this.villea == "Peu importe"){
                  lanced(this.villed);
              }
              else{
                  lance(this.villed, villea);
              }
          }
          
      },

      lance(villed, villea) {
          this.theURL = "https://public-api.blablacar.com/api/v2/trips?fn="+this.villed+"&tn="+this.villea+"&key=cZ1QJSVV5ruMIZiVshHT7jsYy6of6Uwv"
          search()
      },

      lanced(villed) {
          this.theURL = "https://public-api.blablacar.com/api/v2/trips?fn="+this.villed+"&key=cZ1QJSVV5ruMIZiVshHT7jsYy6of6Uwv"
          search()
      },

      lancea(villea){
          this.theURL = "https://public-api.blablacar.com/api/v2/trips?tn="+this.villea+"&key=cZ1QJSVV5ruMIZiVshHT7jsYy6of6Uwv"
          search()
      },


      logArrayElements(element, index, array) {
          array[index] = "<img src=\"plus1.png\" alt=\"plus d'infos sur le trajet\" id=\"plus"+index+"\" height=\"16\" width=\"16\" onclick=\"plusInfos("+index+")\" onmouseover=\"animationp1("+index+")\" onmouseout=\"animationp2("+index+")\" >"
          console.log("a[" + index + "] = " + array[index]);
      },

      search(){
          json = JSON.parse(httpGet(this.theURL))
          var text = ""
          var taille = json['trips'].length
          console.log(taille)
          
          var i = 0

          for(i = 0; i < taille; i++){
              this.plus.push(undefined)
          }

          this.plus.forEach(logArrayElements);
          
          for(i = 0; i < taille; i++){
              text += "<div id=\"texte"+i+"\"> Départ = " + json['trips'][i.toString()]['departure_place']['city_name']+",arrivée = " + json['trips'][i.toString()]['arrival_place']['city_name'] +"   " + this.plus[i] +"</div>"
              
          }
          document.getElementById("test").innerHTML = text
      },

      inverseVille(){
          if(this.inverser == false){
              this.inverser = true;
              document.getElementById("choix1").innerHTML="Ville arrivé"
              document.getElementById("choix2").innerHTML="Ville départ"
          }
          else{
              this.inverser = false;
              document.getElementById("choix1").innerHTML="Ville départ"
              document.getElementById("choix2").innerHTML="Ville arrivé"
          }
      },

      animation1(){
          document.getElementById("fleches").src=('fleches2.png')
      },

      animation2(){
          document.getElementById("fleches").src=('fleches1.png')
      },

      animationp1(i){
          temp = "plus"+i
          console.log(temp)
          document.getElementById(temp).src=('plus2.png');

      },

      animationp2(i){
          temp = "plus"+i
          console.log(temp)
          document.getElementById(temp).src=('plus1.png')
      },

      plusInfos(i){
          console.log("tu as cliqué sur le plus " + i)
          document.getElementById("texte"+i).innerHTML = "Départ à "+json['trips'][i]['departure_place']['city_name']+", "+json['trips'][i]['departure_place']['address']+" le "+json['trips'][i]['departure_date']+",arrivée = " + json['trips'][i]['arrival_place']['city_name']
      }
    },
    mounted() {
      this.Jsonlemans = require("../../meteo/meteo-lemans.json")
      //temporaire

      this.chargerMeteo("lemans")

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
  div.texteinfo{

    margin-left: auto;
    margin-top: auto;
    color: #c9c9bc;
  }
  div.centre {
      position:absolute;
      left: 50%;
      top: 15%;
      width: auto;
      height: auto;
      margin-left: -100px;
      margin-top: -100px;
      background-color: #27293d;
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
