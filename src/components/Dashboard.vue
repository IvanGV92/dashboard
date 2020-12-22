<template>
  <v-app id="inspire">
    <v-system-bar app>
      <v-spacer></v-spacer>

      <v-icon>mdi-square</v-icon>

      <v-icon>mdi-circle</v-icon>

      <v-icon>mdi-triangle</v-icon>
    </v-system-bar>

    <v-app-bar app>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>

      <v-toolbar-title>Gestion de contenidos</v-toolbar-title>
    </v-app-bar>

    <v-navigation-drawer
      v-model="drawer"
      fixed
      temporary
    >
      
  
    <v-card
      class="mx-auto"
      width="300"
    >
      <v-list>
        <v-list-item
        :to="{name: 'Home' }">
          <v-list-item-icon>
            <v-icon>mdi-home</v-icon>
          </v-list-item-icon>
  
          <v-list-item-title>Home</v-list-item-title>
        </v-list-item>

        <v-list-item
        :to="{name: 'Login' }">
          <v-list-item-icon>
            <v-icon>mdi-login</v-icon>
          </v-list-item-icon>
  
          <v-list-item-title>Login</v-list-item-title>
        </v-list-item>
  
        <v-list-group
          :value="true"
          prepend-icon="mdi-details"
        >
          
            <template v-slot:activator>
              <v-list-item-content>
                <v-list-item-title>Administración</v-list-item-title>
              </v-list-item-content>
            </template>
  
            <v-list-item
              v-for="([title, icon, ruta], i) in admins"
              :key="i"
              link
              :to="{name: ruta }"
            >
              <v-list-item-title v-text="title"></v-list-item-title>
  
              <v-list-item-icon>
                <v-icon v-text="icon"></v-icon>
              </v-list-item-icon>
            </v-list-item>
          </v-list-group>
  
          <v-list-group
            :value="true"
            prepend-icon="mdi-contacts"
          >
            <template v-slot:activator>
              <v-list-item-content>
                <v-list-item-title>Usuarios</v-list-item-title>
              </v-list-item-content>
            </template>
  
            <v-list-item
              v-for="([title, icon, ruta], i) in cruds"
              :key="i"
              link
              :to="{name: ruta }"
            >
              <v-list-item-title v-text="title"></v-list-item-title>
  
              <v-list-item-icon>
                <v-icon v-text="icon"></v-icon>
              </v-list-item-icon>
            </v-list-item>
          </v-list-group>
        
      </v-list>
    </v-card>

    </v-navigation-drawer>

    <v-main class="grey lighten-2">
      <v-container>
        <router-view/>
        </v-container>
    </v-main>
  </v-app>


</template>

<script>



export default {
  name: 'DashBoard',

  components: {
    

  },

  data: () => ({
  drawer: null,
   admins: [
      ['Categorias', 'mdi-format-list-text', 'Categoria'],
      ['Productos', 'mdi-format-list-bulleted-type','Producto'],
    ],
    cruds: [
      ['Usuarios', 'mdi-plus-outline','Usuario'],
    ],
  }),
};
</script>
