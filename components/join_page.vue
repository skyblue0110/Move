<template>
    <v-container>
        <v-row>
            <v-col cols="12" class="text-center my-5">
                <h1 class="display-1">회원가입 페이지</h1>
            </v-col>
        </v-row>
        <v-row>
            <v-col cols="8" offset="2" class="text-center">
                <form @submit.prevent="fnRegisterUser">
                    <v-text-field name="Email" label="이메일" type="email" v-model="sEmail" required></v-text-field>
                    <v-text-field name="Password" label="비밀번호" type="password" v-model="sPassword" required></v-text-field>
                    <!-- 비밀번호 확인이 맞는지 검사하도록 rules 속성 사용 -->
                    <v-text-field name="ConfirmPassword" label="비밀번호 확인" type="password" v-model="sConfirmPassword" required :rules="[fnComparePassword]"></v-text-field>
                    <v-btn type="submit" color="orange" dark>회원가입</v-btn>
                    <!-- 시간지연의 경우 보이게 될 회전 프로그레스 원 -->
                    <v-progress-circular v-if="fnGetLoading" indeterminate :width="7" :size="70" color="grey lighten-1"></v-progress-circular>
                    <!-- 오류 메시지가 있을 경우 표시 -->
                    <v-alert class="mt-3" type="error" dismissible v-model="bAlert">
                        {{ fnGetErrMsg  }}
                    </v-alert>
                </form>
            </v-col>
        </v-row>
    </v-container>
</template>

<script>
    export default {
        data() {
            return {
                sEmail:'',
                sPassword:'',
                sConfirmPassword:'',
                bAlert:false
            }
        },
        computed:{
            fnComparePassword(){
                if (this.sPassword == this.sConfirmPassword) return true
                else return '비밀번호가 일치하지 않습니다.'
            },
            fnGetLoading(){
                return this.$store.getters.fnGetLoading;
            },
            fnGetErrMsg(){
                return this.$store.getters.fnGetErrorMessage
            }
        },
        watch:{
            bAlert(pValue){
                console.log(pValue)
                if (pValue == false) this.$store.commit('fnSetErrorMessage', '')
            },
            fnGetErrMsg(pMsg){
                if (pMsg) this.bAlert = true;
            }
        },
        methods:{
            fnRegisterUser(){
                if (this.fnComparePassword) {
                    this.$store.dispatch('fnRegisterUser', {
                        pEmail:this.sEmail,
                        pPassword:this.sPassword
                    })
                }
            }
        }
    }
</script>

<style lang="scss" scoped>

</style>