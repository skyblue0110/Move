<template>
    <v-container class="my-10">
        <v-row class="mb-10">
            <v-col cols="12" class="text-center">
                <h1>{{ itemPic.title }}</h1>
            </v-col>
            <v-col cols="12">
                <v-card flat>
                    <v-form @submit.prevent="onSubmitForm">
                        <v-text-field v-model="itemPic.writer" dense outlined label="작성자" style="width:500px; padding-top:20px;" :rules="[ v => !!v || '작성자는 필수입니다.' ]"></v-text-field>
                        <v-text-field v-model="itemPic.title" dense outlined label="제목" style="width:500px; padding-top:20px;" :rules="[ v => !!v || '제목은 필수입니다.' ]"></v-text-field>
                        <v-textarea v-model="itemPic.text" label="내용" outlined rows="10"></v-textarea>
                        <div class="text-center">
                            <v-btn width="100px" class="mx-1" @click="moveToNoticeList">목록</v-btn>
                            <v-btn width="100px" class="mx-1" type="submit">수정완료</v-btn>
                        </div>
                    </v-form>
                </v-card>
            </v-col>
        </v-row>
    </v-container>
</template>

<script>
    import { oNoticeDB, oReviewDB } from '@/assets/firebase'
    export default {
        firebase:{ contentlist1 : oNoticeDB, contentlist2 : oReviewDB },
        data(){
            return {
                contentlist1:[],
                contentlist2:[],
                itemPic:null,
                pageNum:0,
                type:''
            }
        },
        created(){
            const { idnum } = this.$route.params.item
            this.type = this.$route.params.type
            this.pageNum = this.$route.params.page

            if (this.type==='notice') {
                this.itemPic = this.contentlist1.find( item => item.idnum === idnum )
            } else {
                this.itemPic = this.contentlist2.find( item => item.idnum === idnum )
            }
            
        },
        methods:{
            onSubmitForm(){
                this.date = new Date().toISOString()
                if (this.type==='notice') {
                    oNoticeDB.child(this.itemPic['.key']).update({
                        'title' : this.itemPic.title,
                        'text' : this.itemPic.text,
                        'writer': this.itemPic.writer,
                        'date' : this.date.split('T')[0]
                    }).then(this.$router.push({name:'notice_list', params:{page:this.pageNum}}))
                } else {
                    oReviewDB.child(this.itemPic['.key']).update({
                        'title' : this.itemPic.title,
                        'text' : this.itemPic.text,
                        'writer': this.itemPic.writer,
                        'date' : this.date.split('T')[0]
                    }).then(this.$router.push({name:'review_list', params:{page:this.pageNum}}))
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