<template>
  <div v-if="blocks.length !== 0">
    <div class="resume-block inline" v-for="block in blocks" :key="block.id">
      <h1 v-if="block.type === 'title'">{{block.value}}</h1>
      <h2 v-else-if="block.type === 'subtitle'">{{block.value}}</h2>
      <div v-else-if="block.type === 'avatar'" class="avatar">
        <img :src="block.value" alt="Аватар">
      </div>
      <p v-else>{{block.value}}</p>
      <button class="btn danger" @click="$emit('remove-block', block.id)">Удалить</button>
    </div>
  </div>
  <div class="center" v-else>
    <h2>Документ ещё не заполнен.</h2>
    <h3>Добавьте первый блок</h3>
  </div>
</template>

<script>
export default {
  emits: ['remove-block'],
  props: ['blocks']
}
</script>

<style scoped>
  .avatar {
    display: flex;
    justify-content: center;
  }

  .avatar img {
    width: 150px;
    height: auto;
    border-radius: 50%;
  }

  .resume-block {

  }
</style>