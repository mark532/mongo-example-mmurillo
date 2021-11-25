<!-- BookIndex.vue -->
<template>
  <div class="row">
   <div style="margin-top: 5%">
     <h2>{{title}}</h2>
     <table><thead>
       <tr>
         <th>Author</th>
        <th>Nationality</th>
        <th>Birth Year</th>
        <th>Fields</th>
        <th class="text-center">Actions</th>
	   </tr>
       </thead><tbody>
       <tr v-for='au in authors'>
       <td>{{au.author}}</td>
       <td>{{au.nationality}}</td>
       <td>{{au.birth_year}}</td>
       <td>{{au.fields}}</td>
       <td>
       <router-link class="button"
         :to="'/author/show/'+au._id">Show</router-link>
       &nbsp;
       <router-link class="button"
         :to="'/author/edit/'+au._id">Edit</router-link>
       &nbsp;
       <a class="button"
         v-on:click="deleteBook(au._id)">Erase</a>
       </td>
       </tr></tbody>
     </table>
     <router-link class="button button-primary" 
       to="/author/create">New</router-link>
   </div>
  </div>
</template>

<script>
	
export default {
  name: "Author Index",
  data() {
    return {
      title: 'Author List',
      authors: []
    };
  },
  mounted() {
    this.allAuthors()
  },
  methods: {
    allAuthors() {
      fetch(this.url+'/.netlify/functions/authorFindAll',
        { headers: {'Accept': 'application/json'}})
        .then((response) => response.json())
        .then((items) => {
          this.authors = items;
        })
     },
     deleteBook(id) {
       fetch(this.url+'/.netlify/functions/authorDelete/'+id,
         { headers: {'Content-Type': 'application/json'},
           method: 'DELETE'})
          .then((items) => {
            this.allAuthors();
          }
        )
     }
  }
};
</script>