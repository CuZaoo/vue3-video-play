/*
 * @Author: web.王晓冬
 * @Date: 2021-08-20 19:10:57
 * @LastEditors: itab.link
 * @LastEditTime: 2023-11-09 15:38:31
 * @Description: file content
*/
<template>
  <!-- <div style="height:150px; margin-top:100px">
    <d-slider v-model="options.volume"></d-slider>
  </div>-->
  <div style="text-align: center">
    {{ options.webFullScreen }}
    <button
      @click="options.src = 'https://test-streams.mux.dev/x36xhzz/x36xhzz.m3u8'"
    >
      {{ options.src }}
    </button>
    <videoPlay
      ref="video"
      style="display: inline-block; width: 100%"
      v-bind="options"
    />
  </div>
</template>

<script setup lang="ts">
import { reactive, ref, nextTick } from "vue";
import { videoPlay } from "../lib/index.js";

const options = reactive({
  width: "800px",
  height: "450px",
  color: "#409eff",
  muted: false, //静音
  webFullScreen: false,
  autoPlay: false, //自动播放
  currentTime: 0,
  loop: false, //循环播放
  mirror: false, //镜像画面
  ligthOff: false, //关灯模式
  volume: 0.3, //默认音量大小
  control: true, //是否显示控制器
  title: "", //视频名称
  type: "video/mp4",
  // src: "https://test-streams.mux.dev/x36xhzz/x36xhzz.m3u8", //视频源
  // src: "https://cdn.jsdelivr.net/gh/xdlumia/files/video-play/IronMan.mp4", //视频源
  // src: "https://cn-gdfs-ct-01-03.bilivideo.com/upgcxcode/87/60/28334886087/28334886087-1-100050.m4s?e=ig8euxZM2rNcNbdlhoNvNC8BqJIzNbfq9rVEuxTEnE8L5F6VnEsSTx0vkX8fqJeYTj_lta53NCM=&uipk=5&nbs=1&deadline=1744643660&gen=playurlv2&os=bcache&oi=3070463774&trid=00009b9dacdd6a1549489a336346afee570eT&mid=85641118&platform=html5&og=hw&upsig=93d30896e0f016d27174e8cf426ab0e3&uparams=e,uipk,nbs,deadline,gen,os,oi,trid,mid,platform,og&cdnid=60903&bvc=vod&nettype=0&bw=168031&orderid=0,1&buvid=&build=0&mobi_app=&f=T_0_0&logo=80000000", //视频源
  // src: "https://cn-gdfs-ct-01-03.bilivideo.com/upgcxcode/87/60/28334886087/28334886087-1-192.mp4?e=ig8euxZM2rNcNbR17wdVhwdlhWRMhwdVhoNvNC8BqJIzNbfq9rVEuxTEnE8L5F6VnEsSTx0vkX8fqJeYTj_lta53NCM=&uipk=5&nbs=1&deadline=1744653499&gen=playurlv2&os=bcache&oi=3683093640&trid=0000254cd434374044a5898015bdf353f9b6T&mid=85641118&platform=html5&og=hw&upsig=7be27fc2952b4524ab7198d79678c7f5&uparams=e,uipk,nbs,deadline,gen,os,oi,trid,mid,platform,og&cdnid=60903&bvc=vod&nettype=0&bw=114619&orderid=0,1&buvid=&build=0&mobi_app=&f=T_0_0&logo=80000000",
  src:'https://cn-nmghhht-cu-01-14.bilivideo.com/upgcxcode/01/69/29335356901/29335356901-1-100050.m4s?e=ig8euxZM2rNcNbdlhoNvNC8BqJIzNbfq9rVEuxTEnE8L5F6VnEsSTx0vkX8fqJeYTj_lta53NCM=&uipk=5&nbs=1&deadline=1744655700&gen=playurlv2&os=bcache&oi=989390748&trid=0000caa4e653b09540ed9d09bfc6ff74f11fT&mid=85641118&platform=html5&og=cos&upsig=80a70fe551ce232db375b98250a38abf&uparams=e,uipk,nbs,deadline,gen,os,oi,trid,mid,platform,og&cdnid=7443&bvc=vod&nettype=0&bw=52278&orderid=0,1&buvid=&build=0&mobi_app=&f=T_0_0&logo=80000000',
  // audioSrc: "https://cn-gdfs-ct-01-03.bilivideo.com/upgcxcode/87/60/28334886087/28334886087-1-30232.m4s?e=ig8euxZM2rNcNbdlhoNvNC8BqJIzNbfq9rVEuxTEnE8L5F6VnEsSTx0vkX8fqJeYTj_lta53NCM=&uipk=5&nbs=1&deadline=1744643660&gen=playurlv2&os=bcache&oi=3070463774&trid=00009b9dacdd6a1549489a336346afee570eT&mid=85641118&platform=html5&og=hw&upsig=a0936de2a9a5c88cccb954aed19b3228&uparams=e,uipk,nbs,deadline,gen,os,oi,trid,mid,platform,og&cdnid=60903&bvc=vod&nettype=0&bw=10727&orderid=0,1&buvid=&build=0&mobi_app=&f=T_0_0&logo=80000000", //视频源
  audioSrc: "https://cn-nmghhht-cu-01-14.bilivideo.com/upgcxcode/01/69/29335356901/29335356901-1-30232.m4s?e=ig8euxZM2rNcNbdlhoNvNC8BqJIzNbfq9rVEuxTEnE8L5F6VnEsSTx0vkX8fqJeYTj_lta53NCM=&uipk=5&nbs=1&deadline=1744655700&gen=playurlv2&os=bcache&oi=989390748&trid=0000caa4e653b09540ed9d09bfc6ff74f11fT&mid=85641118&platform=html5&og=cos&upsig=d8ab563bf00e954c94654945a853a6c0&uparams=e,uipk,nbs,deadline,gen,os,oi,trid,mid,platform,og&cdnid=7443&bvc=vod&nettype=0&bw=11782&orderid=0,1&buvid=&build=0&mobi_app=&f=T_0_0&logo=80000000", //视频源
  // src: "https://logos-channel.scaleengine.net/logos-channel/live/biblescreen-ad-free/playlist.m3u8", //视频源
  poster: "https://cdn.jsdelivr.net/gh/xdlumia/files/video-play/ironMan.jpg", //封面
  controlBtns: [
    "audioTrack",
    "quality",
    "speedRate",
    "volume",
    "setting",
    "pip",
    "pageFullScreen",
    "fullScreen",
  ],
});
const video = ref(null);

nextTick(() => {
  console.log(video.value);
});
</script>

<style scoped>
</style>
