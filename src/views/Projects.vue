<template>
  <div class="projects">
    <h1 class="subtitle-1 grey--text mt-3">Articles</h1>
    <!-- <v-row justify="center"> -->
<!--    
      <v-expansion-panels class="mt-3" inset>
    <v-expansion-panel

      v-for="blog in  myProjects" :key="blog.id"

    >
      <v-expansion-panel-header>{{ blog.title }}</v-expansion-panel-header>
      <v-expansion-panel-content>
       <v-card flat>
            <v-card-text class="px-4 grey--text">
              <div class="font-weight-bold">due by {{ blog.due }}</div>
              <div>{{ blog.content }}</div>
            </v-card-text>
          </v-card>

      </v-expansion-panel-content>
    </v-expansion-panel>
  </v-expansion-panels> -->
     
    <v-container class="my-5">
   
  <v-card  hover v-for="blog in  myProjects" :key="blog.id" class="d-inline-block mx-4">
    <v-container>
      <v-row justify="space-between">
        <v-col cols="auto">
          <v-card-text >
      <div class = 'mb-4'>Post by {{ blog.person }}</div>
      <p class="display-1 text--primary">
       {{ blog.title.slice() }}
      </p>
      <!-- <span>Post by {{ blog.person }}</span> -->
      <div class="text--primary">
       {{ blog.content.slice() + '... ' }}
      </div>
    </v-card-text>
    <!-- <v-card-actions> -->
      <v-btn
        text
        color="deep-purple accent-4"
      >
        Read More
      </v-btn>
        </v-col>

        <v-col
          cols="auto"
          class="text-center pl-0"
        >
          <v-row
            class="flex-column ma-0 fill-height"
            justify="center"
          >
            <v-col class="px-0">
              <v-btn icon>
                <v-icon>mdi-heart</v-icon>
              </v-btn>
            </v-col>

            <v-col class="px-0">
              <v-btn @click='editblog' icon>
                <v-icon>mdi-pencil</v-icon>
              </v-btn>
            </v-col>

            <v-col class="px-0">
              <v-btn  @click="removeblog(blog.id)" icon>
                <v-icon>mdi-delete</v-icon>
              
              </v-btn>
            </v-col>
         
          </v-row>
        </v-col>
        
      </v-row>
     
    </v-container>
  </v-card>

 <v-dialog v-model="dialog" persistent max-width="600px">
      <template v-slot:activator="{ off }">
        <v-btn color="primary" dark v-on="on">Add a new article</v-btn>
      </template>
      <v-card>
        <v-card-title>
          <!-- <span class="headline">Add a new Blog</span> -->
        </v-card-title>
        <v-card-text>
          <v-container >

            <v-row >
              <v-col cols="12">
                <v-text-field ref="form" label="Title" v-model="title" :rules="inputruless" prepend-icon="mdi-folder" required></v-text-field>
              </v-col>
              <!-- <v-col cols="12" sm="6" md="4">
                <v-text-field label="Legal middle name" hint="example of helper text only on focus"></v-text-field>
              </v-col> -->
              <!-- <v-col cols="12" sm="6" md="4">
                <v-text-field
                  label="Legal last name*"
                  hint="example of persistent helper text"
                  persistent-hint
                  required
                ></v-text-field> -->
              <!-- </v-col> -->
              <v-col cols="12">
                <v-textarea ref="form" label="Article" :rules="inputrules" v-model="content" prepend-icon="mdi-pencil" required></v-textarea>
              </v-col> 
<v-col cols="12" lg="6">
        <v-menu
          v-model="menu1"
         
          :close-on-content-click="false"
          max-width="290"
        >
          <template v-slot:activator="{ on }">
            <v-text-field ref="form"
             :rules="daterules"
              :value="computedDateFormattedMomentjs"
              clearable
              label="Due date"
              readonly
              v-on="on"
              
            ></v-text-field>
          </template>
          <v-date-picker
           
            v-model="date"
            @change="menu1 = false"
          ></v-date-picker>
        </v-menu>
      </v-col>

      
              
             
              <!-- <v-col cols="12" sm="6">
                <v-select
                  :items="['0-17', '18-29', '30-54', '54+']"
                  label="Age*"
                  required
                ></v-select>
              </v-col>
              <v-col cols="12" sm="6">
                <v-autocomplete
                  :items="['Skiing', 'Ice hockey', 'Soccer', 'Basketball', 'Hockey', 'Reading', 'Writing', 'Coding', 'Basejump']"
                  label="Interests"
                  multiple
                ></v-autocomplete>
              </v-col> -->
            
            </v-row>
          </v-container>
          
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          
          <v-btn color="primary"  v-on:click="submit(); showAlert();" @click="dialog = false">Submit</v-btn>
          <v-btn color="blue darken-1" text @click="dialog = false">Cancel</v-btn>
          
        </v-card-actions>
      </v-card>
    </v-dialog>

    </v-container>

  </div>
</template>

<script>
// @ is an alias to /src
 
const axios = require('axios');
// import popup from '../components/popup'

export default {
  components:{
    // popup
  },
  name: 'home',
  data(){
    return {
       projects: [
        // { title:'Design a new Website', person: 'Prince', due: '28th December 2019', status:'Pending', content:'ABCD'},
        // { title:'Code up the Landing page', person: 'Chukwudire', due: '8th December 2019', status:'Published', content:'EFGH'},
        // { title:'Design Video Thumbnails', person: 'Alphajeez', due: '3rd December 2019', status:'Published', content:'IJKL'},
        // { title:'Design a new Website', person: 'Chimeremeze', due: '1st December 2019', status:'overdue', content:'MNOP'}
      ],

      blogs: [],
        
    }
  
   
  },


  methods: {
     
      editblog(id) {
       axios.put(`https://my-blog-4961b.firebaseio.com/users/${id}.json`, {

       })
      },
  
      removeblog(id){
        alert(id)
          axios.delete(`https://my-blog-4961b.firebaseio.com/users/${id}.json`)
          .then(response => {
            alert(response.data)
          })
          .catch(function (error) {
         alert(error);
  });
  this.updateblogs()
      },

       updateblogs(){
        axios.get( 'https://my-blog-4961b.firebaseio.com/users.json',  {
     
      })
     .then(response => {
    // console.log(response.data);
    const data = response.data
    // console.log(response.data)
    let users = [];
    for(let key in data){
      const user = data[key];
      user.id = key
      users.push(user)
    }
    this.blogs = users
    // console.log(this.blogs)
    // this.shortenedurl = 'https://rel.ink/' + response.data.hashid;
  })
  .catch(function (error) {
   alert(error);
  // Swal.fire('Oops...', 'Something went wrong!', 'error')
   
  });
      },
  },
     

  computed: {
    myProjects(){
      return this.blogs.filter(blog => {
       return  blog.person === 'Fiyin'
      })

    }
  },
  
   created() {
    this.updateblogs()
  },
  
}
</script>
