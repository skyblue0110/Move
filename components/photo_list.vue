<template>
    <v-container class="mb-10 pb-10">
        <v-row class="my-10">
            <v-col cols="12" class="text-center">
                <h1>포토 후기</h1>
            </v-col>
        </v-row>
        <v-row>
            <v-col v-for="(item, index) in historyList" :key="index" class="px-5 mb-10 myflex">
                <v-card class="py-2 px-2">
                    <v-img :src="item.url" height="200px" class="pointer" @click="fnInfo(item)"></v-img>
                    <h1 class="title grey--text text--darken-3 my-3">{{ item.title }}</h1>
                    <p class="body-1 grey--text">{{ item.info  }}</p>
                    <p class="body-1 grey--text">{{ item.date }}</p>
                </v-card>
            </v-col>
        </v-row>
        <v-col cols="12" class="text-center my-3" v-if="showBtn">
            <v-btn color="orange" @click="moveToWrite">글쓰기</v-btn>
        </v-col>
        <v-pagination class="mb-2" v-model="page" :length="pages" @input="updatePage"></v-pagination>

    </v-container>
</template>

<script>
    import { oPhotoDB } from '@/assets/firebase'
    export default {
        name:'photoList',
        firebase:{ oPhotos : oPhotoDB },
        data() {
            return {
                oPhotos:[],
                showBtn : false,
                page:1,
                pageSize:4,
                listCount:0,
                historyList:[],
            }
        },
        mounted() {
            this.listCount = this.oPhotos.length
            console.log(this.listCount)
            this.init()
        },
        methods:{
            moveToWrite(){
                this.$router.push({name:'board_write', params:{ type:'photo'}})
            },
            fnInfo(item){
                this.$router.push({name:'info_page', params:{item:item, title:item.title}})
            },
            updatePage(num){
                let start = (num-1) * this.pageSize
                let end = num * this.pageSize
                this.historyList = this.oPhotos.slice(start, end)
                this.page = num
            },
            init(){
                this.listCount = this.oPhotos.length
                console.log(this.listCount)
                if ( this.listCount < this.pageSize ) {
                    this.historyList = this.oPhotos
                } else {
                    this.historyList = this.oPhotos.slice(0, this.pageSize)
                }
            }
        },
        created(){
            const email = this.$store.getters.fnGetUser.email
            this.showBtn = email ? true : false
            this.page = this.$route.params.page
        },
        computed:{
            pages(){
                if (this.pageSize == null || this.listCount == null ) return 0;
                return Math.ceil(this.listCount / this.pageSize)
            },
            
        },
        
    }
</script>

<style lang="scss" scoped>
    .myflex { flex:0 1 25% }
</style>