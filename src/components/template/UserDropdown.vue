<template>
    <div class="user-dropdown" >
        <div class="user-button">          
            <span class="d-none d-sm-block" >
                <!--img v-if="emp.caminhoImagem"  :src="`img/${emp.caminhoImagem}`"  class="image"  width="70" alt="logo"/-->
                <!--img width="70" alt="logo" class="image" src="@/assets/waforman.png">
                <img width="70" alt="logo" class="image" src="@/assets/wastylus.png">
                <img width="70" alt="logo" class="image" src="@/assets/wakids.png"-->
                 {{emp.razaosocial}} / {{user.name || ''}}</span>
            <div class="user-dropdown-img">
                 <Gravatar :email="user.name || ''" alt="User" />   
            </div>
            <i class="fa fa-angle-down" ></i> 
        </div>
         <div class="user-dropdown-content">
                 <b-btn v-b-modal.modalEmpresa @click.prevent=" loadEmpresas" v-b-popover.hover = "'Selecione a Loja que deseja trabalhar!'"  title = "Escolha a Loja" >Loja</b-btn>
                <router-link to='/admin' v-if="user.admin" > <i class="fa fa-cogs"></i> Administração </router-link>
                <router-link to='/supervisor' v-if="user.supervisor" > <i class="fa fa-users"></i> Supervisor </router-link>
                <a href @click.prevent="logout" > <i class="fa fa-sign-out"></i> Sair
                </a>
        </div>
        <b-modal id="modalEmpresa"
            ref="modal"
            title="Informe a Loja"
            @ok="setEmpresa"
            >
            <b-form-select id="empresa-id" :options="empresas" v-model="empresa.id" />
        </b-modal>
    </div>
</template>

<script>
import {userKey, empresakey} from '@/global'
import {mapState} from 'vuex'
import Gravatar from 'vue-gravatar'
import { baseApiUrl, showError } from '@/global'
import axios from 'axios'

export default {
    name: 'UserDropdown',
     data: function(){
        return {
            empresa: {},
            empresas: []
        }
    },
    components: {Gravatar},
    computed: mapState(['user','emp']),
    methods:{
        logout(){
            localStorage.removeItem(userKey)
            this.$store.commit('setUser',null)
            this.$router.push({name: 'auth'})
        },
        setEmpresa() {
            axios.post(`${baseApiUrl}/setEmp`, this.empresa)
                .then(res => {
                    localStorage.removeItem(empresakey)
                    this.$store.commit('setEmp', res.data)
                    localStorage.setItem(empresakey, JSON.stringify(res.data))
                    this.$router.push({ path: '/' })
                    //this.logoEmp = require(`~/assets/img/${this.empresa.caminhoImagem}`)  
                   
                })
                .catch(showError)
        },
         loadEmpresas() {
            const url = `${baseApiUrl}/empresas`
            axios.get(url).then(res => {
                this.empresas = res.data.map(empresa => {
                    return { ...empresa, value: empresa.id, text: empresa.razaosocial }})
            })
        }
    }
}
</script>

<style>
    .user-dropdown {
        position: relative;
        height: 100%;
    }

    .user-button {
        display: flex;
        align-items: center;
        color: #fff;
        font-weight: 100;
        height: 100%;
        padding: 0px 20px;
    }

    .user-dropdown:hover {
        background-color: rgba(0, 0, 0, 0.2);
    }

    .user-dropdown-img {
        margin: 0px 10px;
    }

    .user-dropdown-img > img {
        max-height: 37px;
        border-radius: 5px;
    }

    .user-dropdown-content {
        position: absolute;
        right: 0px;
        background-color: #f9f9f9;
        min-width: 170px;
        box-shadow: 0px 8px 16px 0px rgba(0,0,0,0.2);
        padding: 10px;
        z-index: 1;

        display: flex;
        flex-direction: column;
        flex-wrap: wrap;

        visibility: hidden;
        opacity: 0;
        transition: visibility 0s, opacity 0.5s linear;
    }

    .user-dropdown:hover .user-dropdown-content {
        visibility: visible;
        opacity: 1;
    }

    .user-dropdown-content a{
        text-decoration: none;
        color: #000;
        padding: 10px;
    }

      .user-dropdown-content a:hover{
        text-decoration: none;
        color: #000;
        background-color: #EDEDED;

      }

</style>
