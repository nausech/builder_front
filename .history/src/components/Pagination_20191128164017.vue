<!-- <template>
  <div>
    <div class="row align--center">
      <div class="flex xs12 md12" v-if="searchable">
        <va-input v-model="querySearch" placeholder="Buscar cualquier cosa" v-debounce:1000ms="sendSearch">
          <va-icon name="fa fa-search" slot="prepend"/>
        </va-input>
      </div>

      <div class="flex xs12 md3 offset--md3 text-right">
        <template>
            <slot name="pagination_buttons"></slot>
        </template>
      </div>
    </div>

    <div class="markup-tables">
      <table class="va-table">
        <thead>
          <tr>
            <th class="text-left" v-for="col in cols" :key="col.id" v-bind:style="{width: col.width}">{{col.label}}</th>
            <th v-if="actions" class="text-right" style="width:20%">Acciones</th>
          </tr>
        </thead>

        <tbody>
          <tr v-for="item in rows" :key="item.id">
              <td v-for="col in cols" :key="col.id" class="text-left" v-bind:style="{width: col.width}">
                  <div @click="callfunction(item,col)" v-html="col.render ? col.render(collectFormatted(item,col),item) : collectFormatted(item,col,item)"></div>
              </td>

              <td v-if="actions" class="text-right" style="width:20%">

                  <va-button flat color="danger" icon="fa fa-trash" @click="deleteItem(item)" v-if="btnDelete"/>
                  <!-- <va-button flat color="success" icon="fa fa-pencil" :to="{ name: `${uri3}`, params: { id: item.id }}"/>    :hola='hola === false'-->

                 <!-- <template>
                      <slot :item="item" name="pagination_actions"/>

                  </template>
              </td>
          </tr>
        </tbody>
      </table>
      </div>
  </div>

</template> 

<script>
/*
import Vue from 'vue'
import vueDebounce from 'vue-debounce'
import VueSweetalert2 from 'vue-sweetalert2'

Vue.use(VueSweetalert2)
Vue.use(vueDebounce)

export default {
  props: {
    buttons: {
      type: Array,
      required: false,
    },
    searchable: {
      type: Boolean,
      default: true,
    },
    columns: {
      type: Array,
      required: true,
    },
    actions: {
      type: Boolean,
      default: true,
    },
    paginable: {
      type: Boolean,
      default: true,
    },
    uri: {
      type: String,
      required: true,
    },
    uri2: {
      type: String,
      required: false,
    },
    btnDelete: {
      type: Boolean,
      default: true,
    },
    uri3: {
      type: String,
      required: false,
    },
    met: {
      type: String,
      required: false,
    },
  },
  data () {
    return {
      querySearch: '',
      Page: 1,
      // ShowEntries: 10,
      pagination: {},
      rows: [],
      loading: false,
      term: null,
      showTopModal: false,
      withIcon: '',
      dataById: [],
      insert: {
        minimo: '',
        maximo: '',
        valor: '',
      },
    }
  },

  computed: {

    deleteRows () {
      return this.rows
    },
    pagesNumber () {
      if (!this.pagination.to) {
        return []
      }

      let from = this.pagination.current_page - this.offset
      if (from < 1) {
        from = 1
      }

      let to = from + (this.offset * 2)
      if (to >= this.pagination.last_page) {
        to = this.pagination.last_page
      }
      let pagesArray = []
      for (let page = from; page <= to; page++) {
        pagesArray.push(page)
      }
      return pagesArray
    },
    cols () {
      let app = this
      return app.columns.map(item => {
        if (item.type) item.typeDef = app.dataTypes[item.type]
        return item
      })
    },
  },

  created () {
    this.getRows()
    this.$bus.$on('updatePagination', () => {
      this.getRows()
    })
  },

  methods: {
    sendSearch () {
      this.Page = 1
      this.getRows()
    },

    collect (obj, field) {
      // utility function to get nested property
      function dig (obj, selector) {
        let result = obj
        const splitter = selector.split('.')
        for (let i = 0; i < splitter.length; i++) {
          if (typeof result === 'undefined') {
            return undefined
          }
          result = result[splitter[i]]
        }
        return result
      }

      if (typeof field === 'string') return dig(obj, field)
      return undefined
    },
    deleteItem (items) {
      this.$swal({
        title: 'Estas seguro de eliminar este registro ?',
        text: '¡No podrás revertir esto!!',
        type: 'question',
        customClass: 'bg-body',
        showCancelButton: true,
        confirmButtonClass: 'btn btn-success font-weight-light',
        cancelButtonClass: 'btn btn-danger font-weight-light',
        confirmButtonColor: '#53D190',
        cancelButtonColor: '#F65F6E',
        confirmButtonText: '¡Sí, bórralo!',
      }).then((result) => {
        if (result.value) {
          let urldelete = ``
          // console.log("hola:",items);
          switch (this.met) {
            case 'methodoly':
              urldelete = `${this.uri2}/${items.id}/${items.coordinadorfloors_id}`
              // console.log("baseurl:",urldelete)
              break
            case 'base':
              urldelete = `${this.uri2}/${items.id}/${items.coordinadorbasetables_id}`
              // console.log("baseurl:",urldelete)
              break
            case 'telercaderista':
              urldelete = `${this.uri2}/${items.user_id}/${items.campaign_id}`
              // console.log("baseurl:",urldelete)
              break
            case 'modifiers':
              if (items.grupo === 'Acelerador') {
                this.uri2 = 'accelerators'
              }
              urldelete = `${this.uri2}/${items.id}`
              console.log('baseurl:', urldelete)
              break

            default:
              console.log('Error ala enviar la accion que se va hacer ' + met + '.')
          }
          axios.delete(`${urldelete}`).then(response => {
            // this.rows = this.rows.filter(item => item.user_id != items.id)
		                this.$bus.$emit('updatePagination')
            this.$bus.$emit('getRowsCount')
            this.getRows()
            this.$swal({
              title: 'Borrado!',
              text: response.data.message,
              type: 'success',
              customClass: 'bg-body rounded-0',
              confirmButtonClass: 'btn btn-primary font-weight-light',
            })
            this.$bus.$emit('updatePagination')
          })
        }
      })
    },
    itemById (id) {
      this.showTopModal = true
      let urlItemById = `${this.uri3}/${id}`
      axios.get(urlItemById).then(responseById => {
        this.$bus.$emit('updatePagination')
        this.dataById = responseById.data['data']
      }).catch((error) => {
        console.log('Error in itemById:', error)
      })
    },

    updateItem (id) {
      let urlUpdate = `${this.uri3}/${id}`
      axios.put(urlUpdate, this.dataById).then((responseUpdate) => {
        let updatedData = responseUpdate.data['data']
        this.$bus.$emit('updatePagination')
        this.showTopModal = false
        this.$swal({
          title: 'Datos actualizados!',
          text: responseUpdate.data.message,
          type: 'success',
          customClass: 'bg-body rounded-0',
          confirmButtonClass: 'btn btn-primary font-weight-light',
        })
      }).catch((error) => {
        console.log('Error in updateItem:', error)
      })
    },
    collectFormatted (obj, column) {
      let value
      value = this.collect(obj, column.field)
      if (value === undefined) return ''
      // lets format the resultant data
      const type = column.typeDef
      if (typeof type !== 'undefined' && value != '') {
        return type.format(value)
      }
      return value
    },

    callfunction (item, col) {
      if (col.action) {
        col.action(item)
      }
    },

    getRows () {
      this.url = `${this.uri}?&search=${this.querySearch}`
      this.loading = true
      axios.get(this.url).then(response => {
        this.loading = false
        this.pagination = response.data.pagination
        this.rows = response.data.data
        this.$emit('rows', this.rows)
      })
    },
  },
} */
</script>
<style>
</style>-->
