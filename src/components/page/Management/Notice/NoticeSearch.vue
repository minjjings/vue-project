<!-- setup을 적어야 Composition API를 사용할 수 있다.  -->
<script setup>
import { onMounted, ref } from 'vue';
import router from '@/router';
import { useModalStore } from '../../../../stores/modalState';
const searchTitle = ref('');
const searchStDate = ref('');
const searchEdDate = ref('');
const modalState = useModalStore();
const handlerSearch = () => {
    const query = [];
    !searchTitle.value || query.push(`searchTitle=${searchTitle.value}`);
    !searchStDate.value || query.push(`searchStDate=${searchStDate.value}`);
    !searchEdDate.value || query.push(`searchEdDate=${searchEdDate.value}`);
    const queryString = query.length > 0 ? `?${query.join('&')}` : '';
    console.log(queryString);

    router.push(queryString);
};

//새로고침시 queryParam만 변경
//1. NoticeSearch라는 컴포넌트가 열릴 때, url에 쿼리 파람이 남아 있는지 확인
//2. 남아 있는 경우, 경로(queryPram을 제외)로 현재 url 대체
onMounted(() => {
    window.location.search && router.replace(window.location.pathname);
});
</script>

<template>
    <div class="search-box">
        <!-- v-model을 이용하여 양방향 바인딩을 쉽게 할 수 있다. -->
        <!-- lazy : focus가 해제되었을 때 model에 저장됨 -->
        <input v-model.lazy="searchTitle" />
        <input type="date" v-model="searchStDate" />
        <input type="date" v-model="searchEdDate" />
        <!-- v-on:click="" 또는 @click=""으로 이벤트를 설정한다. -->
        <button @click="handlerSearch">검색</button>
        <button @click="modalState.setModalState()">신규등록</button>
    </div>
</template>

<style lang="scss" scoped>
.search-box {
    margin-bottom: 10px;
    display: block;
    float: inline-end;
}

input {
    padding: 8px;
    margin-top: 5px;
    margin-bottom: 5px;
    margin-right: 5px;
    border-radius: 4px;
    border: 1px solid #ccc;
}

button {
    text-align: center;
    text-decoration: none;
    display: inline-block;
    border: none;
    color: white;
    width: 70px;
    padding-top: 8px;
    padding-bottom: 8px;
    font-size: 12px;
    margin: 4px 2px;
    cursor: pointer;
    border-radius: 12px;
    box-shadow: 0 4px #999;
    background-color: #3bb2ea;

    &:hover {
        background-color: #45a049;
    }

    &:active {
        background-color: #3e8e41;
        box-shadow: 0 2px #666;
        transform: translateY(2px);
    }
}
</style>
