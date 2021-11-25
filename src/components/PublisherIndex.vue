<!-- BookIndex.vue -->
<template>
  <div class="row">
   <div style="margin-top: 5%">
     <h2>{{title}}</h2>
     <table><thead>
       <tr>
         <th>Publisher</th>
        <th>Country</th>
        <th>Founded</th>
        <th>Genere</th>
        <th class="text-center">Actions</th>
	   </tr>
       </thead><tbody>
       <tr v-for='pu in publishers'>
       <td>{{pu.publisher}}</td>
       <td>{{pu.country}}</td>
       <td>{{pu.founded}}</td>
       <td>{{pu.genere}}</td>
       <td>
       <router-link class="button"
         :to="'/publisher/show/'+pu._id">Show</router-link>
       &nbsp;
       <router-link class="button"
         :to="'/publisher/edit/'+pu._id">Edit</router-link>
       &nbsp;
       <a class="button"
         v-on:click="deletePublisher(pu._id)">Erase</a>
       </td>
       </tr></tbody>
     </table>
     <router-link class="button button-primary" 
       to="/publisher/create">New</router-link>
   </div>
  </div>
</template>

<script>
	
export default {
  name: "Publisher Index",
  data() {
    return {
      title: 'Publisher List',
      publishers: []
    };
  },
  mounted() {
    this.allPublisher()
  },
  methods: {
    allPublisher() {
      fetch(this.url+'/.netlify/functions/publisherFindAll',
        { headers: {'Accept': 'application/json'}})
        .then((response) => response.json())
        .then((items) => {
          this.publishers = items;
        })
     },
     deletePublisher(id) {
       fetch(this.url+'/.netlify/functions/publisherDelete/'+id,
         { headers: {'Content-Type': 'application/json'},
           method: 'DELETE'})
          .then((items) => {
            this.allBooks();
          }
        )
     }
  }
};
</script>