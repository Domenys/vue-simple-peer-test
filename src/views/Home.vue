<template>
  <div class="home">
    <video id="testVideo"></video>
  </div>
</template>

<script>
import SimplePeer from 'simple-peer'
export default {
  name: 'Home',
  created() {
    this.getMedia()
  },
  methods: {
    getMedia() {
      navigator.mediaDevices.getUserMedia({
        video: true,
        audio: true
      }).then(this.gotMedia).catch(() => {})
    },
    gotMedia (stream) {
      let peer1 = new SimplePeer({ initiator: true, stream: stream })
      let peer2 = new SimplePeer()

      peer1.on('signal', data => {
        peer2.signal(data)
      })

      peer2.on('signal', data => {
        peer1.signal(data)
      })

      peer2.on('stream', stream => {
        var video = document.querySelector('video')

        if ('srcObject' in video) {
          video.srcObject = stream
        } else {
          video.src = window.URL.createObjectURL(stream)
        }

        video.play()
      })
    }
  }
}
</script>
