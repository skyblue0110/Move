<template>
    <v-container class="my-10">
        <v-row class="mb-10">
            <v-col cols="12" class="text-center">
                <h1>{{ type }} 글쓰기</h1>
            </v-col>
            <v-col cols="12">
                <v-card flat>
                    <v-form @submit.prevent="onSubmitForm">
                        <v-text-field v-model="writer" dense outlined label="작성자" style="width:500px; padding-top:20px;" :rules="[ v => !!v || '작성자는 필수입니다.' ]"></v-text-field>
                        <v-text-field v-model="title" dense outlined label="제목" style="width:500px; padding-top:20px;" :rules="[ v => !!v || '제목은 필수입니다.' ]"></v-text-field>
                        <v-textarea v-model="text" label="내용" outlined rows="10"></v-textarea>
                        <v-file-input @change="previewImage" v-if="fileBtn" accept="image/*" v-model="files" dense label="사진 첨부" placeholder="사진 업로드" prepend-icon="mdi-paperclip" outlined :show-size="1000" couter style="width:500px; padding-bottom:20px"></v-file-input>
                        <div class="text-center" v-if="!bIsWait">
                            <v-btn width="100px" class="mx-1" @click="moveToList">취소</v-btn>
                            <v-btn width="100px" class="mx-1" type="submit">등록</v-btn>
                        </div>
                        <div class="text-center" v-else>
                            <v-progress-circular size="50" indeterminate color="grey"></v-progress-circular>
                        </div>
                    </v-form>
                </v-card>
            </v-col>
        </v-row>
    </v-container>
</template>

<script>
    import { oNoticeDB, oReviewDB, oStorage, oPhotoDB} from '@/assets/firebase'
    export default {
        data(){
            return {
                writer:'',
                title:'',
                text:'',
                date:'',
                type:'',
                fileBtn:false,
                files:'',
                imageData:'',
                bIsWait:false
            }
        },
        created(){
           this.type = this.$route.params.type
           if (this.type==='photo') this.fileBtn = true
        },
        methods:{
            onSubmitForm(){
                this.date = new Date().toISOString()
                this.$store.commit('fnSetNum')
                if (this.type==='notice') {
                    oNoticeDB.push({
                    'idnum' : this.$store.getters.fnGetNum,
                    'title' : this.title,
                    'text' : this.text,
                    'writer': this.writer,
                    'date' : this.date.split('T')[0]
                    }).then(this.$router.push('/notice_list'))
                } else if (this.type==='review') {
                    oReviewDB.push({
                    'userEmail':this.$store.getters.fnGetUser.email,
                    'idnum' : this.$store.getters.fnGetNum,
                    'title' : this.title,
                    'text' : this.text,
                    'writer': this.writer,
                    'date' : this.date.split('T')[0]
                    }).then(this.$router.push('/review_list'))
                } else {
                    this.onUpload()
                }
            },
            moveToList(){
                if (this.type==='notice'){
                    this.$router.push('/notice_list')
                } else if (this.type==='review') {
                    this.$router.push('/review_list')
                } else {
                    this.$router.push('/photo_list')
                }
            },
            previewImage(){
                console.log(this.files)
                this.imageData = this.files
            },
            onUpload(){
                let uploadTask = oStorage.ref('images').child(this.imageData.name).put(this.imageData)
                const self = this
                uploadTask.on('state_changed', function (snapshot) {
                    let progress = (snapshot.bytesTransferred / snapshot.totalBytes) * 100;
                    console.log('업로드: ' + progress + '% 완료', snapshot.state);
                    self.bIsWait = !self.bIsWait
                }, function (error) {
                    console.log(error)
                }, function () {
                    uploadTask.snapshot.ref.getDownloadURL().then(function (downloadURL) {
                    console.log('업로드URL:', downloadURL);
                    oPhotoDB.push({
                        'url':downloadURL,
                        'idnum':self.$store.getters.fnGetNum,
                        'title':self.title,
                        'info':self.text, 
                        'writer':self.writer,
                        'date':self.date.split('T')[0],
                        'filename':self.imageData.name,
                        'userEmail':self.$store.getters.fnGetUser.email
                    }).then(self.$router.push('/photo_list'))
                    });
                }); 
            }
        }
    }
</script>

<style lang="scss" scoped>

</style>