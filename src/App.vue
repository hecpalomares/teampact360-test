<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>Teampact360 Assessment</h1>
    </div>
    <div class="panel-heading" v-for="categoria in categorias">
      <h3 class="panel-title">{{categoria['.key']}}</h3>
      <div class="panel-body" v-for="competencia in filterByCompetencia(categoria)">
        <table class="table table-striped table-responsive">
          <thead>
            <tr>
              <th colspan="4">{{competencia.name}}</th>
            </tr>
            <tr>
              <th colspan="4">{{competencia.desc}}</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(cluster, key, index) in filterByCluster(competencia)">
              <td>
                {{ index + 1}}
              </td>
              <td style="width: 85%;">
                {{ cluster }}
              </td>
              <td>
                <select class="form-control">
                  <option value=" "></option>
                  <option value="1">1</option>
                  <option value="2">2</option>
                  <option value="3">3</option>
                  <option value="4">4</option>
                  <option value="5">5</option>
                  <option value="6">6</option>
                </select>
              </td>
              <td>
                <select class="form-control">
                  <option value=" "></option>
                  <option value="1">1</option>
                  <option value="2">2</option>
                  <option value="3">3</option>
                  <option value="4">4</option>
                  <option value="5">5</option>
                  <option value="6">6</option>
                </select>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <input id="sendForm" name="sendForm" type="submit" class="btn btn-primary btn-lg pull-right" value="Enviar Resultados">
  </div>
</template>

<script>
  import Firebase from 'firebase'
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

  export default {
    name: 'app',
    firebase: {
      categorias: categoriasRef
    },
    methods: {
      filterByCluster: function (questionsArray) {
        delete questionsArray.name
        delete questionsArray.desc
        return questionsArray
      },
      filterByCompetencia: function (competenciaArray) {
        delete competenciaArray['.key']
        return competenciaArray
      }
    }
  }
</script>
