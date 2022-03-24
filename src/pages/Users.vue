<template>
   <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">
        <table>
          <thead>
            <tr>
              <th @click="sort('name')">Name</th>
              <th @click="sort('username')">UserName</th>
              <th @click="sort('email')">Email</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="user in usersSort" :key="user.id">
              <td>
                <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcTUW0u5Eiiy3oM6wcpeEE6sXCzlh8G-tX1_Iw&usqp=CAU" alt="">
                <span>{{ user.name }}</span>
              </td>
              <td>{{ user.username }}</td>
              <td>{{ user.email }}</td>
            </tr>
          </tbody>
        </table>
        <p>debug:  sort: {{ currentSort }}, dir: {{ currentSortDir }}</p>
      </div>
    </section>
  </div>
</template>

<script>

import axios from 'axios'

export default {
  data() {
    return {
      users: [],
      currentSort: 'name',
      currentSortDir: 'asc'
    } 
  },
  created() {
      axios
        .get('https://jsonplaceholder.typicode.com/users')
        .then(response => {
          this.users = response.data
        })
        .catch((err) => {
          console.log(err);
        })
  },
  computed:{
    usersSort() {
      return this.users.sort((prev,next) => {
        let mod = 1
        if(this.currentSortDir === 'desc') mod = -1
        if(prev[this.currentSort] < next[this.currentSort] ) return -1 * mod      
        if(prev[this.currentSort] > next[this.currentSort] ) return 1 * mod   
        // return 0    
      })
    }
  },
  methods: {
    sort(e) {
      if(e === this.currentSort) {
        this.currentSortDir = this.currentSortDir === 'asc' ? 'desc' : 'asc';
      }else {
        this.currentSort = e    
      }
    }
  }
}
</script>

<style lang="scss" scoped>

img {
  width: 60px;
  height: auto;
  border-radius: 50%;
  margin-right: 16px;
}

</style>