<script setup>
import boardService from '@/services/boardService';
import { reactive, onMounted } from 'vue';
import { useRoute } from 'vue-router';
import { useAuthenticationStore } from '@/stores/authentication';

const authentication = useAuthenticationStore();
const route = useRoute();
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

const doDelete = async () =>{

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
    <button>수정</button>
</div>

</template>

<style scoped>

</style>