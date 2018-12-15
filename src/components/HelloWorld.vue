<template>
<div class="hello">
    <b-container>
        <b-row>
            <b-col md='4' offset-md="8">
                <b-form-select v-model="selected" :options="lang" class="mb-3" />
            </b-col>
        </b-row>
        <b-row>
            <b-col md='6' v-for="(item, index) in dataImage" v-bind:key="index">
                <div class="wrapper" @click='showModal(item.id)'>
                        <img v-if="isMobile" class="img-fluid" :src="`../../static/images/${item.image_mobile}`">
                    <img v-else class="img-fluid" :src="`../../static/images/${item.image}`">
                    <h3 v-if="selected==='ind'">{{item.title.id}}</h3>
                    <h3 v-else-if="selected==='eng'">{{item.title.en}}</h3>
                    <h3 v-else-if="selected==='jap'">{{item.title.ja}}</h3>
                </div>
            </b-col>
        </b-row>
    </b-container>
    <b-modal id="modal1" title="Bootstrap-Vue" ref="myModalRef">
        <b-carousel id="carousel1"
                    controls
                    indicators
                    background="#ababab"
                    :interval="4000"
                    img-width="1024"
                    img-height="480"
                    v-model="slide"
                    @sliding-start="onSlideStart"
                    @sliding-end="onSlideEnd">
            <b-carousel-slide 
                v-if="isMobile"
                v-for="(test, index) in dataImage" 
                v-bind:key="index" 
                :img-src="`../../static/images/${test.image_mobile}`">
            </b-carousel-slide>
            <b-carousel-slide 
                v-for="(test, index) in dataImage" 
                v-bind:key="index" 
                :img-src="`../../static/images/${test.image}`">
            </b-carousel-slide>
        </b-carousel>
    </b-modal>
</div>
</template>

<script>
export default {
  data: function() {
  return {
        dataImage: [],
        lang: [
            {
                text: 'Japan',
                value: 'jap'
            },
            {
                text: 'English',
                value: 'eng'
            },
            {
                text: 'Bahasa',
                value: 'ind'
            }
        ],
        selected: 'jap',
        error: false,
        slide: 0,
        sliding: null
    };
  },
  computed: {
      isMobile: function() {
          if( navigator.userAgent.match(/Android/i)
            || navigator.userAgent.match(/webOS/i)
            || navigator.userAgent.match(/iPhone/i)
            || navigator.userAgent.match(/iPad/i)
            || navigator.userAgent.match(/iPod/i)
            || navigator.userAgent.match(/BlackBerry/i)
            || navigator.userAgent.match(/Windows Phone/i)
        ){
            return true;
        }
        else {
            return false;
        }
      }
  },
  created: function() {
      const API = 'https://demo0486113.mockable.io/gallery'
      const request = new Request(API);
      return fetch(request).then(response => response.json()).then(json => {
          this.dataImage = json.data
      }).catch(err => {
        if (err) {
          this.error = true
        }
      })
  },
  methods: {
    onSlideStart (slide) {
        this.sliding = true
    },
    onSlideEnd (slide) {
        this.sliding = false
    },
    showModal(id) {
        this.slide = id - 1
        this.$refs.myModalRef.show()
    }
  }
}
</script>

<style lang="scss">

.wrapper {
    border: 1px solid #0f0f0f;
    padding: 10px;
    margin-bottom: 15px;
    img {
        margin-bottom: 15px;
    }
    &:hover {
        box-shadow: 0 2px 4px 0 rgba(0,0,0,0.16),0 2px 10px 0 rgba(0,0,0,0.12)!important;
    }
}

</style>

