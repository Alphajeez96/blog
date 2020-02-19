<template>
  <div class="dashboard">
    <h1 class="subtitle-1 grey--text">Content Library</h1>
     <v-container class="my-5">
       <v-layout row class="mb-3">
         
          <v-tooltip top>
             <template v-slot:activator="{ on }">
             <v-btn @click="sortBy('title')" class="ml-4 text-lowercase" small color="grey"  v-on="on">
             <v-icon left small>mdi-folder</v-icon>
             By Title</v-btn>
             </template>
            <span>Sort Contents by Title</span>
          </v-tooltip>

          <v-tooltip top>
             <template v-slot:activator="{ on }">
             <v-btn @click="sortBy('person')" class="ml-4 text-lowercase" small color="grey"  v-on="on">
             <v-icon left small>mdi-account</v-icon>
             By Author</v-btn>
             </template>
            <span>Sort Contents by Author</span>
          </v-tooltip>
       </v-layout>

         <v-card flat class="pa-3" v-for="blog in blogs" :key="blog.id" >
           <v-layout row wrap :class="`pa-3 project ${blog.status}`">
             <v-flex xs6 sm4 md6>
               <div class="caption grey--text pl-2"> Title</div>
               <div class="pl-2"> {{ blog.title }} </div>
             </v-flex>
             <v-flex xs6 sm4 md2>
                <div class="caption grey--text">Author</div>
                <div>{{ blog.person }}</div>
             </v-flex>
             <v-flex xs6 sm4 md2>
                <div class="caption grey--text">Posted on</div>
                <div> {{ blog.due}}</div>
             </v-flex>
              <v-flex xs6 sm4 md2>
                <div class="right">
                 <v-chip small :class="`${blog.status} white--text caption my-2 mx-3`"> {{ blog.status }} </v-chip>
               </div>
             </v-flex>
           </v-layout>
           <v-divider></v-divider>
         </v-card>
    </v-container>
  </div>
</template>

<script>
// @ is an alias to /src
const axios = require('axios');

export default {
  name: 'home',
  data(){
    return {
      projects: [
        // { title:'Design a new Website', person: 'Prince', due: '28th December 2019', status:'Pending', content:'ABCD'},
        // { title:'Code up the Landing page', person: 'Chukwudire', due: '8th December 2019', status:'Published', content:'EFGH'},
        // { title:'Design Video Thumbnails', person: 'Alphajeez', due: '3rd December 2019', status:'Published', content:'IJKL'},
        // { title:'Design a new Website', person: 'Chimeremeze', due: '1st December 2019', status:'overdue', content:'MNOP'}
      ],
      blogs: []
    }
  },
  methods:{
    sortBy(prop){
      this.blogs.sort((a,b) => a[prop] < b[prop] ? -1 : 1)
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
  created() {
    
   this.updateblogs()

    
  }
}

</script>
<style>
.project.completed{
  border-left:4px solid #3cd1c2;
}
.project.ongoing{
  border-left:4px solid orange;
}
.project.overdue{
  border-left:4px solid tomato;
}
.v-chip.completed{
background:#3cd1c2;
}
.v-chip.ongoing{
  background:#ffaa2c;
}
.v-chip.overdue{
background:tomato;
}

  body{
    font-size: 12px;
  }

</style>