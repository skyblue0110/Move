<template>
    <v-carousel cycle height="100" hide-delimiter-background show-arrows-on-hover>
      <v-carousel-item v-for="(slide, i) in slides" :key="i" :src="slide.src">
        <v-sheet height="100%">
          <v-row class="fill-height" align="center" justify="center">
          </v-row>
        </v-sheet>
      </v-carousel-item>
    </v-carousel>
  </template>
  
  <script>
    export default {
      data () {
        return {
          slides: [
            { src:'./assets/images/16780934203280.jpg' },
            { src:'./assets/images/광고-BUGS.jpg' },
          ],
        }
      },
      computed:{
          displayText(){
              let text = ''
              if ( this.$mq === 'mobile') {
                  text = '모바일'
              } else if (this.$mq === 'tablet') {
                  text = '태블릿'
              } else {
                  text = 'PC'
              }
              return text
              // return this.$mq = 'mobile' ? '모바일' : this.$mq
          }
      }
    }
  </script>
  
  <style lang="scss">
      .theme--light.v-sheet {
          background: transparent !important;
          color: #fff;
     }
     .fontsize.mobile { font-size: 20em !important; }
     .fontsize.tablet { font-size: 30px !important; }
     .fontsize.pc { font-size: 40px !important; }
  </style>