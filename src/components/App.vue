<template>
  <app-alert :alert="alert" @close="alert = null"></app-alert>
  <div class="container column">
    <form class="card card-w30" @submit.prevent="addBlock">
      <div class="form-control">
        <label for="type">Тип блока</label>
        <select id="type" v-model="newBlockType">
          <option value="title">Заголовок</option>
          <option value="subtitle">Подзаголовок</option>
          <option value="avatar">Аватар</option>
          <option value="text">Текст</option>
        </select>
      </div>
      <div class="form-control">
        <label for="value">Значение</label>
        <textarea v-model.trim="newBlockValue" id="value" rows="3"></textarea>
      </div>
      <button class="btn primary" type="submit" :disabled="newBlockValue.length < 3">Добавить</button>
    </form>

    <div class="card card-w70">
      <app-loader v-if="loadingBlocks"></app-loader>
      <app-block-list v-else :blocks="blocks" @remove-block="removeBlock"></app-block-list>
    </div>
  </div>
  <app-comment-list :comments="comments" :is-loading="loadingComments" @load-comments="loadComments"></app-comment-list>
</template>

<script>
import AppAlert from "@/components/AppAlert"
import AppBlockList from "@/components/AppBlockList"
import AppCommentList from "@/components/AppCommentList"
import AppLoader from "@/components/AppLoader"
import axios from "axios"

export default {
  components: {
    AppAlert,
    AppBlockList,
    AppCommentList,
    AppLoader
  },
  data() {
    return {
      alert: null,
      blocks: [],
      comments: [],
      commentsUrl: 'https://jsonplaceholder.typicode.com/comments?_limit=42',
      databaseUrl: 'https://minin-vue3-module8-default-rtdb.europe-west1.firebasedatabase.app/resume',
      loadingBlocks: false,
      loadingComments: false,
      newBlockValue: '',
      newBlockType: 'title'
    }
  },
  methods: {
    async addBlock() {
      const {data} = await axios.post(
        `${this.databaseUrl}.json`,
        JSON.stringify({type: this.newBlockType, value: this.newBlockValue})
      )
      console.log(data)
      this.blocks.push({
        id: data.name,
        type: this.newBlockType,
        value: this.newBlockValue
      })
      this.newBlockValue = ''
      this.newBlockType = 'title'
    },
    async loadBlocks() {
      try {
        this.loadingBlocks = true
        const {data} = await axios.get(`${this.databaseUrl}.json`)
        console.log(data)
        this.blocks = Object.keys(data).map(key => {
          return {
            id: key,
            ...data[key]
          }
        })
        this.loadingBlocks = false
      } catch (e) {
        this.loadingBlocks = false
      }
    },
    async loadComments() {
      try {
        this.loadingComments = true
        const {data} = await axios.get(this.commentsUrl)

        if (!data) {
          throw new Error('Список комментариев пуст')
        }

        this.comments = Object.keys(data).map(key => {
          return {
            id: key,
            ...data[key]
          }
        })
        this.loadingComments = false
      } catch (e) {
        this.alert = {
          type: 'danger',
          title: 'Ошибка загрузки комментариев',
          text: e.message
        }
        this.loadingComments = false
      }
    },
    async removeBlock(id) {
      try {
        await axios.delete(`${this.databaseUrl}/${id}.json`)
        this.blocks = this.blocks.filter(block => block.id !== id)
        this.alert = {
          type: 'primary',
          text: `Блок успешно удалён`
        }
      } catch (e) {
        this.alert = {
          type: 'danger',
          text: 'Ошибка при удалении блока ' + e.message
        }
      }
    }
  },
  mounted() {
    this.loadBlocks()
  }
}
</script>

