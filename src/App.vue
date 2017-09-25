<template>
  <div id="app" class="container">
    <div class="page-header">
      <h1>Teampact360 Assessment</h1>
    </div>
    <div class="panel-heading">
      <h3 class="panel-title">Question List</h3>
      <div class="panel-body">
        <table class="table table-striped" v-for="question in questions">
            <thead>
              <tr>
                <th>{{question['.key']}}</th>
                <tr v-for="cluster in question">
                  {{cluster}}
                <tr>
              </tr>
            </thead>
            <tbody>
              <tr >
              </tr>
            </tbody>
        </table>
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
      }
    }
  </script>
