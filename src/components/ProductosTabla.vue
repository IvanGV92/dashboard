<template>
 <div id="app">
  <v-app id="inspire">
    <v-data-table
      :headers="headers"
      :items="productos"
      sort-by="nombre"
      class="elevation-1"
    >
      <template v-slot:top>
        <v-toolbar
          flat
        >
          <v-toolbar-title>Productos</v-toolbar-title>
          <v-divider
            class="mx-4"
            inset
            vertical
          ></v-divider>
          <v-spacer></v-spacer>
          <v-dialog
            v-model="dialog"
            max-width="500px"
          >
            <template v-slot:activator="{ on, attrs }">
              <v-btn
                color="primary"
                dark
                class="mb-2"
                v-bind="attrs"
                v-on="on"
              >
                Nuevo Elemento
              </v-btn>
            </template>
            <v-card>
              <v-card-title>
                <span class="headline">{{ formTitle }}</span>
              </v-card-title>
  
              <v-card-text>
                <v-container>
                  <v-row>
                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        v-model="editedItem.codigo"
                        label="Codigo"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        v-model="editedItem.nombre"
                        label="Nombre"
                      ></v-text-field>
                    </v-col>
                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-select
                        v-model="editedItem.categoriaId"
                        label="Categoria"
                        :items="categorias"
                        item-text="nombre"
                        item-value="id"
                        return-object
                      ></v-select>
                    </v-col>
                    
                    

                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        v-model="editedItem.precio_venta"
                        label="Precio Venta"
                      ></v-text-field>
                    </v-col>

                      <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        v-model="editedItem.inventario"
                        label="Inventario"
                      ></v-text-field>
                    </v-col>
                   
                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-text-field
                        v-model="editedItem.estado"
                        label="Estado"
                      ></v-text-field>
                    </v-col>

                    <v-col
                      cols="12"
                      sm="6"
                      md="4"
                    >
                      <v-textarea
                        v-model="editedItem.descripcion"
                        label="Descripcion"
                        auto-grow
                        no-resize
                        shaped
                        counter="250"
                      ></v-textarea>
                    </v-col>
                    
                  </v-row>
                </v-container>
              </v-card-text>
  
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn
                  color="blue darken-1"
                  text
                  @click="close"
                >
                  Cancel
                </v-btn>
                <v-btn
                  color="blue darken-1"
                  text
                  @click="save"
                >
                  Save
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-dialog>
          <v-dialog v-model="dialogDelete" max-width="500px">
            <v-card>
              <v-card-title class="headline">Are you sure you want to delete this item?</v-card-title>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="blue darken-1" text @click="closeDelete">Cancel</v-btn>
                <v-btn color="blue darken-1" text @click="deleteItemConfirm">OK</v-btn>
                <v-spacer></v-spacer>
              </v-card-actions>
            </v-card>
          </v-dialog>
        </v-toolbar>
      </template>
      <template v-slot:item.actions="{ item }">
        <v-icon
          small
          class="mr-2"
          @click="editItem(item)"
        >
          mdi-pencil
        </v-icon>
        <v-icon
          small
          @click="deleteItem(item)"
        >
          mdi-delete
        </v-icon>
      </template>
      
    </v-data-table>
  </v-app>
</div>
</template>

<script>
import axios from 'axios';
  export default {
  
  data: () => ({
    dialog: false,
    dialogDelete: false,
    headers: [
      {
        text: 'Codigo',
        align: 'start',
        sortable: true,
        value: 'codigo',
      },
      { text: 'Nombre', value: 'nombre' },
      { text: 'Descripcion', value: 'descripcion' },
      { text: 'Categoría',value: 'categoria.nombre', sortable: true},
      { text: 'Precio Venta', value: 'precio_venta' },
      { text: 'Inventario', value: 'stock' },
      { text: 'Estado', value: 'estado' },
      { text: 'Acciones', value: 'actions',sortable:false },
      
      
      
    ],
   
    productos: [],
    categorias:[],
    categoria:'',
    editedIndex: -1,
    editedItem: {
      id:0,
      nombre: '',
      descripcion: '',
      precio_venta: 0,
      stock: 0,
      codigo:'',
      estado: 0,
      categoria: {
        id:0,
        nombre:'',
      }
      
    },
    defaultItem: {
      id:0,
      nombre: '',
      descripcion: '',
      precio_venta: 0,
      stock: 0,
      codigo:'',
      estado: 0,
      categoria: {
        id:0,
        nombre:'',
      }
      
      
    },
  }),

  computed: {
    formTitle () {
      return this.editedIndex === -1 ? 'Nuevo Producto' : 'Editar Producto'
    },
  },

  watch: {
    dialog (val) {
      val || this.close()
    },
    dialogDelete (val) {
      val || this.closeDelete()
    },
  },

  created () {
    this.list()
    this.listCategoria()
  },

  methods: {
   
    list(){
        axios.get('https://articulosback.herokuapp.com/api/articulo/list')
          .then( response=>{
            this.productos = response.data;
            console.log(this.productos);
          })
          .catch(err =>{
            console.log(err);
          })
    },
      listCategoria(){
        axios.get('https://articulosback.herokuapp.com/api/categoria/list')
          .then( response=>{
            this.categorias = response.data;
            console.log(this.productos);
          })
          .catch(err =>{
            console.log(err);
          })
    },

    editItem (item) {
      this.editedIndex = item.id
      this.categoria = item? item.categoria : '';
      this.editedItem = Object.assign({}, item)
      this.dialog = true
    },

    deleteItem (item) {
      this.editedIndex = item.id
      this.editedItem = Object.assign({}, item)
      this.dialogDelete = true
    },

    deleteItemConfirm () {
      
      this.closeDelete()
    },

    close () {
      this.dialog = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.categoria = ''
        this.editedIndex = -1
      })
    },

    closeDelete () {
      this.dialogDelete = false
      this.$nextTick(() => {
        this.editedItem = Object.assign({}, this.defaultItem)
        this.editedIndex = -1
      })
    },

     save () {
      if (this.editedIndex > -1) {
        //index ya existe y hay que actualizar registro - put
         axios.put('https://articulosback.herokuapp.com/api/articulo/update',{
          "id": this.editedItem.id,
          "codigo": this.editedItem.codigo,
          "nombre": this.editedItem.nombre,
          "descripcion": this.editedItem.descripcion,
          "precio_venta": this.editedItem.precio_venta,
          "stock": this.editedItem.stock,
          "estado": this.editedItem.estado,
          "categoriaId": this.editedItem.categoriaId,
           
           
         })
          .then( response =>{
            if(response.status == 200){
              this.list();
              console.log("lo logre");
            }
            
          })
          .catch(err =>{
            return err
          })

      } else {
        //index no existe y debemos crear registro - post
        axios.post('https://articulosback.herokuapp.com/api/articulo/add',{
          "codigo": this.editedItem.codigo,
          "nombre": this.editedItem.nombre,
          "descripcion": this.editedItem.descripcion,
          "precio_venta": this.editedItem.precio_venta,
          "stock": this.editedItem.stock,
          "estado": this.editedItem.estado,
          "categoriaId": this.editedItem.categoriaId,



           
         })
          .then( response =>{
            if(response.status == 200){
              this.list();
            }
            
          })
          .catch(err =>{
            return err
          })
      }
      this.close()
    },
  },

  }
</script>