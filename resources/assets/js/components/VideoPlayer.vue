<template>

	<video
		id="video"
		class="video-js vjs-default-skin vjs-big-play-centered vjs-16-9"
		controls
		preload="auto"
		data-setup='{ "fluid" : true, "preload" : "auto" }'
		:poster="thumbnailUrl"
	>
		<source type="video/mp4" :src="videoUrl" ></source>
	</video>

</template>
<script>
	import videojs from 'video.js'
	export default{

		data(){
			return: {
				player: null,
				duration: null
			}
		},

		props: {
			videoUrl: null,
			thumbnailUrl: null,
			videoUid: null
		},

		methods: {
			hasHitQuotaView () {
                if (!this.duration) {
                    return false;
                }

                return Math.round(this.player.currentTime()) === Math.round((10 * this.duration) / 100);
            },

            createView () {
                axios.post('/videos/' + this.videoUid + '/views');
            }
		},

		mounted(){
			this.player = videojs('video');

			this.player.on('loadedmetadata', () => {
				this.duration = Math.round(this.player.duration());
			});

			setInterval(() => {
				if(this.hasHitQuotaView()){
					this.createView();
				}
			},1000);
		}

	}
</script