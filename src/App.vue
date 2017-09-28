<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>Teampact360 Assessment</h1>
    </div>
    <div class="panel-heading">
      <h3 class="panel-title">Preguntas</h3>
      <div class="panel-body">
        <table class="table table-striped table-responsive" v-for="question in questions" :key="question['.key']">
          <thead>
            <tr>
              <th colspan="4">{{question['.key']}}</th>
            </tr>
            <tr>
              <th></th>
              <th>Cluster</th>
              <th>Aplicación</th>
              <th>Teoria</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(cluster, key, index) in filterLastElement(question)" :key="cluster.key">
              <td>
                {{index + 1}}
              </td>
              <td style="width: 85%;">
                {{cluster}}
              </td>
              <td>
                <select>
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
                <select>
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
        <input id="saveForm" name="saveForm" type="submit" class="btn btn-primary btn-lg pull-right" value="Enviar Resultados">
      </div>
    </div>
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
  let questionsRef = db.ref('questions')

  export default {
    name: 'app',
    firebase: {
      questions: questionsRef
    },
    methods: {
      filterLastElement: function (questionsArray) {
        delete questionsArray['.key']
        return questionsArray
      }
    }
  }
</script>
