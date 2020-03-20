<template>
  <div>
<!--    <video :id="'videoElement'+index" v-for="(item,index) in urlList" class="centeredVideo" :key="index" controls autoplay width="1024" height="576">Your browser is too old which doesn't support HTML5 video.</video>-->
    <video id="videoElement1" class="centeredVideo" controls autoplay width="1024" height="576">Your browser is too old which doesn't support HTML5 video.</video>
  </div>
</template>

<script>
  const flvjs = require('flv.js/dist/flv.js')
export default {
  name: 'MonitorView',
  data () {
    return {
      urlList : [
        {url:'http://172.16.7.115:8088/live?port=1994&app=myapp&stream=fff'},
        {url:'http://172.16.7.115:8088/live?port=1994&app=myapp&stream=ttt'}
      ],
      flvPlayer: Object
    }
  },
  mounted() {
    this.show()
  },
  methods: {
    show () {
      var player = document.getElementById('videoElement1')
      if ( flvjs.isSupported()){
        this.flvPlayer = flvjs.createPlayer({
          type: 'flv',
          url: this.urlList[1].url,//<==自行修改
          enableWorker: true,
          enableStashBuffer: false,
          stashInitialSize: 128,
          isLive: true,
          isAutoPlay: true,
          isContinue: true,
          lazyLoad: true
        })
        this.flvPlayer.attachMediaElement(player);
        this.flvPlayer.load(); //加载
        player.play()
        //避免时间长时间积累缓冲导致延迟越来越高
        setInterval(() => {
          if (!this.flvPlayer.buffered.length) {
            return;
          }
          let end = this.flvPlayer.buffered.end(0);
          let diff = end - this.flvPlayer.currentTime;
          if (diff >= 1.5) {
            console.log('触发');
            this.flvPlayer.currentTime = end - 0.1;
          }
        }, 3 * 10 * 1000);
      }
    }
  },
  // flv_start() {
  //   player.play();
  // },
  //
  //
  // flv_pause() {
  //   player.pause();
  // },
  //
  // flv_destroy() {
  //   player.pause();
  //   player.unload();
  //   player.detachMediaElement();
  //   player.destroy();
  //   player = null;
  // },
  //
  // flv_seekto() {
  //   player.currentTime = parseFloat(document.getElementsByName('seekpoint')[0].value);
  // }
}
</script>

<style scoped>

</style>
