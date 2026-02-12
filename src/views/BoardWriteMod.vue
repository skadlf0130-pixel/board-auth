<script setup>
import { reactive, onMounted } from 'vue';
import boardService from '@/services/boardService';
import { useRouter, useRoute} from 'vue-router';

const router = useRouter();
const route= useRoute();

const state = reactive({
    board: {
        title: '',
        contents: ''
    }
});

const submit = async () => {
    const result = await ( state.board.id 
                            ? boardService.putBoard(state.board) 
                            : boardService.postBoard(state.board) );
    console.log('result: ', result);
    router.push(`/board/${result.resultData}`);
}

onMounted( () => {    
    console.log('route.state.data: ', history.state.data);

    if(history.state.data) { //넘어왔는 데이터가 있다.
        state.board = history.state.data;
    }
} );




</script>

<template>
<h3>{{ state.board.id ? '글수정' : '글쓰기'}}</h3>
<div><input type="text" placeholder="제목" v-model="state.board.title"></div>
<div><textarea placeholder="제목" v-model="state.board.contents"></textarea></div>
<div><button @click="submit">{{ state.board.id ? '수정' : '등록'}}</button></div>
</template>

<style scoped>

</style>