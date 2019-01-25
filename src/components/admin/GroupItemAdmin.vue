<template>
    <div class="groupitem-admin">
         <b-form  v-show="mode === 'save' || mode === 'remove' || mode === 'read'">
             <input id="groupitem-id" type="hidden" v-model="groupitem.id" />
             <b-row>
                <b-col md="6" sm="12">
                    <b-form-group label="Nome:" label-for="groupitem-name">
                        <b-form-input id="groupitem-name" type="text" ref="searchFocus"
                            v-model="groupitem.name" required
                            size="sm"
                            :readonly="mode === 'remove' || mode === 'read' || mode === 'insert'"
                            placeholder="Nome do Grupo..." />
                    </b-form-group>
                </b-col>    
                <b-col md="3" sm="12">    
                     <b-form-group label="Código:" label-for="groupitem-internal_code_group_item">
                        <b-form-input id="groupitem-internal_code_group_item" type="text"
                            v-model="groupitem.internal_code_group_item" required
                            size="sm"
                            :readonly="mode === 'remove' || mode === 'read' || mode === 'insert'"
                            placeholder="Código..." />
                    </b-form-group>
                </b-col>
                <b-col md="3" sm="12">
                    <div class="form-group">
                        <div class="form-check">
                            <input class="form-check-input" v-model="groupitem.blocked" true-value="1" false-value="0" 
                                type="checkbox" 
                                id="groupitem-blocked"
                                :readonly="mode === 'remove' || mode === 'read' || mode === 'insert'"
                            >
                            <label class="form-check-label" for="groupitem-blocked">
                                Bloqueado?
                            </label>
                        </div>
                    </div>
                </b-col>  
             </b-row>
             <b-row>
                <b-col md="3" sm="12">
                    <div class="form-group">
                        <div class="form-check">
                            <input class="form-check-input" 
                                v-model="groupitem.synthetic" true-value="1" false-value="0" 
                                type="checkbox" 
                                id="groupitem-synthetic"
                                :readonly="mode === 'remove' || mode === 'read' || mode === 'insert'"
                            >
                            <label class="form-check-label" for="groupitem-synthetic">
                                Sintético?
                            </label>
                        </div>
                    </div>
                </b-col> 
                <b-col md="3" sm="12">
                    <div class="form-group" >
                        <div class="form-check">
                            <input class="form-check-input" 
                                v-model="groupitem.item_sale" true-value="1" false-value="0" 
                                type="checkbox" 
                                id="groupitem-item_sale"
                                :readonly="mode === 'remove' || mode === 'read' || mode === 'insert'"
                            >
                            <label class="form-check-label" for="groupitem-item_sale">
                                Grupo de Venda?
                            </label>
                        </div>
                    </div>
                </b-col>  
                <b-col md="3" sm="12">
                    <div class="form-group"  >
                        <div class="form-check">
                            <input class="form-check-input" 
                                v-model="groupitem.item_purchase" true-value="1" false-value="0" 
                                type="checkbox" 
                                id="groupitem-item_purchase"
                                :readonly="mode === 'remove' || mode === 'read' || mode === 'insert'"
                            >
                            <label class="form-check-label" for="groupitem-item_purchase">
                                Grupo de Compra?
                            </label>
                        </div>
                    </div>
                </b-col>  
                <b-col md="3" sm="12">
                    <div class="form-group" >
                        <div class="form-check">
                            <input class="form-check-input" 
                                v-model="groupitem.item_service" true-value="1" false-value="0" 
                                type="checkbox" 
                                id="groupitem-item_service"
                                :readonly="mode === 'remove' || mode === 'read' || mode === 'insert'"
                            >
                            <label class="form-check-label" for="groupitem-item_service">
                                Grupo de Serviço?
                            </label>
                        </div>
                    </div>
                </b-col>
            </b-row>
            <b-row>
                 <b-col md="3" sm="12">    
                     <b-form-group label="CNAE:" label-for="groupitem-cnae">
                        <b-form-input id="groupitem-cnae" type="text"
                            v-model="groupitem.cnae" 
                            size="sm"
                            :readonly="mode === 'remove' || mode === 'read' || mode === 'insert'"
                            placeholder="Código CNAE..." />
                    </b-form-group>
                </b-col>
            </b-row>
            <hr>
         </b-form>

              <b-row>
                <b-col xs="12">
                    <b-button size="sm" variant="outline-success" class="mr-2" v-show="mode !== 'save' && mode !== 'remove'"
                        @click="alterModo">Adicionar</b-button>
                    <b-button size="sm" variant="outline-primary" v-if="mode === 'save'"
                        @click="save">Salvar</b-button>
                    <b-button size="sm" variant="danger" v-if="mode === 'remove'"
                        @click="remove">Excluir</b-button>
                    <b-button size="sm" variant="outline-secondary" class="ml-2"  v-if="mode === 'remove' || mode === 'save' || mode === 'read' "
                    @click="reset">Cancelar</b-button>
                </b-col>
            </b-row>
              <hr>
                <div class="col-sm-4">
                    <div class="form-group">
                        <label class="control-label"> <i class="fa fa-search"></i>  Pesquisar os Grupos de Items</label>
                        <input type="text" icon="search" v-model="search" placeholder="Informe o nome do Grupo do Item" class="form-control">
                    </div>
                </div>
            <hr>
         <b-table class="table-responsive" hover striped :items="filteredList" :fields="fields">
               <template slot="actions" slot-scope="data">
                    <b-button size="sm" variant="outline-secondary" @click="loadGroupitem(data.item, 'read')" class="mr-2 mt-2">
                        <i class="fa fa-drivers-license-o"></i>
                    </b-button>
                    <b-button size="sm" variant="outline-warning" @click="loadGroupitem(data.item)" class="mr-2 mt-2">
                        <i class="fa fa-pencil"></i>
                    </b-button>
                    <b-button size="sm" variant="outline-danger" @click="loadGroupitem(data.item, 'remove')" class="mr-2 mt-2">
                        <i class="fa fa-trash"></i>
                    </b-button>
                    <b-button v-show="data.item.synthetic == 0"  size="sm" variant="outline-primary" @click="actionItem(data.item)" class="mr-2 mt-2">
                        <i class="fa fa-plus"></i>
                    </b-button>
                </template>   
         </b-table>    
    </div>
</template>

<script>
import { baseApiUrl, showError } from '@/global'
import axios from 'axios'
import ItemSearch from '../item/ItemSearch'

export default {
    name: 'GroupitemAdmin',
    components: {ItemSearch},
    data: function() {
        return {
            mode: 'insert',
            search: '',
            groupitem: {},
            groupitems: [],
            fields: [
                { key: 'id', label: 'Código', sortable: true },
                { key: 'name', label: 'Nome', sortable: true },
                { key: 'internal_code_group_item', label: 'Código Interno', sortable: false },
                { key: 'synthetic', label: 'Sintético', sortable: false,
                        formatter: value => value ? 'Sim' : 'Não' },
                { key: 'item_sale', label: 'Grupo de Venda', sortable: false,
                        formatter: value => value ? 'Sim' : 'Não' },    
                { key: 'item_purchase', label: 'Grupo de Compra', sortable: false,
                        formatter: value => value ? 'Sim' : 'Não' },     
                { key: 'item_service', label: 'Grupo de Serviço', sortable: false,
                        formatter: value => value ? 'Sim' : 'Não' },         
                { key: 'blocked', label: 'Bloqueado', sortable: false,
                        formatter: value => value ? 'Sim' : 'Não' },               
                { key: 'actions', label: 'Ações' }
            ]
        }
    },
     computed: {
        filteredList() {
        return this.groupitems.filter(groupitem => {
        return groupitem.name.toLowerCase().includes(this.search.toLowerCase())
            })
        }
    },
    methods: {
        loadGroupitems() {
            const url = `${baseApiUrl}/groupitems`
            axios.get(url).then(res => {
                this.groupitems = res.data
            })
        },
        reset() {
            this.mode = 'insert'
            this.loadGroupitems()
        },
        alterModo() {
                    this.mode = 'save'
                    this.groupitem = {}
                    this.$refs.searchFocus.focus();
            },
        actionItem(groupitem) {                   
            this.$router.push({
                    name: 'itemSearch',    
                    params: {id: groupitem.id}
                })
        },    
        save() {
            const method = this.groupitem.id ? 'put' : 'post'
            const id = this.groupitem.id ? `/${this.groupitem.id}` : ''
            axios[method](`${baseApiUrl}/groupitems${id}`, this.groupitem)
                .then(() => {
                    this.$toasted.global.defaultSuccess()
                    this.reset()
                })
                .catch(showError)
        },
        remove() {
            const id = this.groupitem.id
            axios.delete(`${baseApiUrl}/groupitems/${id}`)
                .then(() => {
                    this.$toasted.global.defaultSuccess()
                    this.reset()
                })
                .catch(showError)
        },
        loadGroupitem(groupitem, mode = 'save') {
            this.mode = mode
            this.groupitem = { ...groupitem }
            this.$refs.searchFocus.focus();
        }
    },
    mounted() {
        this.loadGroupitems()
    }
}
</script>

<style>


    .groupitem-admin select{
        font-size: 0.8rem;
    }
    .groupitem-admin input{
        font-size: 0.8rem;
    }
    .groupitem-admin button{
        font-size: 0.8rem;
    }


</style>
