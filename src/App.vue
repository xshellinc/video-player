<template>
  	<div id="app">
		<h1>Video Player</h1>
		<video 
			controls
			id="video"
			type="video/mp4"
			src="./assets/big_buck_bunny_720p_surround.mp4"
			poster="https://peach.blender.org/wp-content/uploads/title_anouncement.jpg?x11217"
			width="620" 
		/>
		<div>{{`current speed: ${playbackSpeed}`}}</div>
		<button @click="play">{{ playing ? "Pause" : "Play" }}</button>
		<button @click="changeSpeed(-0.5)">speed down</button>
		<button @click="changeSpeed(0.5)">speed up</button>
		<button @click="rewinding = !rewinding; rewind();">rev</button>
  	</div>
</template>

<script>
export default {
	
	name: 'app',

	data() {
		return {
			playing: false,
			rewinding: false,
			playbackSpeed: 1.0,
			intervalRewind: null,
		}
	},

	created() {
		document.onkeydown = this.onkeydown
	},
	
	methods: {

		play() {
			var video = document.getElementById('video')
			this.playing ? video.pause() : video.play()
			this.playing = !this.playing
		},

		rewind() {
			var video = document.getElementById('video')
			if(this.rewinding) {
				video.playbackRate = 0
				setTimeout(() => { 
					var video = document.getElementById('video')
					video.currentTime -=(0.1)
					this.rewind()
				}, 90);
			} else {
				video.playbackRate = this.playbackSpeed
			}
		},

		stepBack() {
			var video = document.getElementById('video')
			video.currentTime -=0.1
		},

		stepForward() {
			var video = document.getElementById('video')
			video.currentTime +=0.1
		},

		onkeydown(e) {
			const { key } = e
			if(key === "ArrowLeft") {
				this.stepBack()
			} else if(key === "ArrowRight") {
				this.stepForward()
			}
		},

		changeSpeed(amount) {
			this.playbackSpeed = this.playbackSpeed += amount
			this.playbackSpeed = parseFloat(this.playbackSpeed.toFixed(2))
			console.log(this.playbackSpeed)
			var video = document.getElementById('video')
			video.playbackRate = this.playbackSpeed
		}
  	}
}
</script>

