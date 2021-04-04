<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>TAMBAH POST</h5>
          <hr>
          <b-form @submit="store">
            <b-form-group label="Title Post">
              <b-form-input type="text" v-model="post.title" :class="{ 'is-invalid': validation.title }"
                placeholder="masukkan title post">
              </b-form-input>
              <div v-if="validation.title" class="mt-2">
                <b-alert show variant="danger">{{ validation.title[0] }}</b-alert>
              </div>
            </b-form-group>
            <b-form-group label="Content Post">
              <b-form-textarea id="textarea" v-model="post.content" :class="{ 'is-invalid': validation.title }"
                placeholder="masukkan content post" rows="5">
              </b-form-textarea>
              <div v-if="validation.content" class="mt-2">
                <b-alert show variant="danger">{{ validation.content[0] }}</b-alert>
              </div>
            </b-form-group>
            <b-button type="submit" variant="primary">SIMPAN</b-button>
          </b-form>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
  export default {

    data() {
      return {
        //state post
        post: {
          title: '',
          content: ''
        },
        //state validation
        validation: []
      }
    },

    methods: {
      
      //method "store"
      async store(e) {
        e.preventDefault()

        //send data ke Rest API
        await this.$axios.post('/api/posts', {

            //data yang dikirim ke server
            title:   this.post.title,
            content: this.post.content
            
          })
          .then(() => {
            
            //redirect ke route "post"
            this.$router.push({
              name: 'post'
            })

          })
          .catch(error => {
            //assign validation  
            this.validation = error.response.data
          })
      }
    }

  }
</script>

<style>

</style>