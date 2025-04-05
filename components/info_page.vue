<template>
    <v-container class="my-10">
        <v-row>
            <v-col cols="12">
                <v-card class="py-3 px-3">
                    <v-img height="450px" contain :src="itemPic.url"></v-img>
                    <v-card-text>
                        <h1 class="headline mt-1 text-center">{{ itemPic.title }}</h1>
                        <p class="body-1 mt-1 text-center">{{ itemPic.info }}</p>
                    </v-card-text>
                </v-card>
            </v-col>
            <v-col cols="12" class="text-center mt-3">
                <v-btn v-if="showBtn" color="grey" fab dark @click="fnDeleteItem">
                    <v-icon>mdi mdi-delete</v-icon>
                </v-btn>
                <v-btn color="grey" fab dark @click="$router.push('/photo_list')" class="mx-3">
                    <v-icon>mdi mdi-view-list</v-icon>
                </v-btn>
            </v-col>
        </v-row>
    </v-container>
</template>

<script>
    import { oPhotoDB, oStorage } from '@/assets/firebase'
    export default {
        name:'infoPage',
        firebase:{ oPhotos : oPhotoDB},
        data() {
            return {
               oPhotos: [],
               itemPic : null,
               showBtn : false
            }
        },
        created(){
            const { idnum, userEmail } = this.$route.params.item
            this.itemPic = this.oPhotos.find( item => item.idnum === idnum )
            if (userEmail === this.$store.getters.fnGetUser.email) {
                this.showBtn = !this.showBtn
            }
        },
        methods:{
            fnDeleteItem(){
                oPhotoDB.child(this.itemPic['.key']).remove()
                if ( this.itemPic.filename ) {
                    oStorage.ref('images').child(this.itemPic.filename).delete()
                }   
                this.$router.push('/photo_list')
            }
        }
    }
</script>

<style lang="scss" scoped>

</style>