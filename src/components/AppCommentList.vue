<template>
  <div class="container">
    <div class="card center" v-if="comments.length === 0">
      <h4>Комментариев пока нет</h4>
      <p><button class="btn primary" @click="$emit('load-comments')">Загрузить комментарии</button></p>
    </div>
    <div v-else class="card">
      <h2>Комментарии</h2>
      <ul class="list" v-for="comment in comments" :key="comment.id">
        <li class="list-item">
          <div>
            <p><strong>{{comment.email}}</strong></p>
            <small>{{comment.body}}</small>
          </div>
        </li>
      </ul>
    </div>
    <div class="loader" v-if="isLoading"></div>
  </div>
</template>

<script>
import AppLoader from "@/components/AppLoader";

export default {
  components: {AppLoader},
  emits: ['load-comments'],
  props: ['comments', 'isLoading']
}
</script>
