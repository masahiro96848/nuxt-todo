<template>
  <section class="container">
    <h1>Todoリスト</h1>
    <div class="addArea">
      <input type="text" name="addName" v-model="content" id="addName" placeholder="タスクを入力してください">
      <button id="addButton" class="button button--green" @click="insert">追加</button>
    </div>
    <div class="Filter">
      <td>
        <button class="button button--gray" v-bind:class="{'is-active':(!find_flg)}" @click="flag_reset">全て</button>
        <button class="button button--gray" v-bind:class="{'is-active':find_flg && (find_state == '作業前')}" @click="find('作業前')">作業前</button>
        <button class="button button--gray" v-bind:class="{'is-active':find_flg && (find_state == '作業中')}" @click="find('作業中')">作業中</button>
        <button class="button button--gray" v-bind:class="{'is-active':find_flg && (find_state == '完了')}" @click="find('完了')">完了</button>
      </td>
    </div>
    <table class="Lists">
      <thead>
        <tr v-for="(item, index) in display_todos" :key="index">
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
      find_state: '',
      find_flg: '',
    }
  },
  computed: {
    ...mapState(['todos']),
    display_todos: function(){
      if(this.find_flg){
        let arr = [];
        let data = this.todos;
        data.forEach(element => {
          if(element.state == this.find_state) {
            arr.push(element)
          }
        });
        return arr;
      }else{
        return this.todos;
      }
    }
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
    },
    find(findState){
      this.find_state = findState
      this.find_flg = true
    },
    flag_reset(){
      this.find_flg = false
    }
  }
}
</script>
