<template>
    <div>
        <h1 class="title">Music Player</h1>

        <div class="music-container" id="music-container">
            <div class="music-info">
                <h4 id="title">{{ songTitle }}.mp3</h4>
                <div class="progress-container" id="progress-container">
                    <div class="progress" id="progress"><label id="minutes" style="font-size: 12px;">{{ minutesLabel
                    }}</label>:<label style="font-size: 12px;" id="seconds">{{ secondsLabel }}</label>


                    </div>

                </div>
            </div>
            <audio id="audio">
                <source :src="songs[0].song" type="audio/mpeg" />
            </audio>

            <div class="img-container">
                <img :src="songs[0].songImage" alt="music-cover" id="cover" />
            </div>
            <div class="navigation">
                <button id="prev" @click="prevSong" class="action-btn">
                    <i class="fas fa-backward"></i>
                </button>


                <button @click="playSound" id="play" class="action-btn action-btn-big">
                    <div v-if="!togglePlay">
                        <i class="fas fa-play"></i>
                    </div>
                    <div v-if="togglePlay">
                        <i class="fas fa-pause"></i>
                    </div>
                </button>


                <button @click="nextSong" id="next" class="action-btn">
                    <i class="fas fa-forward"></i>
                </button>
            </div>
        </div>
    </div>
</template>

<script lang="ts" setup>
import { onMounted, ref } from 'vue';
import Ranjithame from '../assets/Ranjithame.mp3'
import JimikkiPonnu from '../assets/JimikkiPonnu.mp3'
import TheeThalapathy from '../assets/TheeThalapathy.mp3'
import summer from "../assets/summer.jpg"
import hey from "../assets/hey.jpg"
import ukulele from "../assets/ukulele.jpg"


const songs = ref([
    {
        song: Ranjithame,
        songImage: summer,
        songName: "Ranjithame"
    },
    {
        song: JimikkiPonnu,
        songImage: hey,
        songName: "JimikkiPonnu"
    },
    {
        song: TheeThalapathy,
        songImage: ukulele,
        songName: "TheeThalapathy"
    }]);

const currentSongIndex = ref<number>(0)

const songTitle = ref<string>('')
const togglePlay = ref<boolean>(false)
var minutesLabel = ref(document.getElementById("minutes")) as any
var secondsLabel = ref(document.getElementById("seconds")) as any
var minutesLabelMinus = ref(document.getElementById("minutes-minus")) as any
var secondsLabelMinus = ref(document.getElementById("seconds-minus")) as any
const totalSeconds = ref(0) as any
const totalSecondsMinus = ref(0) as any
setInterval(setTime, 1000);
setInterval(setTimeMinus, 1000)

const playSound = (() => {

    let myAudio = document.getElementById("audio") as HTMLMediaElement;
    myAudio.addEventListener('timeupdate', updateProgress)
    if (myAudio != null) {

        togglePlay.value ? myAudio.pause() : myAudio.play();


        myAudio.onplaying = function () {
            togglePlay.value = true;
            document.querySelector(".music-container")?.classList.add("play")


        }
        myAudio.onpause = function () {
            togglePlay.value = false;
            document.querySelector(".music-container")?.classList.remove("play")

        }


    }

    songTitle.value = songs.value[currentSongIndex.value].songName

    let img = document.getElementById("cover") as HTMLMediaElement;
    img.src = songs.value[currentSongIndex.value].songImage

    myAudio.addEventListener('timeupdate', updateProgress)
    // myAudio.addEventListener('timeupdate', decreaseDuration)
}
)



const nextSong = () => {

    let myAudio = document.getElementById("audio") as HTMLMediaElement;
    currentSongIndex.value++;
    myAudio.src = songs.value[currentSongIndex.value].song

    myAudio.play();
    if (currentSongIndex.value > songs.value.length - 1) {
        currentSongIndex.value = 0;
    }
    songTitle.value = songs.value[currentSongIndex.value].songName
    let img = document.getElementById("cover") as HTMLMediaElement;
    img.src = songs.value[currentSongIndex.value].songImage

    totalSeconds.value = 0;


    setTime()
    setTimeMinus()

}
const prevSong = () => {

    let myAudio = document.getElementById("audio") as HTMLMediaElement;
    currentSongIndex.value--;
    myAudio.src = songs.value[currentSongIndex.value].song
    myAudio.play();
    if (currentSongIndex.value > songs.value.length - 1) {
        currentSongIndex.value = 0;
    }

    songTitle.value = songs.value[currentSongIndex.value].songName
    let img = document.getElementById("cover") as HTMLMediaElement;
    img.src = songs.value[currentSongIndex.value].songImage
    totalSeconds.value = 0;
    setTime();
    setTimeMinus()
}


function setTime() {
    ++totalSeconds.value;
    secondsLabel.value = getMinutesSeconds(totalSeconds.value % 60);
    minutesLabel.value = getMinutesSeconds(parseInt(totalSeconds.value / 60));

}
function setTimeMinus() {
    totalSecondsMinus.value = --totalDuration.value;
    secondsLabelMinus.value = getMinutesSeconds(totalSecondsMinus.value % 60);
    minutesLabelMinus.value = getMinutesSeconds(parseInt(totalSecondsMinus.value / 60));
}

function getMinutesSeconds(val: number) {
    var valString = val + "";
    if (valString.length < 2) {
        return "0" + valString;
    } else {
        return valString;
    }
}

const totalDuration = ref(0);



const updateProgress = (e: any) => {
    const { duration, currentTime } = e.srcElement;

    const progressPercent = (currentTime / duration) * 100;
    const progress = document.getElementById("progress");
    progress!.style.width = `${progressPercent}%`;
    totalDuration.value = duration;

}





</script>