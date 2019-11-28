<template>
    <div class="flex md12">
        <!--//Forms Methodology-->
        <form action="POST" @submit.prevent="insertItem">
            <div class="row">
                <div class="flex md4 sm6 xs12">
                    <va-input
                    :label="$t('Nombre')" v-model="simple" placeholder="Nombre"/>
                </div>
                <div class="flex md4 sm6 xs12">
                    <va-input :label="$t('Dirección')" v-model="simple"  placeholder="Dirección" />
                </div>
                <div class="flex md4 sm6 xs12">
                     <va-select :label="$t('Ciudad')"  v-model="selectCitys" multiple textBy="Ciudad"
                    :options="cityList" id="cityList" :disabled="disabledSelected"/>
                </div>
                <div class="flex md4 sm6 xs12">
                    <va-input type="number"
                    :label="$t('Metros ^2')" v-model="simple" placeholder="Mts cuadrados"/>
                </div>
                <div class="flex md4 sm6 xs12">
                    <va-input ype="number" :label="$t('Pisos')"  v-model="simple"  placeholder="Pisos" />
                </div>
                <div class="flex md4 sm6 xs12">
                     <va-select :label="$t('Tipo de estructura')"  v-model="selectTipo" multiple textBy="Ciudad"
                    :options="tipoList" id="cityList" :disabled="disabledSelected"/>
                </div>
                <div class="flex md4 sm6 xs12">
                    <va-input type="number"
                    :label="$t('Cant. Habitaciones')" v-model="simple" placeholder="Habitaciones"/>
                </div>
                <div class="flex md4 sm6 xs12">
                    <va-input ype="number" :label="$t('Cant. Cocinas')"  v-model="simple"  placeholder="Cocinas" />
                </div>
                <div class="flex md4 sm6 xs12">
                    <va-input ype="number" :label="$t('Cant. baños')"  v-model="simple"  placeholder="Baños" />
                </div>
            </div>
            <div class="flex md12 align-right">
                <va-button color="success">{{ $t('Guardar') }}</va-button>
            </div>
        </form>
    </div>

</template>

<script scoped>
// import Pagination from '@/components/Pagination'

export default {
  name: 'methodology',
  components: {
    // Pagination,
  },
  data () {
    return {
      uri: `coordinadorfloors/${sessionStorage.getItem('_current_user_id')}/${sessionStorage.getItem('_campanna_id')}`,
      uri2: 'coordinadorfloors',
      uri3: 'confcoordinadorfloors',
      selectCitys: [],
      causalList: ['Neiva', 'Bogotá'],
      tipoList: [],
      selectTipo: [],
      disabledSelected: false,
      idtablefl: 0,
      showModal: false,
      dataById: [],
      idtable: 0,
      valorMinimo: 0,
      insert: {
        minimo: null,
        maximo: null,
        valor: null,
        horas: '',
        orden: 0,
      },
    }
  },
  computed: {
    fields () {
      return [{
        name: '__slot:select',
      }, {
        field: 'orden',
        label: this.$t('Pisos'),
        width: '10%',
      }, {
        field: 'horas',
        label: this.$t('Int. de horas'),
        width: '10%',
      }, {
        field: 'products',
        label: this.$t('Productos de campaña'),
        width: '30%',
        render (h, data) {
          let dataArray = ''
          console.log('data', data)
          for (var i = 0; i < data.products.length; i++) {
            dataArray += data.products[i]
            if ((i + 1) < data.products.length) { dataArray = dataArray + ', ' }
          }
          return dataArray
        },
      }, {
        field: 'minimo',
        label: this.$t('Valor mínimo'),
        width: '10%',
      }, {
        field: 'maximo',
        label: this.$t('Valor máximo'),
        width: '10%',
      }, {
        field: 'valor',
        label: this.$t('Valor a pagar'),
        width: '10%',
      }]
    },
  },
  created () {

  },
  methods: {

  },
}
</script>

<style scoped>

</style>
