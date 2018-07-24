<template>
  <form>
    <div class="form-group">
      <label for="servise"><h3>New Service:</h3></label><br>
      <input v-model='title' type="text" title="title" placeholder="Title"><br>
      <textarea v-model='servise' rows="5" class="form-control" id="servise" placeholder="Description"></textarea>
    </div>
    <a @click="post" class="btn btn-lg btn-primary">POST</a>
  </form>
</template>

<script>
  export default {

    name: 'post-servise',
    data () {
      return {
        servise : '',
        username : ''
      }
    },
    methods : {
      post () {
        let url = 'http://localhost:8081/servise';
        let info = {
          username : localStorage.getItem('username'),
          description : this.servise,
          date : new Date().setUTCHours(-4),

          title : this.title
        }

        this.$http.post(url, info).then((res) => {
          location.reload();
        }, (err) => {
          console.log(err);
        });
      }
    }
  }
</script>