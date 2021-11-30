<template>
  <section class="container">
    <h1>Todoリスト</h1>
    <div class="addArea">
      <input type="text" name="addName" v-model="content" id="addName" placeholder="タスクを入力してください">
      <button id="addButton" class="button button--green" @click="insert">追加</button>
    </div>
    <div class="Filter">
      <td>
        
      </td>
    </div>
    <table class="Lists">
      <thead>
        <tr v-for="(item, index) in todos" :key="index">
          <td>{{ item.content }}</td>
          <td>{{ item.created }}</td>
          <button class="button"
                :class="{
                  'button--yet':item.state == '作業前',
                  'button--progress':item.state == '作業中',
                  'button--done':item.state == '完了'
                }"  
                @click="changeState(item)"
        >
        {{ item.state }}
        </button>
          <td><button class="button button--delete" @click="remove(item)">削除</button></td>
      </tr>         
      </thead>
      
    </table>
  </section>
</template>
<script>
import {mapState} from 'vuex';

export default {
  data: function() {
    return {
      content: "",
      state: ''
    }
  },
  computed: {
    ...mapState(['todos'])
  },
  methods: {
    insert() {
      if(this.content != '') {
        this.$store.commit('insert', {content: this.content});
        this.content = ''
      }
    },
    remove(todo) {
      this.$store.commit('remove', todo)
    },
    changeState(todo){
      this.$store.commit('changeState',todo)
    }
  }
}
</script>
