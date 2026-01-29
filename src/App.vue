<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>
      <v-toolbar-title>校园轻社交+资源置换平台</v-toolbar-title>
      
      <v-spacer></v-spacer>
      
      <v-btn icon @click="toggleTheme">
        <v-icon>{{ themeIcon }}</v-icon>
      </v-btn>
      
      <v-btn icon>
        <v-icon>mdi-bell</v-icon>
      </v-btn>
      
      <v-menu offset-y>
        <template v-slot:activator="{ props }">
          <v-btn class="ml-2" variant="text" v-bind="props">
            <v-avatar size="32">
              <v-img src="https://cdn.vuetifyjs.com/images/john.jpg" alt="Avatar"></v-img>
            </v-avatar>
          </v-btn>
        </template>
        <v-list>
          <v-list-item @click="() => {}">
            <v-list-item-title>个人资料</v-list-item-title>
          </v-list-item>
          <v-list-item @click="() => {}">
            <v-list-item-title>设置</v-list-item-title>
          </v-list-item>
          <v-list-item @click="logout">
            <v-list-item-title>退出登录</v-list-item-title>
          </v-list-item>
        </v-list>
      </v-menu>
    </v-app-bar>

    <v-navigation-drawer v-model="drawer" temporary>
      <v-list nav dense>
        <v-list-item link to="/" exact>
          <v-list-item-action>
            <v-icon>mdi-home</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>首页</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        
        <v-list-item link to="/social" exact>
          <v-list-item-action>
            <v-icon>mdi-account-group</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>社交圈</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        
        <v-list-item link to="/exchange" exact>
          <v-list-item-action>
            <v-icon>mdi-swap-horizontal</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>资源置换</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        
        <v-list-item link to="/post" exact>
          <v-list-item-action>
            <v-icon>mdi-plus</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>发布动态</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
        
        <v-list-item link to="/profile" exact>
          <v-list-item-action>
            <v-icon>mdi-account</v-icon>
          </v-list-item-action>
          <v-list-item-content>
            <v-list-item-title>个人中心</v-list-item-title>
          </v-list-item-content>
        </v-list-item>
      </v-list>
    </v-navigation-drawer>

    <v-main>
      <router-view />
    </v-main>

    <v-footer app>
      <span>&copy; {{ new Date().getFullYear() }} 校园轻社交+资源置换平台</span>
    </v-footer>
  </v-app>
</template>

<script>
export default {
  name: 'App',
  
  data: () => ({
    drawer: null,
    darkTheme: false
  }),
  
  computed: {
    themeIcon() {
      return this.darkTheme ? 'mdi-weather-sunny' : 'mdi-weather-night';
    }
  },
  
  methods: {
    toggleTheme() {
      this.darkTheme = !this.darkTheme;
      this.$vuetify.theme.global.name = this.darkTheme ? 'dark' : 'light';
    },
    
    logout() {
      // 处理登出逻辑
      console.log('用户登出');
    }
  }
};
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
</style>