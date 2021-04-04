<template>
  <b-container fluid="md" class="mt-5 mb-5">
    <b-row>
      <b-col md="12">
        <b-card class="shadow-md border-0 rounded-lg">
          <h5>DATA POSTS</h5>
          <hr>
          <b-button :to="{name: 'post-create'}" variant="primary" class="mb-3">TAMBAH</b-button>
          <b-table striped bordered hover :items="posts" :fields="fields" show-empty>
            <template v-slot:cell(actions)="row">
              <b-button :to="{name: 'post-edit-id', params: {id: row.item.id}}" variant="warning" size="sm">EDIT
              </b-button>
              <b-button variant="danger" size="sm" @click="deletePost(row)">DELETE</b-button>
            </template>
          </b-table>
        </b-card>
      </b-col>
    </b-row>
  </b-container>
</template>

<script>
  export default {

    data() {
      return {
        //header table  
        fields: ['title', 'content', 'actions'],
        //posts data
        posts: [],
      }
    },

    mounted() {

      //fething ke Rest API 
      this.$axios.get('/api/posts')
        .then(response => {

          //assign response ke state "posts"
          this.posts = response.data.data

        })
        .catch(error => {
          console.log(error.response.data)
        })
    },

    methods: {

       async deletePost(row) {
        
        //delete data post by ID
        await this.$axios.delete(`/api/posts/${row.item.id}`)
          .then(() => {

            //remove item array by index
            this.posts.splice(row.index, 1);

          })

      }
    }

  }
</script>

<style>

</style>