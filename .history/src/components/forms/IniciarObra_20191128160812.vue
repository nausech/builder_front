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
        </form>


        <!--//Table-->
        <va-card class="card-tab">
            <!--//Data table and button edit, delete-->
            <!-- <Pagination :uri="uri" :columns="fields"/> 
            <Pagination :searchable="false" :uri="uri" :uri2="uri2" :columns="fields" :uri3="uri3" :met="`${$t('methodoly')}`">
                <template v-slot:pagination_actions="{ item }">
                    <va-button flat color="info" icon="fa fa-pencil" @click="itemById(item.id)"/>
                </template>

            </Pagination> -->
            <!-- Button save-view -->
            <template>
                <div class="grid row">
                    <div class="flex md6">
                        <va-popover :message="`${$t('Selecciona para ver')}`" placement="top">
                            <va-button flat small color="gray" icon="fa fa-eye" />
                        </va-popover>
                    </div>
                    
                    <div class="flex md6 align-right">
                        <va-button color="info" @click="updateEstado">{{ $t('Guardar') }}</va-button>
                    </div>
                </div>
            </template>
        </va-card>
        <template>
            <va-modal v-model="showModal" position="top" :title=" $t('Actualizar metodología')" 
            :okText="$t('Actualizar')" :cancelText="$t('Cancelar')" @ok="updateItem(dataById.id)">

            <div class="flex md12 xs12">
                <div class="grid row" style="width:400px">
                <div class="flex md12">
                    <div class="grid row">
                    <div class="flex md4 mt-2">
                        <label> Valor mínimo:</label>
                    </div>
                    <div class="flex md8">
                        <va-input v-model="dataById.minimo">
                            <va-icon slot="prepend" color="gray" name="fa fa-dollar"/>
                        </va-input>
                    </div>
                    </div>
                </div>
                <div class="flex md12">
                    <div class="grid row">
                    <div class="flex md4 mt-2">
                        <label> Valor máximo:</label>
                    </div>
                    <div class="flex md8">
                        <va-input v-model="dataById.maximo">
                            <va-icon slot="prepend" color="gray" name="fa fa-dollar"/>
                        </va-input>
                    </div>
                    </div>
                </div>
                <div class="flex md12">
                    <div class="grid row">
                    <div class="flex md4 mt-2">
                        <label> Valor a pagar:</label>
                    </div>
                    <div class="flex md8">
                        <va-input v-model="dataById.valor">
                            <va-icon slot="prepend" color="gray" name="fa fa-dollar"/>
                        </va-input>
                    </div>
                    </div>
                </div>
                </div>
            </div>
            </va-modal>
        </template>

    </div>

</template>

<script scoped>
import Pagination from '@/components/Pagination'

export default {
    name: 'methodology',
    components: {
      Pagination
    },
    data() {
        return {
            uri:`coordinadorfloors/${sessionStorage.getItem('_current_user_id')}/${sessionStorage.getItem('_campanna_id')}`,
            uri2:'coordinadorfloors',
            uri3: 'confcoordinadorfloors',
            selectCitys: [],
            causalList: ['Neiva', 'Bogotá'],
            tipoList: [],
            selectTipo: [],
            disabledSelected: false,
            idtablefl:0,
            showModal: false,
            dataById: [],
            idtable:0,
            valorMinimo:0,
            insert: {
                minimo: null,
                maximo: null,
                valor: null,
                horas: '',
                orden: 0
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
                width: '10%'
            }, {
                field: 'products',
                label: this.$t('Productos de campaña'),
                width: '30%',
                render(h,data){
                    let dataArray=""
                    console.log("data",data)
                    for(var i=0;i<data.products.length;i++){
                        dataArray+= data.products[i]
                        if((i+1)<data.products.length)
                            dataArray=dataArray+", "
                    }
                    return dataArray;
                }
            }, {
                field: 'minimo',
                label: this.$t('Valor mínimo'),
                width: '10%'
            }, {
                field: 'maximo',
                label: this.$t('Valor máximo'),
                width: '10%'
            }, {
                field: 'valor',
                label: this.$t('Valor a pagar'),
                width: '10%'
            }]
        }
    },
    created(){
        this.getTables()
        this.getProducts()
        this.getHours();
        this.$bus.$on('getRowsCount',() => {
            this.getTables()
        })
    },
    methods:{
        onChanges(){
            console.log("hola mundo")
            for( var i=0; this.selectProduct.length>i; i++) {
                console.log(this.selectProduct[i].id)
            }
        },
        getProducts(){
            this.loading = true
            const urlProducts = `products-campaign/${sessionStorage.getItem('_campanna_id')}`

            axios.get(urlProducts).then((respArrayProduct) => {
                let lengthData = respArrayProduct.data['data']

                for (let i = 0; i < lengthData.length; i++) {
                    let dataProducts = lengthData[i].products[0]
                    this.productList.push(dataProducts)
                    console.log("productos",this.productList)
                    this.loading = false
                }
            }).catch((err) => {
                console.log('Error in method getProducts:', err)
                this.loading = false
            })
        },

        getHours() {
            this.loading = true

            let urlProfiles = `profiles`
  
            axios.get(urlProfiles).then((respArrayProfile) => {
                let resultDataProfile = respArrayProfile.data['data']
                this.profileList = resultDataProfile
                this.loading = false

            }).catch((err) => {
                console.log('Error in method getHours:', err)
                this.loading = false
            })
        },
        updateEstado(){
             console.log(`coordinadorfloors/${this.idtable}`)
            let urlUpdate = `coordinadorfloors/${this.idtable}`
            let insertTableupd=[]
            insertTableupd = {
                    
                estado: "CERRADO"
            }
            axios.put(urlUpdate,insertTableupd).then((res)=> {
                console.log("update of coordinadorfloors",res)
                let updatedData = res.data['data']
                this.$bus.$emit('updatePagination')
                this.showTopModal = false
                this.$swal({
                    title: 'Pisos guardados',
                    text: res.data.message,
                    type: 'success',
                    customClass: 'bg-body rounded-0',
                    confirmButtonClass: 'btn btn-primary font-weight-light',
                })
            })
        },
        getTables() {
            this.loading = true
            let urlTables = `coordinadorfloors/${sessionStorage.getItem('_current_user_id')}/${sessionStorage.getItem('_campanna_id')}`
            
            axios.get(urlTables).then((respArrayData) => {
                let data = respArrayData.data.data

                
                if (data === 0) {
                    this.orden = data
                    this.disabledSelected=false
                    this.loading = false
                }else {
                    this.orden = data.length
                    this.valorMinimo=data[data.length-1].maximo
                    this.disabledSelected = true
                    let product = data[0].products
                    console.log("idtable"+data[0].coordinadorfloors_id)
                    this.idtable=data[0].coordinadorfloors_id
                    this.selectProduct = product
                    let horaInsert = data[0].horas
                    this.insert.horas = horaInsert
                    this.loading = false
                }
             }).catch((err) => {
                console.log('Error in method getTable:', err)
                this.loading = false
            })
        },

        async insertItem() {
            let valordespues=parseInt(this.valorMinimo)
            let fminimo=parseInt(this.insert.minimo)
            let fmaximo=parseInt(this.insert.maximo)
            if(fmaximo>fminimo){
                if(fminimo>valordespues){
                    this.loading = true
                    let urlInsert = ``
                    let insertTable=[]
                    if(this.orden === 0){
                        urlInsert =`coordinadorfloors`
                        this.orden ++
                        let arrayProducts = []
                        for( var i=0; this.selectProduct.length>i; i++) {
                            arrayProducts.push(this.selectProduct[i].id)
                        }
                        
                        insertTable = {
                            minimo: this.insert.minimo,
                            maximo: this.insert.maximo,
                            valor: this.insert.valor,
                            horas: this.insert.horas.horas,
                            orden: this.orden,
                            estado: "ABIERTO",
                            user_id: sessionStorage.getItem('_current_user_id'),
                            confcampaign_id: sessionStorage.getItem('_campanna_id'),
                            array:arrayProducts
                        }
                        console.log("insertTable1",insertTable)
                    }else{
                        urlInsert =`confcoordinadorfloors`
                        let urlconsid=`coordinadorfloors-id/${sessionStorage.getItem('_current_user_id')}/${sessionStorage.getItem('_campanna_id')}`                
                        this.orden ++
                        let response = await axios.get(urlconsid)
                        response = response.data.data
                        this.idtablefl=response.id
                        insertTable = {
                            minimo: this.insert.minimo,
                            maximo: this.insert.maximo,
                            valor: this.insert.valor,
                            horas: this.insert.horas,
                            orden: this.orden++,
                            coordinadorfloors_id:this.idtablefl,
                            user_id: sessionStorage.getItem('_current_user_id')
                        }
                        console.log(insertTable)
                    }

                    axios.post(urlInsert, insertTable ).then((insertData) => {
                        
                        let dataInsert = insertData.data.data;
                            this.loading = false
                            this.$bus.$emit('updatePagination')
                            this.insert.minimo=""
                            this.insert.maximo=""
                            this.insert.valor=""
                            this.getTables()
                            this.$swal({
                                position: 'top',
                                toast: true,
                                type: 'success',
                                title: insertData.data.message,
                                showConfirmButton: false,
                                timer: 3000
                            })

                    }).catch((err) => {
                        console.log('Error en metodo Insert:', err);
                        this.loading = false
                    })
                }else{
                    this.$swal({
                        position: 'center',
                        toast: true,
                        type: 'warning',
                        title: 'Error valor minimo no puede ser menor, que el valor mayor del ultimo piso',
                        showConfirmButton: false,
                        timer: 5000
                    })
                }
            }else{
                this.$swal({
                    position: 'center',
                    toast: true,
                    type: 'warning',
                    title: 'Error el valor minimo no puede ser mayor al valor máximo.',
                    showConfirmButton: false,
                    timer: 4000
                })
            }
        },
        itemById(id) {
            this.showModal = true
            let urlItemById = `confcoordinadorfloors/${id}`
        
            axios.get(urlItemById).then(responseById => {
                this.$bus.$emit('updatePagination')
                this.dataById = responseById.data['data']
            }).catch((error) => {
                console.log('Error in itemById:', error);
            })
        },

        updateItem(id){
            let urlUpdate = `confcoordinadorfloors/${id}`
            axios.put(urlUpdate, this.dataById).then((responseUpdate) => {
                let updatedData = responseUpdate.data['data']
                this.$bus.$emit('updatePagination')
                this.showModal = false
                this.$swal({
                    title: 'Datos actualizados!',
                    text: responseUpdate.data.message,
                    type: 'success',
                    customClass: 'bg-body rounded-0',
                    confirmButtonClass: 'btn btn-primary font-weight-light',
                })

            }).catch((error) =>{
                console.log('Error in updateItem:', error);
            })
        },

        
    }
}
</script>

<style scoped>

    
</style>
