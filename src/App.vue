<template>
  <div>
    <h1>Memo List</h1>
    <p v-for="(memo, i) in memos" v-bind:key="i">
      <a class="memo-list" v-on:click="showMemo(i)">{{ memo.split('\n', 1).join() }}</a>
    </p>
    <p>
      <a class="memo-list" v-on:click="newMemo()">+</a>
    </p>
    <br>
    <h2>Edit Memo</h2>
    <label>
      <textarea rows="10" cols="30" v-model="memoForm"></textarea>
    </label>
    <br>
    <a class="border-button" v-on:click="addMemo()">Save</a> <a class="border-button" v-on:click="deleteMemo()">Delete</a>
  </div>
</template>

<script>
export default {
  data() {
    return {
      memos: [],
      memoIndex: null,
      editFlag: false,
      memoForm: '',
    }
  },

  mounted() {
    if (localStorage.getItem('memos')) {
      try {
        this.memos = JSON.parse(localStorage.getItem('memos'))
      } catch(e) {
        localStorage.removeItem('memos')
      }
    }
  },

  methods: {
    showMemo(i) {
      this.memoForm = this.memos[i]
      this.memoIndex = i
      this.editFlag = true
    },

    newMemo() {
      this.memoForm = ''
      this.editFlag = false
    },

    addMemo() {
      if (!this.memoForm) {
        return
      }

      if (this.editFlag) {
        this.memos[this.memoIndex] = this.memoForm
        this.memoIndex = null
      }
      else {
        this.memos.push(this.memoForm)
      }
      this.saveMemo()
      this.memoForm = ''
      this.editFlag = false
    },

    deleteMemo() {
      if (this.editFlag) {
        this.memos.splice(this.memoIndex, 1)
        this.saveMemo()
      }
      this.memoForm = ''
      this.memoIndex = null
      this.editFlag = false
    },

    saveMemo() {
      const saveJson = JSON.stringify(this.memos)
      localStorage.setItem('memos', saveJson)
    }
  }
}
</script>

<style>
div{
  border: none
}
h1{
  color: #808080
}
h2{
  color: #4682b4
}
p{
  color: #808080
}
textarea {
  resize: none;
  border-color: #4682b4
}
.memo-list:hover {
  background: #808080;
  color: white;
}
.border-button {
  display: inline-block;
  padding: 0.3em 1em;
  text-decoration: none;
  color: #4682b4;
  border: solid 2px #4682b4;
  border-radius: 3px;
  transition: .4s;
}
.border-button:hover {
  background: #4682b4;
  color: white;
}
</style>
