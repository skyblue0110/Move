<template>
    <v-container class="my-10">
        <v-row>
            <v-col cols="10" class="mx-auto">
                <v-card>
                    <v-img :src="imgUrl"></v-img>
                </v-card>
            </v-col>
        </v-row>
        <v-row>
            <v-col cols="8" class="mx-auto">
                <v-text-field name="title" label="사진제목" v-model="sTitle" autofocus></v-text-field>
            </v-col>
        </v-row>
        <v-row>
            <v-col cols="8" class="mx-auto">
                <v-textarea name="info" label="사진설명" v-model="sInfo" multi-line rows="3"></v-textarea>
            </v-col>
        </v-row>
        <v-row>
            <v-col cols="12" class="text-center">
                <v-btn color="orange" dark large @click="fnSubmitPost">업로드</v-btn>
            </v-col>
        </v-row>
    </v-container>
</template>

<script>
    import { oPicturesinDB } from '@/assets/firebase'
    export default {
        data() {
            return {
               sTitle:'',
               sInfo:'',
               imgUrl:this.$store.getters.imgUrl,
               fileName:this.$store.getters.fileName
            }
        },
        methods:{
            fnSubmitPost(){
                oPicturesinDB.push({
                    'url':this.imgUrl,
                    'title':this.sTitle,
                    'info':this.sInfo,
                    'filename':this.fileName,
                }).then(this.$router.push('/'))
            }
        }
    }
</script>

<style lang="scss" scoped>

</style>