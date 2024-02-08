<template>
    <div class="home">
        <v-form>
            <v-file-input
            label="Selecione legendas"
            prepend-icon="mdi-message-text"
            append-icon="mdi-send"
            outlined
            multiple
            v-model="files"
            @click:append="processSubtitles"
            >
            </v-file-input>
        </v-form>
        <div class="pills">
            <PillComponesnt v-for="item in groupedWords" :word="item.word"
            :amount="item.amount" :key="item.word"></PillComponesnt>
        </div>
    </div>
</template>

<script>
import PillComponesnt from './PillComponesnt.vue';
import { ipcRenderer } from 'electron';
export default {
    components:{
        PillComponesnt
    },
    data: function() {
        return {
            files:[],
            groupedWords:[]
        }
    },
    methods:{
        processSubtitles(){
            let paths = this.files.map(f=>f.path)
            ipcRenderer.send("process-subtitles",paths )
            ipcRenderer.on("process-subtitles", (event, resp)=>{
                this.groupedWords = resp
            })
        }
    }
}
</script>

<style>
.home{
    margin: 20px;
}

.pills {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
}

</style>