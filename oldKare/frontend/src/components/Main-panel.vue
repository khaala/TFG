
<template>
<div >
  <div class="row">
    <div class="col-md-4">
      <login></login>
    </div>
    <div class="col-md-8">
      <table  id="myTable" width="100%">
        <thead>
          <tr>
            <th>Service List</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="servise in servises">
            <td>
              <div class="panel panel-default">
                <div class="panel-heading">
                  <h3 class="panel-title">{{ servise.username }} @ {{ servise.date | date-format}}</h3>
                </div>
                <div class="panel-title" >
                 Title: {{ servise.title }}
                </div>
                <div class="panel-body" >
                  {{ servise.description }}
                </div>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
  <div id="bottom"></div>
</div>
</template>

<script>
  import Vue from 'vue'
  import Login from './Login'

  Vue.filter('date-format', function (value) {
    let date = value.split('T')[0];
    let time = value.split('T')[1].split('+')[0].substring(0,5);
    return date+" "+time ;
  });

  

  export default {
        components: { Login },

        name: 'main-panel',
        data () {
          return {
            // msg : 'main-panel',
            servises : ''
          }
        }, 
        created: function () {
          this.getServises();
        },
        methods : {
          getServises () {
            this.$http.get('http://localhost:8081/servise').then((res) => {
              this.servises = res.body._embedded.servise.reverse();
              console.log(this.servises);
              $(document).ready(function() {
                $('#myTable').DataTable({
                  "bSort" : false,
                  "lengthMenu" : [[3,5,10,-1], [3,5,10,"ALL"]],
                  stateSave: true
                });
              });
            }, (err) => {
              console.log(err);
            });
          }
        }

      }
</script>

<style type="text/css">
      
      #bottom {
        padding-bottom: 50px;
      }
    </style>