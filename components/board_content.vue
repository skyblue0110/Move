<template>
    <v-container class="my-10">
        <v-row class="mb-10">
            <v-col cols="12" class="text-center">
                <h1>{{  itemPic.title }}</h1>
            </v-col>
            <v-col cols="12">
                <v-card flat>
                    <v-text-field disabled dense outlined label="작성자" :value="itemPic.writer" style="width:500px; padding-top:20px;"></v-text-field>
                    <v-text-field disabled dense outlined label="제목" :value="itemPic.title" style="width:500px; padding-top:20px;"></v-text-field>
                    <v-textarea disabled label="내용" outlined rows="10" :value="itemPic.text"></v-textarea>
                    <div class="text-center">
                        <v-btn width="100px" class="mx-1" @click="moveToNoticeList">목록</v-btn>
                        <v-btn width="100px" class="mx-1" @click="fnModify" v-if="showBtn">수정</v-btn>
                        <v-btn width="100px" class="mx-1" @click="fnDelete" v-if="showBtn">삭제</v-btn>
                    </div>
                </v-card>
            </v-col>
        </v-row>
    </v-container>
</template>

<script>
    import { oNoticeDB, oReviewDB } from '@/assets/firebase'
    export default {
        firebase:{ contentlist1 : oNoticeDB, contentlist2 : oReviewDB },
        data() {
            return {
                contentlist1: [],
                contentlist2: [],
                itemPic : null,
                pageNum:0,
                showBtn:false,
                type:''
            }
        },
        created(){
            const { idnum, userEmail } = this.$route.params.item
            this.type = this.$route.params.type
            this.pageNum = this.$route.params.page
            if (this.type==='notice') {
                this.itemPic = this.contentlist1.find(item => item.idnum === idnum )
            } else {
                this.itemPic = this.contentlist2.find(item => item.idnum === idnum )
            }
            this.showBtn = this.$store.getters.fnGetUser.email === userEmail ? true : false
        },
        methods:{
            fnModify(){
                this.$router.push({name:'board_modify', params:{ item:this.itemPic, title:this.itemPic.title, page:this.pageNum, type:this.type}})
            },
            fnDelete(){
                if (this.type==='notice') {
                    oNoticeDB.child(this.itemPic['.key']).remove()
                    this.$router.push({name:'notice_list', params:{page:this.pageNum} })
                } else {
                    oReviewDB.child(this.itemPic['.key']).remove()
                    this.$router.push({name:'review_list', params:{page:this.pageNum} })
                }
                
            },
            moveToNoticeList(){
                if (this.type==='notice') {
                    this.$router.push({name:'notice_list', params:{page:this.pageNum} })
                } else {
                    this.$router.push({name:'review_list', params:{page:this.pageNum} })
                }
                

            }
        }
    }
</script>

<style lang="scss" scoped>

</style>