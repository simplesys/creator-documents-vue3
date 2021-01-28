<template>
  <div class="alert container" v-if="alert" :class="alert.type">
    <button class="btn-close" aria-label="Закрыть" @click="$emit('close')"></button>
    <h2>{{alert.title}}</h2>
    <p>{{alert.text}}</p>
  </div>
</template>

<script>
export default {
  emits: ['close'],
  props: ['alert']
}
</script>

<style scoped>
  .btn-close {
    position: absolute;
    top: 0;
    right: 0;
    z-index: 2;
    padding: 1.25rem 1rem;
    box-sizing: content-box;
    width: 1em;
    height: 1em;
    color: #000;
    background: transparent url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 16 16' fill='%23000'%3e%3cpath d='M.293.293a1 1 0 011.414 0L8 6.586 14.293.293a1 1 0 111.414 1.414L9.414 8l6.293 6.293a1 1 0 01-1.414 1.414L8 9.414l-6.293 6.293a1 1 0 01-1.414-1.414L6.586 8 .293 1.707a1 1 0 010-1.414z'/%3e%3c/svg%3e") center/1em auto no-repeat;
    border: 0;
    border-radius: .25rem;
    opacity: .5;
    cursor: pointer;
  }
</style>