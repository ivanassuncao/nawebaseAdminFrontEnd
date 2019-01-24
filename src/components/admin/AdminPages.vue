<template>
    <div class="admin-pages" >
        <PageTitle icon="fa fa-cogs" main="Administração do Sistema"
        sub="Cadastros e Configurações" />
        <div class="admin-pages-tabs">
            <b-card no-body >
                <b-tabs card>
                     <b-tab title="Usuários">
                        <UserAdmin />
                    </b-tab>
                    <b-tab title="Empresa" active>
                        <CompanyAdmin />
                    </b-tab>
                    <!--b-tab title="Plano Conta">
                        <PlanoContaAdmin />
                    </b-tab>
                    <b-tab title="Funcionário">
                        <FuncionarioAdmin />
                    </b-tab>
                    
                    <b-tab title="Vendedor">
                        <VendedorAdmin />
                    </b-tab>
                    <b-tab title="Cliente">
                        <ClienteAdmin />
                    </b-tab>
                    <b-tab title="Grupo de Itens">
                        <GrupoItemAdmin />
                    </b-tab>
                     <b-tab title="Itens">
                        <ItemAdmin />
                    </b-tab>
                    <b-tab title="Artigos" >
                        <ArticleAdmin />
                    </b-tab>
                    <b-tab title="Categorias">
                        <CategoryAdmin />
                    </b-tab-->
                </b-tabs>  
            </b-card>
        </div>
    </div>
</template>

<script>
import PageTitle from '../template/PageTitle'
import ArticleAdmin from './ArticleAdmin'
import CategoryAdmin from './CategoryAdmin'
import UserAdmin from './UserAdmin'
import GrupoItemAdmin from './GrupoItemAdmin'
import CompanyAdmin from './CompanyAdmin'
import VendedorAdmin from './VendedorAdmin'
import ClienteAdmin from './ClienteAdmin'
import ItemAdmin from './ItemAdmin'
import PlanoContaAdmin from './PlanoContaAdmin'
import FuncionarioAdmin from './FuncionarioAdmin'

export default {
    name: 'AdminPages',
    components:{PageTitle,
                ArticleAdmin,
                CategoryAdmin,
                UserAdmin,
                GrupoItemAdmin,
                CompanyAdmin,
                VendedorAdmin,
                ClienteAdmin,
                ItemAdmin,
                PlanoContaAdmin,
                FuncionarioAdmin
                }
}
</script>

<style>

</style>
