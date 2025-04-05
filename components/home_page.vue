<template>
    <v-container>
        <v-row class="my-10">
            <v-col cols="12">
                <slide-components></slide-components>
            </v-col>
        </v-row>
        <v-row class="my-10">
            <v-col cols="12">
                <tab-components></tab-components>
            </v-col>
        </v-row>
        <v-row class="my-10">
            <v-col cols="12" class="text-left">
                <h1>EVENT</h1>
                <event-components></event-components>
            </v-col>
        </v-row>
        <v-row class="my-10">
            <v-col cols="12" class="text-left">
                <h1>특별관</h1>
                <!-- <special-components></special-components> -->
            </v-col>
        </v-row>
        <v-row class="my-10">
            <v-col cols="12" class="text-left">
                <h1>패키지</h1>
                <package-components></package-components>
            </v-col>
        </v-row>
        <v-row class="my-10">
            <v-col cols="12" class="text-left">
                <h1>공지사항</h1>
                <notice-components></notice-components>
            </v-col>
        </v-row>
        <v-row class="my-10">
            <v-col cols="12">
                <h1>AD</h1>
                <v-app id="inspire">
                    <v-container>
                        <v-row no-gutters>
                            <v-col>
                            <v-card class="pa-2" outlined tile>
                                AD
                                <v-img src="./assets/images/16780934203280.png"></v-img>
                                <v-img src="./assets/images/광고-BUGS.png"></v-img>
                            </v-card>
                        </v-row>
                        </v-container>
                    </v-app>
            </v-col>
        </v-row>
        <v-row class="my-10">
            <v-col cols="12" class="text-left">
                <h1>footer</h1>
            </v-col>
            <v-col cols="12" sm="6" md="4" lg="3" xl="2" v-for="item in aPictures" :key="item.filename">
                <!-- <v-card class="py-2 px-2">
                    <v-img :src="item.url" height="200px" class="pointer" @click="fnDisplayInfo(item['.key'], item.title)"></v-img>
                    <v-card-title>
                        <h1 class="title grey--text text--darken-3 mb-3">{{ item.title }}</h1>
                        <p class="body-1 grey--text">{{ item.info }}</p>
                    </v-card-title>
                </v-card> -->
            </v-col>
        </v-row>

            

            <!-- <v-btn @click="$router.push('/post')" color="red" dark bottom right fixed fab>
                <v-icon>mdi mdi-plus</v-icon>
            </v-btn> -->
    </v-container>
</template>

<script>
    // import oPictures from '../assets/picture-data.js'
    import { oPicturesinDB } from '@/assets/firebase'
    import slide from '@/components/slide_component.vue'
    import tabMenu from '@/components/tab_menu.vue'
    import event from '@/components/event_component.vue'
    // import special from '@/components/special_component.vue'
    import packages from '@/components/package_component.vue'
    import notice from '@/components/notice_component.vue'

    export default {
        name:'HomePage',
        // 파이어베이스와 연결된 뷰파이어 oPictures 객체 준비
        firebase:{ aPictures : oPicturesinDB  },
        data(){
            return {
                aPictures : []
            }
        },
        components:{
            'slide-components' : slide,
            'tab-components' : tabMenu,
            'event-components' : event,
            // 'special-components' : special,
            'package-components' : packages,
            'notice-components' : notice,
        },
        methods:{
            fnDisplayInfo(pID, pTitle) {
                // console.log(this.aPictures, pID)
                this.$router.push({name:'info_page', params:{p_id : pID, p_title : pTitle }})
            }
        }
    }
</script>

<style lang="scss" scoped>
    .pointer { cursor:pointer; }
</style>