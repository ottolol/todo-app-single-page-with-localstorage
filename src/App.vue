<template>
  <div class="container">
    <h1>Мои заметки:</h1>

    <div class="input-group mb-3">
      <span class="input-group-text" id="basic-addon1">✎</span>
      <input
        type="text"
        class="form-control"
        placeholder="Введите название заметки"
        @keyup.enter="add"
        v-model="itemInput"
      />
    </div>

    <div class="list-group" v-for="(it, ix) in itemList" :key="ix">
      <label class="list-group-item">
        <input class="form-check-input me-1" type="checkbox" @click.prevent="done(ix)" />
        {{ ix + 1 }}. {{ it }}
        <button
          class="button btn-close"
          aria-label="Close"
          @click="remove(ix)"
        ></button>
      </label>
    </div>

    <br />
    <div>
      <!-- <h2 class="doneListTitle" v-if="doneList.length > 0">Выполненные заметки:</h2> -->
      <h2 class="doneListTitle" v-if="doneList.length">Выполненные заметки:</h2>

      <div v-for="(it, ix) in doneList" :key="ix">
        <div class="list-group-item list-group-item-info">
          <input class="form-check-input" type="checkbox" checked @click.prevent="back(ix)" />
          {{ it }}
          <button
            class="button btn-close"
            aria-label="Close"
            @click="removeDone(ix)"
          ></button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'App',
  data() {
    return {
      itemList: ['Купить продукты', 'Помыть полы', 'Покормить котов', 'Сготовить ужин', 'Вынести мусор'],
      itemInput: '',
      doneList: [],
    }
  },
  mounted() {
    this.loadState();
  },
  methods: {
    add() {
      // if (this.itemInput === '') {
      //   return;
      // }

      if (this.inputItem === '') return;

      this.itemList.push(this.itemInput);
      this.itemInput = '';

      this.saveState();
    },
    remove(ix) {
      this.itemList.splice(ix, 1);

      // this.itemList = this.itemList.filter((it, index) => ix !== index);

      // this.itemList = this.itemList.filter((it, index) => {
      //   if (ix === index) {
      //     return false;
      //   }
      //   return true;
      // });

      this.saveState();
    },
    done(ix) {
      let deletedItem = this.itemList.splice(ix, 1);
      deletedItem = deletedItem[0];
      this.doneList.push(deletedItem);

      // let [deletedItem] = this.itemList.splice(ix, 1);

      // let deletedItem = this.itemList.splice(ix, 1)[0];

      this.saveState();
    },
    back(ix) {
      let backItem = this.doneList.splice(ix, 1);
      backItem = backItem[0];
      this.itemList.push(backItem);

      this.saveState();
    },
    removeDone(ix) {
      this.doneList.splice(ix, 1);

      this.saveState();
    },
    saveState() {
      localStorage.setItem('data_todo-app-single-page-with-localstorage', JSON.stringify({ itemList: this.itemList, doneList: this.doneList }))
    },
    loadState() {
      let data = localStorage.getItem('data_todo-app-single-page-with-localstorage');
      if (data) {
        data = JSON.parse(data);
        this.itemList = data.itemList;
        this.doneList = data.doneList;
      }
    }
  },
}
</script>

<style>
input {
  outline: none;
}
body {
  background-color: azure;
}
.btn-close {
  box-sizing: inherit;
}
#app {
  margin-top: 60px;
}
</style>
