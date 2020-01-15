<template>
    <div class="row">
        <button v-if="!isRecording" @click="startRecording">Start</button>
        <button v-if="isRecording" @click="stopRecording">Stop</button>
        <div>{{startTime}}</div>

        <template v-if="audioSource">
            <audio :src="audioSource" controls></audio>
            <button @click="deleteRecording">Discard</button>
        </template>

        <vue-dictaphone-spectrum-analyser/>

    </div>
</template>

<script>
    import Vue from "vue";
    import VueDictaphone from "vue-dictaphone";
    Vue.use(VueDictaphone);

    export default {
        name: "MyVoice2",
        data() {
            return {
                audioSource: undefined,
                isRecording: false,
                startTime: ""
            };
        },
        mounted(){
            navigator.mediaDevices.getUserMedia({ audio: true }).then(stream => {
                this.initMediaRecorder.call(this, stream);
            });
        },
        methods: {
            startRecording() {
                this.deleteRecording();
                this.isRecording = true;
                this.mediaRecorder.start();
                this.startTime = new Date();
            },
            stopRecording() {
                this.isRecording = false;
                this.mediaRecorder.stop();
                this.startTime = new Date();
            },
            deleteRecording() {
                this.isRecording = false;
                this.audioSource = null;
                this.startTime = "";
            },
            initMediaRecorder(stream) {
                const recorder = new MediaRecorder(stream);
                let chunks = [];

                recorder.addEventListener("stop", () => {
                    var blob = new Blob(chunks, { type: "audio/webm" });
                    chunks = [];
                    this.audioSource = URL.createObjectURL(blob);
                });

                recorder.addEventListener("dataavailable", e => {
                    chunks.push(e.data);
                });

                this.mediaRecorder = recorder;
            }
        }
    }
</script>