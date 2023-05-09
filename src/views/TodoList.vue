<script>
import Button from '../components/Button.vue';

let id = 0;

export default {
  name: 'TodoList',
  data() {
    return {
      newItem: "",
      items: [],
      hideCompleted: false,
    };
  },
  components: {
    Button
  },
  computed: {
    filteredTodos() {
      return this.hideCompleted
        ? this.items.filter((i) => !i.done)
        : this.items
    },
    completed(){
      return this.items.filter((i)=>
       i.done
      )
    },
  },
  mounted() {
    if (localStorage.getItem("items")) {
      this.items = JSON.parse(localStorage.getItem("items"));
    }
  },
  methods: {
    addItem() {
      if (this.newItem) {
        this.items.unshift({
          id: id++,
          name: this.newItem,
          done: false,
        });
        this.newItem = "";
        localStorage.setItem("items", JSON.stringify(this.items));
      }
    },
    removeItem(id) {
      this.items.splice(id, 1);
      localStorage.setItem("items", JSON.stringify(this.items));
    },
    clearAll(){
     this.items = [];
     localStorage.setItem("items", JSON.stringify(this.items));
    },
    updateItems() {
      localStorage.setItem("items", JSON.stringify(this.items));
    }
  }
};
</script>

<template>
  <section class="TODO">
    <h1>TodoList</h1>
    <form @submit.prevent="addItem">
      <input v-model="newItem" placeholder="Add tasks">
      <Button class="sbt_button" type="submit">Добавить</Button>
    </form>
    <ul v-if="items.length">
      <li v-for="(item, id) in filteredTodos" :key="id">
        <input @change="updateItems()" type="checkbox" v-model="item.done" />
        <span :class="{ done: item.done }">
          {{ item.name }}
        </span>
        <Button class="rem_button" @click="removeItem(id)">Удалить</Button>
      </li>
    </ul> 
      <section v-else > Список дел пуст</section>
    <Button v-if="completed.length > 0" @click="hideCompleted = !hideCompleted">
      {{ hideCompleted ? 'Показать все задачи' : 'Скрыть завершённые задачи' }}
    </Button>
    <Button @click="clearAll()">
        Очистить всё
      </Button>
  </section>


</template>

<style>
.TODO{
  display: flex;
  flex-direction: column;
  align-items: center;
  font-size: 40px;
  padding: 10px;
  color: rgb(0, 0, 0);
}

input{
  border: 2px solid #443d3d;
  border-radius: 3px ;
  padding: 5px;
  font-size: 20px;
  background: #e4dcdc;
  color: rgb(22, 21, 21);
}

.sbt_button{
  background: #5E856D ;
  color: rgb(17, 16, 16);
}

.rem_button{
  background: #a81b1b;
  color: white;
}

li{
  min-width: 500px;
  min-height: 50px;
  background-color: #d3cccc98;
  border-radius: 10px;
  margin-top: 15px;
  list-style-type: none;
  font-size: 30px;
}

span{
  margin-right: 15px;
}


.done{
  text-decoration: line-through;
}
</style>
