<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>Teampact360 Assessment</h1>
    </div>
    <button v-on:click="calculateExam" class="btn btn-primary btn-lg pull-right">Enviar Resultados</button>
    <div class="panel-heading" v-for="categoria in categorias">
      <h3 class="panel-title">{{categoria['.key']}}</h3>
      <div class="panel-body" v-for="competencia in filterByCompetencia(categoria)">
        <table class="table table-striped table-responsive" v-bind:class="formIdByCluster(competencia)">
          <thead>
            <tr>
              <th colspan="4">{{competencia.name}}</th>
            </tr>
            <tr>
              <th colspan="4">{{competencia.desc}}</th>
            </tr>
            <tr>
              <th>
              </th>
              <th>
              </th>
              <th>
                Aplicación
              </th>
              <th>
                Expertise
              </th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(cluster, key, index) in filterByCluster(competencia)">
              <td>
              </td>
              <td style="width: 85%;">
                {{ cluster }}
              </td>
              <td>
                <select class="form-control" v-bind:class="'a'+'_'+formIdByCluster(competencia)">
                  <option value=1>1</option>
                  <option value=2>2</option>
                  <option value=3>3</option>
                  <option value=4>4</option>
                  <option value=5>5</option>
                  <option value=6>6</option>
                </select>
              </td>
              <td>
                <select class="form-control" v-bind:class="'e'+'_'+formIdByCluster(competencia)">
                  <option value=1>1</option>
                  <option value=2>2</option>
                  <option value=3>3</option>
                  <option value=4>4</option>
                  <option value=5>5</option>
                  <option value=6>6</option>
                </select>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </div>
</template>

<script>
  import Firebase from 'firebase'
  import _ from 'underscore'

  let config = {
    apiKey: 'AIzaSyAuAqG-Zw7UANCBUeDc8zMzSeEASJCYdHk',
    authDomain: 'teampact360-test.firebaseapp.com',
    databaseURL: 'https://teampact360-test.firebaseio.com',
    projectId: 'teampact360-test',
    storageBucket: 'teampact360-test.appspot.com',
    messagingSenderId: '66366476103'
  }

  let app = Firebase.initializeApp(config)
  let db = app.database()
  let categoriasRef = db.ref('categorias')
  let usersRef = db.ref('usuarios')

  export default {
    name: 'app',
    firebase: {
      categorias: categoriasRef,
      users: usersRef
    },
    methods: {
      calculateExam: function (e) {
        let selects = Array.from(document.querySelectorAll('select'))
        let clusters = _.groupBy(selects, 'className')
        let cleanClusters = this.cleanClusters()
        let resultsAverageNotPaired = this.clustersArray(clusters)
        let resultPaired = this.resultsPaired(resultsAverageNotPaired)
        let fixedResults = this.resultsToFixedDecimals(resultPaired)
        let resultsMatched = this.matchResultsPaired(cleanClusters, fixedResults)
        console.log(resultsMatched)
        // Push data to firebase
        // usersRef.push(resultsAverage)
      },
      // Iterate over cluster results by class, get their value and return the average
      clustersArray: function (clusterObject) {
        let clusterResults = Object.values(clusterObject).map(select => select.reduce((total, select, index) => {
          let value = select.value
          return total + parseInt(value)
        }, 0) / (select.length || 1))
        return clusterResults
      },
      // Make pairs by 'Application and Expertise' per cluster
      resultsPaired: function (resultsArray) {
        let resultsPaired = resultsArray.reduce((result, value, index, array) => {
          if (index % 2 === 0) {
            result.push(array.slice(index, index + 2))
          }
          return result
        }, [])
        return resultsPaired
      },
      // Fix to 2 decimals the results of the test
      resultsToFixedDecimals: function (resultsPaired) {
        resultsPaired.forEach(result => {
          result.forEach((value, index) => {
            let valueFixed = parseFloat(value.toFixed(1))
            result[index] = valueFixed
          })
        })
        return resultsPaired
      },
      // Merge together the results with the cluster name
      matchResultsPaired: function (cleanClusters, resultsPaired) {
        let objectPaired = _.object(cleanClusters, resultsPaired)
        return objectPaired
      },
      cleanClusters: function () {
        let questions = Array.from(document.querySelectorAll('table'))
        // Group them by class
        let clusters = _.groupBy(questions, 'className')
        let cleanCluster = Object.keys(clusters).map((key, index) => {
          let splittedKeys = key.split(' ')
          return splittedKeys.slice(-1)[0]
        })
        return cleanCluster
      },
      filterByCluster: function (questionsArray) {
        let clusterArray = Object.keys(questionsArray).map((key, index) => {
          if (key !== ('name' || 'desc')) {
            return questionsArray[key]
          }
        })
        return clusterArray.filter(e => e)
      },
      filterByCompetencia: function (competencyArray) {
        delete competencyArray['.key']
        return competencyArray
      },
      formIdByCluster: function (questionsArray) {
        return questionsArray.name.replace(/ /g, '-').toLowerCase()
      }
    }
  }
</script>
