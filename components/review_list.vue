<template>
    <v-container class="my-10">
        <v-row>
            <v-col cols="12" class="text-center">
                <h1>상품후기</h1>
            </v-col>
        </v-row>
        <v-row>
            <v-col class="mt-3 px-0">count : {{ contentlist.length }}개</v-col>
            <v-space></v-space>
            <v-text-field class="mb-5" v-model="search" append-icon="mdi-magnify" label="Search" single-line hide-details></v-text-field>
        </v-row>
        <v-row class="mb-10">
            <v-data-table class="elevation-1" :sort-by="['index']" :sort-desc="[true]" :headers="headers" :items="itemsWithIndex" :search="search" :loading="loading" loading-text="text" :page.sync="page" :items-per-page="itemsPerPage" hide-default-footer @page-count="pageCount = $event" @click:row="clickRow"></v-data-table>
            <v-col cols="12">
                <v-pagination v-model="page" :length="pageCount" :total-visible="totalVisible" next-icon="mdi-menu-right" prev-icon="mdi-menu-left"></v-pagination>
            </v-col>
            <v-col cols="12" class="text-center my-3" v-if="showWriteBtn">
                <v-btn color="orange" @click="moveToWrite">글쓰기</v-btn>
            </v-col>
        </v-row>
    </v-container>
</template>

<script>
    import { oReviewDB } from '@/assets/firebase'
    export default {
        firebase : { contentlist : oReviewDB },
        data(){
            return {
                contentlist : [],
                headers:[
                    { text:'번호', width:'10%', value:'index', sortable:false, align:'center' },
                    { text:'제목', width:'auto', value:'title', sortable:false, align:'center' },
                    { text:'작성자', width:'10%', value:'writer', sortable:false, align:'center' },
                    { text:'작성일', width:'10%', value:'date', sortable:false, align:'center' }
                ],
                text:'Welcome!!',
                page: 1,
                pageCount:0,
                itemsPerPage:5,
                totalVisible:10,
                loading:false,
                search:'',
                showWriteBtn:false
            }
        },
        methods:{
            clickRow(item){
                this.$router.push({name:'board_content', params:{ item:item, title:item.title, page:this.page, type:'review' }})
            },
            moveToWrite(){
                this.$router.push({name:'board_write', params:{ type:'review' }})
            }
        },
        created(){
            this.page = this.$route.params.page ? this.$route.params.page : 1
            const email = this.$store.getters.fnGetUser.email
            this.showWriteBtn = email ? true : false
        },
        computed :{
            itemsWithIndex(){
                return this.contentlist.map((item, index)=> ({...item,  index:index+1}))
            }
        }
        
    }
</script>

<style lang="scss" scoped>

</style>