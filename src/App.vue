<template>
  <v-app class="forimage">
    <v-app-bar app color="#37dacc">
      <v-toolbar-title >Eagle Financial Services</v-toolbar-title>
      <v-spacer></v-spacer>
      <div class="hidden-xs-only">
        <v-btn
          v-for="item in menu"
          :key="item.title"
          :to="item.url"
          flat
          text 
          rounded
          dark
        >{{ item.title }}</v-btn>
        <v-btn flat dark text rounded v-if="!authenticated"
                @click="login">Log in
        </v-btn>
        <v-btn flat dark text rounded v-if="!authenticated"
                @click="signup">Signup
        </v-btn>
        <v-btn flat dark text rounded v-if="authenticated"
                @click="logout">Log Out
        </v-btn>
      </div>
      <div class="hidden-sm-and-up">
      <v-menu bottom left>
        <template v-slot:activator="{ on, attrs }">
          <v-btn
            dark
            icon
            v-bind="attrs"
            v-on="on"
          >
            <v-icon>mdi-dots-vertical</v-icon>
          </v-btn>
        </template>
        <v-list>
          <v-list-item  v-for="item in menu"
            :key="item.title"
            :to="item.url"
          >{{ item.title }}</v-list-item>
          <v-list-item v-if="!authenticated"
          @click="login">Log in</v-list-item>
          <v-list-item v-if="!authenticated"
          @click="signup">Signup</v-list-item>
          <v-list-item v-if="authenticated"
          @click="logout">Log out</v-list-item>
        </v-list>
      </v-menu>
      </div>
    </v-app-bar>
    <v-content>
      <router-view/>
    </v-content>
  </v-app>

</template>

<script>
  import router from './router';
  import {APIService} from './http/APIService';
  const apiService = new APIService();

  export default {
    name: 'App',
    data: () => ({
      authenticated: false,
      dialog: false,
      menu: [
        { title: 'Home', url:"/"},
        { title: 'Customers', url:"/customer-list" },
        { title: 'Stocks', url:"/stock-list" },
        { title: 'Investments', url:"/investment-list" },
        { title: 'Mutual Funds', url:"/fund-list" },
      ]
    }),

    mounted() {
      apiService.getCustomerList().then(response => {
        this.authenticated = true;
      }).catch(error => {
        if (error.response.status === 401) {
          localStorage.removeItem('isAuthenticates');
          localStorage.removeItem('log_user');
          localStorage.removeItem('token');
          this.authenticated = false;
        }
      });
      console.log('this.authenticated--'+this.authenticated);
    },

    methods: {
      logout() {
        localStorage.removeItem('isAuthenticates');
        localStorage.removeItem('log_user');
        localStorage.removeItem('token');
        this.authenticated = false;
        // router.push('/');
        window.location = "/"
      },
      login() {
        router.push("/auth");
      },
      signup(){
        router.push("/signup");
      }
    }
  };

</script>
<style scoped>
   .forimage{
    background-image: url('https://mdbootstrap.com/img/Photos/Horizontal/Nature/full%20page/img(11).jpg');
    background-size: cover;
  }
</style>
<style>
footer {
  position: fixed;
  left: 0;
  bottom: 0;
  height: 4%;
  width: 100%;
  background-color: red;
  color:black;
  text-align: center;
}

</style>