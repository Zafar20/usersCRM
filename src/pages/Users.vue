<template>
   <div class="wrapper-content wrapper-content--fixed">
    <section>
      <div class="container">
        <table>
          <thead>
            <tr>
              <th @click="sort('name')">Name ↓</th>
              <th @click="sort('username')">UserName ↓</th>
              <th @click="sort('email')">Email ↓</th>
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
        <p class="page">Страница {{ page.current }}  длина {{page.length}}</p>
      </div>
    </section>
    <!-- buttons -->
    <section>
      <div class="container">
          <div class="button-list">
            <button class="btn btnPrimary" @click="prevPage">←</button>
            <button class="btn btnPrimary" @click="nextPage">→</button>
          </div>
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
      currentSortDir: 'asc',
      page: {
        current: 1,
        length: 3
      }
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
        return 0    
      }).filter((row,index) => {
          let start = (this.page.current - 1) * this.page.length;
          let end = this.page.current  * this.page.length 
          if(index >= start && index < end  ) return true 
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
    },
    //Pagination
    prevPage() {
      if(this.page.current > 1) {
        this.page.current -= 1
      }
    },
    nextPage() {
       if((this.page.current * this.page.length) < this.users.length ) {
        this.page.current += 1
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


.button-list {
  gap: 30px;
  display: flex;
  justify-content: center;
  .btn {
    border-radius: 60px;

  }
}
.page {
  margin-top: 30px  ;
}

</style>