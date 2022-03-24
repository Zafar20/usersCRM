<template>
  <div class="wrapper">
    <header class="header">
      <div class="navbar">
        <div class="container">
          <div class="navbar-content">
            <div class="logo">Users CRM</div>
            <ul class="navbar-list">
              <li class="navbar-item" v-for="link in links" :key="link.title">
                <router-link class="navbar-link" :to="link.url">{{ link.title }}</router-link>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </header>
    <router-view></router-view>
    <footer class="footer"></footer>
  </div>
</template>

<script>
export default {
  data() {
    return {  
      links: [
        {title: 'Home', url: '/'},
        {title: 'Users', url: '/users'},
      ]
    }
  }
};
</script>

<style lang="scss">


.navbar-link {
  &.router-link-exact-active  {
      color: rgb(52, 52, 200);
  }
}


</style>
