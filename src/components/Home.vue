<template>
    <div class="container">
        
        <audio ref="backgroundMusic" src="/music/相爱-脆莓乐队.mp3"></audio>
        <Transition name="fade">
            <button v-if="!isPlaying" @click="playMusic" class="play-button">开启</button>
        </Transition>

        <div v-if="isPlaying" class="content">
            hhhhhhh
        </div>
    </div>
</template>

<script>
import confetti from 'canvas-confetti';

export default{
    name: 'Home.vue',
    data(){
        return{
            isPlaying:false,
        };
    },
    mounted(){
        this.playMusic();
    },
    methods:{
        playMusic(){
            const audio =this.$refs.backgroundMusic;
            if(audio){
                audio.play()
                .then(()=>{
                    this.isPlaying=true;
                })
                .then(()=>{
                    this.launchFireworks();
                })
                .catch(error=>{
                    console.error('播放失败',error);
                });
            }
        },
        launchFireworks() {
            const duration = 5 * 1000;
            const animationEnd = Date.now() + duration;
            const defaults = { startVelocity: 30, spread: 360, ticks: 60, zIndex: 0 };

            function randomInRange(min, max) {
                return Math.random() * (max - min) + min;
            }

            const interval = setInterval(function() {
                const timeLeft = animationEnd - Date.now();

                if (timeLeft <= 0) {
                return clearInterval(interval);
                }

                const particleCount = 50 * (timeLeft / duration);
                confetti(Object.assign({}, defaults, { particleCount, origin: { x: randomInRange(0.1, 0.3), y: Math.random() - 0.2 } }));
                confetti(Object.assign({}, defaults, { particleCount, origin: { x: randomInRange(0.7, 0.9), y: Math.random() - 0.2 } }));
            }, 250);
        }
    }
}
</script>

<style scoped>
/* 渐变效果 */
.fade-enter-active, .fade-leave-active {
  transition: opacity 1s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active in <2.1.8 */ {
  opacity: 0;
}

/* 容器样式 */
.container {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  text-align: center;
}

/* 按钮样式 */
.play-button {
  padding: 15px 30px;
  font-size: 99px;
  font-weight: bold;
  color: white;
  background-color: #007bff;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  transition: background-color 0.3s, transform 0.3s;
}

.play-button:hover {
  background-color: #0056b3;
  transform: scale(1.05);
}

.play-button:active {
  background-color: #004085;
}

/* 其他内容样式 */
.content {
  font-size: 24px;
  color: #333;
}
</style>

