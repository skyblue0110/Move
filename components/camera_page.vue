<template>
    <v-container>
        <v-row>
            <v-col cols="12" class="text-center">
                <video ref="rVideo"></video>
            </v-col>
            <v-col cols="12" class="text-center mt-5">
                <p>현재 iOS는 지원하지 않습니다.</p>
            </v-col>
        </v-row>
        <div class="text-center my-3">
            <v-btn v-if="!bIsWait" color="red" fab dark bottom @click="fnCameraCapture()">
                <v-icon>mdi mdi-camera-iris</v-icon>
            </v-btn>
            <v-progress-circular size="50" indeterminate color="grey" v-else></v-progress-circular>
        </div>
    </v-container>
</template>

<script>
    import { oStorage } from '@/assets/firebase'

    export default {
        name:'CameraPage',
        // 파이어베이스와 연결된 뷰파이어 oPictures 객체 준비
        // firebase:{ oPictures : oPicturesinDB  },
        data() {
            return {
                bIsWait: false,
                oPictures:[],       // 사진데이터 저장 변수
                oVideoStream:null,  // 카메라 영상 스트림을 저장할 객체변수
            }
        },
        methods:{
            fnCameraCapture(){
                this.bIsWait = !this.bIsWait
                // 현재 재생되는 트랙을 찾아 스틸이미지로 캡처함
                const oVideoTrack = this.oVideoStream.getVideoTracks()[0]
                let oCapturedImage = new window.ImageCapture(oVideoTrack)
                const options = {
                imageHeight: 359,
                imageWidth: 640,
                fillLightMode: 'off'
                };
                const self = this
                // 캡처된 이미지를 파이어베이스 DB와 스토리지에 저장함
                return oCapturedImage.takePhoto(options).then(pImageData => {
                // 영상 정지
                const oTrack = self.oVideoStream.getTracks()
                oTrack.map(pTrack => pTrack.stop())
                console.log('캡처: ' + pImageData.type + ', ' + pImageData.size + '바이트');
                // 저장할 이미지 파일이름으로 사용할 ID 준비
          const nID = new Date().toISOString()
          // 파이어베이스 스토리지에 이미지 파일 저장
          let uploadTask = oStorage.ref('images').child('pic' + nID).put(pImageData)
          uploadTask.on('state_changed', function (snapshot) {
            // state_changed 이벤트를 통해서 얼만큼의 바이트가 업로드 중인지 콘솔에 표시
            let progress = (snapshot.bytesTransferred / snapshot.totalBytes) * 100;
            console.log('업로드: ' + progress + '% 완료', snapshot.state);
          }, function (error) {
            console.log(error)
            // 오류 발생 시 콘솔에 표시
          }, function () {
            // 성공적으로 업로드 완료 후 파이어베이스 DB에 정보 저장
            uploadTask.snapshot.ref.getDownloadURL().then(function (downloadURL) {
              console.log('업로드URL:', downloadURL);
               self.$store.commit('IMG_SAVE', {url:downloadURL, filename:'pic'+nID})
               self.$router.push('/post')
            });
           });    
          })
         }
        },
        mounted() {
            // Web API를 통해서 사용자 카메라의 접근(영상 only)을 요청함
            navigator.mediaDevices.getUserMedia({
                video: true
            }).then(pVideoStream => {
                // 카메라 영상 스트림 정보를 oVideoStream에 저장함
                this.oVideoStream = pVideoStream
                // 카메라 영상 스트림 정보를 video 엘리먼트에 표시함
                this.$refs.rVideo.srcObject = pVideoStream
                this.$refs.rVideo.play()
            }).catch(function (err) {
                console.log(err)
            })
            },
            destroyed(){
                // 현재 화면을 종료할 경우 현재 재생되는 영상 트랙을 찾아 종료시킴
                const oTrack = this.oVideoStream.getTracks()
                oTrack.map(pTrack => pTrack.stop())
            }
    }
</script>

<style lang="scss" scoped>
    
</style>