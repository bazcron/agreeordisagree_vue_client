<template>
  <div class="hero">
    <div id="example-1">
      <button v-on:click="signInAsAdmin">Sign in as Admin</button>
    </div>
    <h3 class="vue-title"><i class="fa fa-question" style="padding: 3px"></i>{{messagetitle}}</h3>
    <div id="app1">
      <v-client-table :columns="columns" :data="statements" :options="options">
        <a slot="Agree" slot-scope="props" class="fa fa-thumbs-up fa-2x" style="color:green"  @click="agree(props.row._id)"></a>
        <a slot="Disagree" slot-scope="props" class="fa fa-thumbs-down fa-2x" style="color:red" @click="disagree(props.row._id)"></a>
        <a slot="Remove" slot-scope="props" class="fa fa-trash-o fa-2x" @click="deleteStatement(props.row._id)"></a>
        <a slot="Edit" slot-scope="props" class="fa fa-edit fa-2x" @click="editStatement(props.row._id)"></a>

      </v-client-table>
    </div>
  </div>
</template>

<script>

import QuizService from '@/services/theQuiz'
import Vue from 'vue'
import VueTables from 'vue-tables-2'

Vue.use(VueTables.ClientTable, {compileTemplates: true, filterByColumn: false, search: true}) //, perPage: 1

// var passwordCorrect = false
// var yes

/* if (this.passwordCorrect === 'true') {
  this.columns = ['statement', 'agree', 'disagree', 'Agree', 'Disagree', 'Edit', 'Remove']
} else {
  this.columns = ['statement', 'agree', 'disagree', 'Agree', 'Disagree']
} */

export default {
  name: 'Statements',
  // el: '#example-1',
  data () {
    return {
      passwordCorrect: 'false',
      counter: 0,
      messagetitle: ' The Survey ',
      statements: [],
      props: ['_id'],
      errors: [],
      // columns: yes,
      columns: ['statement', 'agree', 'disagree', 'Agree', 'Disagree'],
      options: {
        // perPage: 2,
        headings: {
          statement: 'Statement',
          agree: 'Agreed',
          disagree: 'Disagreed',
          edit: 'Edit',
          remove: 'Remove'
        }
      }
    }
  },
  // Fetches statements when the component is created.
  created () {
    this.loadStatements()
  },
  methods: {
    loadStatements: function () {
      QuizService.fetchStatements()
        .then(response => {
          // JSON responses are automatically parsed.
          this.statements = response.data
          console.log(this.statements)
        })
        .catch(error => {
          this.errors.push(error)
          console.log(error)
        })
    },
    agree: function (id) {
      QuizService.agreeWithStatement(id)
        .then(response => {
          this.loadStatements()
          console.log(response)
        })
        .catch(error => {
          this.errors.push(error)
          console.log(error)
        })
    },
    disagree: function (id) {
      QuizService.disagreeWithStatement(id)
        .then(response => {
          this.loadStatements()
          console.log(response)
        })
        .catch(error => {
          this.errors.push(error)
          console.log(error)
        })
    },
    editStatement: function (id) {
      this.$router.params = id
      this.$router.push('edit')
    },
    signInAsAdmin: function () {
      this.$swal({
        title: 'Enter Password',
        input: 'password',
        inputPlaceholder: 'Type password here',
        inputAttributes: {
          maxlength: 10,
          autocapitalize: 'off',
          autocorrect: 'off'
        },
        showCancelButton: true,
        confirmButtonText: 'Sign In',
        cancelButtonText: 'Cancel',
        showCloseButton: true
      }).then((password) => {
        console.log('SWAL Result : ' + password)
        if (password === 'password') {
          this.columns = ['statement', 'agree', 'disagree', 'Agree', 'Disagree', 'Edit', 'Remove']
          this.loadStatements()
        } else {
          this.columns = ['statement', 'agree', 'disagree', 'Agree', 'Disagree']
          this.loadStatements()
          this.$swal('Wrong Password', 'Please Try Again', 'info')
        }
      })
    },
    deleteStatement: function (id) {
      this.$swal({
        title: 'Delete? Are You Sure?',
        /* input: 'password',
        inputPlaceholder: 'Type password here',
        inputAttributes: {
          maxlength: 10,
          autocapitalize: 'off',
          autocorrect: 'off'
        }, */
        showCancelButton: true,
        confirmButtonText: 'OK Delete it',
        cancelButtonText: 'Cancel',
        showCloseButton: true
      }).then(() => {
        QuizService.deleteStatement(id)
          .then(response => {
            // JSON responses are automatically parsed.
            this.message = response.data
            console.log(this.message)
            this.loadStatements()
            // Vue.nextTick(() => this.$refs.vuetable.refresh())
            this.$swal('Deleted', 'You successfully deleted this Statement ' + JSON.stringify(response.data, null, 5), 'success')
          })
          .catch(error => {
            this.$swal('ERROR', 'Something went wrong trying to Delete ' + error, 'error')
            this.errors.push(error)
            console.log(error)
          })
      })
    }

    /* if (password === 'password') {
        Swal.fire(`Entered password: ${password}`)
      }
      this.$swal({
        title: 'Are you totaly sure?',
        text: 'You can\'t Undo this action',
        input: 'text',
        inputPlaceholder: 'Please Enter the Password',
        type: 'warning',
        showCancelButton: true,
        confirmButtonText: 'OK Delete it',
        cancelButtonText: 'Cancel',
        showCloseButton: true
        // showLoaderOnConfirm: true
      }).then((result) => {
        console.log('SWAL Result : ' + result)
        if (result === true && text === 'password') {
          QuizService.deleteStatement(id)
            .then(response => {
              // JSON responses are automatically parsed.
              this.message = response.data
              console.log(this.message)
              this.loadStatements()
              // Vue.nextTick(() => this.$refs.vuetable.refresh())
              this.$swal('Deleted', 'You successfully deleted this Statement ' + JSON.stringify(response.data, null, 5), 'success')
            })
            .catch(error => {
              this.$swal('ERROR', 'Something went wrong trying to Delete ' + error, 'error')
              this.errors.push(error)
              console.log(error)
            })
        } else {
          this.$swal('Cancelled', 'You changed your mind', 'info')
        }
      })
    } */
  }
}

</script>

<style scoped>
  .vue-title {
    margin-top: 30px;
    text-align: center;
    font-size: 45pt;
    margin-bottom: 10px;
  }
  #app1 {
    width: 80%;
    margin: 0 auto;
  }
  /*th:nth-child(1) {
    text-align: left;
  }*/
</style>
