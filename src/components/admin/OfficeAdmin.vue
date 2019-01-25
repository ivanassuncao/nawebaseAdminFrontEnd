<template>
    <div class="office-admin">
         <b-form>
             <input id="office-id" type="hidden" v-model="office.id" />
             <b-row>
                <b-col md="6" sm="12">
                    <b-form-group label="Nome:" label-for="office-name">
                        <b-form-input id="office-name" type="text" ref="searchFocus"
                            v-model="office.name" required
                            size="sm"
                            :readonly="mode === 'remove' || mode === 'read' || mode === 'insert'"
                            placeholder="Nome do Cargo/Função..." />
                    </b-form-group>
                     <b-form-group label="Decrição do Cargo/Função:" label-for="office-description">
                        <b-form-textarea id="office-acessoMenu"
                            :rows="3"
                            :max-rows="6"
                            size="sm"
                            v-model="office.description"
                            :readonly="mode === 'remove' || mode === 'read' || mode === 'insert'"
                            placeholder="" />
                    </b-form-group>
                </b-col>

             </b-row>
                <hr>
              <b-row >
  
                <b-col xs="12">
                    <b-button  size="sm" variant="outline-success" class="mr-2" v-show="mode === 'insert'"
                        @click="alterModo">Adicionar</b-button>
                    <b-button  size="sm" variant="outline-primary" v-if="mode === 'save'"
                        @click="save">Salvar</b-button>
                    <b-button  size="sm" variant="danger" v-if="mode === 'remove'"
                        @click="remove">Excluir</b-button>
                    <b-button  size="sm" btn-sm variant="outline-secondary" class="ml-2"  v-if="mode === 'remove' || mode === 'save' "
                    @click="reset">Cancelar</b-button>
                </b-col>
            </b-row>
         </b-form>
              <hr> 
                <div class="col-sm-4">
                    <div class="form-group">
                        <label class="control-label"> <i class="fa fa-search"></i>  Pesquisar os Cargos/Funções</label>
                        <input type="text" icon="search" v-model="search" placeholder="Informe o nome do Cargo/Função" class="form-control">
                    </div>
                </div>
            <hr>
         <b-table class="table-responsive" hover striped :items="filteredList" :fields="fields">
               <template slot="actions" slot-scope="data">
                    <b-button  size="sm" variant="outline-warning" @click="loadOffice(data.item)" class="mr-2 mt-2">
                        <i class="fa fa-pencil"></i>
                    </b-button>
                     <b-button  size="sm" variant="outline-danger" @click="loadOffice(data.item, 'remove')" class="mr-2 mt-2">
                        <i class="fa fa-trash"></i>
                    </b-button>
                </template>   
         </b-table>    
    </div>
</template>

<script>
import { baseApiUrl, showError } from '@/global'
import axios from 'axios'

export default {
    name: 'OfficeAdmin',
    data: function() {
        return {
            mode: 'insert',
            search: '',
            office: {},
            offices: [],
            fields: [
                { key: 'id', label: 'Código', sortable: true },
                { key: 'name', label: 'Nome', sortable: true },
                { key: 'description', label: 'Descrição', sortable: false },
                { key: 'actions', label: 'Ações' }
            ]
        }
    },
     computed: {
        filteredList() {
        return this.offices.filter(office => {
        return office.name.toLowerCase().includes(this.search.toLowerCase())
            })
        }
    },
    methods: {
        loadOffices() {
            const url = `${baseApiUrl}/offices`
            axios.get(url).then(res => {
                this.offices = res.data
            })
        },
        reset() {
            this.mode = 'insert'
            this.loadOffices()
        },
        alterModo() {
                    this.mode = 'save'
                    this.office = {}
                    this.$refs.searchFocus.focus();
            },
        save() {
            const method = this.office.id ? 'put' : 'post'
            const id = this.office.id ? `/${this.office.id}` : ''
            axios[method](`${baseApiUrl}/offices${id}`, this.office)
                .then(() => {
                    this.$toasted.global.defaultSuccess()
                    this.reset()
                })
                .catch(showError)
        },
        remove() {
            const id = this.office.id
            axios.delete(`${baseApiUrl}/offices/${id}`)
                .then(() => {
                    this.$toasted.global.defaultSuccess()
                    this.reset()
                })
                .catch(showError)
        },
        loadOffice(office, mode = 'save') {
            this.mode = mode
            this.office = { ...office }
            this.$refs.searchFocus.focus();
        }
    },
    mounted() {
        this.loadOffices()
    }
}
</script>

<style>


    .office-admin select{
        font-size: 0.8rem;
    }
    .office-admin input{
        font-size: 0.8rem;
    }
    .office-admin button{
        font-size: 0.8rem;
    }

</style>
