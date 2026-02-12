<script setup>
import { onMounted, reactive } from 'vue';
import { useRoute, useRouter } from 'vue-router';
import { useAuthenticationStore } from '@/stores/authentication';
import boardService from '@/services/boardService';

const route = useRoute();
const router = useRouter();

const authentication = useAuthenticationStore();


const state = reactive({
    data : {
        id :0,
        userId: 0,
        title : '',
        contents: '',
        createdAt: '',
        nm:''
    }
})

onMounted( async () => {    
    state.data = await boardService.getBoard( route.params.id);
});

const doDelete = async () => {
    if(!confirm('삭제하시겠습니까?')) { return; }
    const params = {
        id: route.params.id
    }
    const result = await boardService.delBoard(params);
    if(result.resultData) {
        router.push('/board/list');
    } else {
        alert(result.resultMessage);
    }
}

const goToMod = () => {    
    router.push({
        path: '/board/write',
        state: {
            data: {
                id: route.params.id
                , title: state.data.title
                , contents: state.data.contents
            }
        }
    });
}




</script>

<template>
<h3>게시판 디테일</h3>
<div>No.{{ state.data.id }}</div>
<div>userId.{{ state.data.userId }}</div>
<div>title.{{ state.data.title }}</div>
<div>contents.{{ state.data.contents }}</div>
<div>createdAt.{{ state.data.createdAt }}</div>
<div>name.{{ state.data.nm }}</div>
<div v-if="state.data.userId === authentication.state.signedUser.signedUserId">
    <button @click="doDelete">삭제</button>    
    <button @click="goToMod">수정</button>
    <button>목록</button>

</div>

</template>

<style scoped>

</style>