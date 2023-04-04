<template>
   <div class="to-do-list">
    <h1>TASKS ({{ amount }})</h1>

    <div> 
      <div class="input-group mb-3">
        <input 
          v-model="text" 
          type="text" 
          class="form-control" 
          placeholder="Task..." 
          aria-label="task" 
        >
        <button class="btn btn-primary" @click="addToList" >+ Add</button>
      </div>
    </div>

    <ul v-if="showToDoList"> 
      <li v-for="(item, index) in toDoList" :key="index">
        <p 
          :class="item.isComplete ? 'complete' : ''" 
          @click="toggleStatus(index)"
        >
            {{ item.title }}
        </p>
        <button class="danger" @click="onDelete(index)"> X </button>
      </li>
    </ul>

   </div>
</template>

<script setup>
  import { ref, computed, onMounted } from 'vue';

  // DATA

  const text = ref('');
  const toDoList = ref([]);

  // COMPUTED

  const amount = computed(() => toDoList.value.length);
  const showToDoList = computed(() => !!toDoList.value.length);

  // FUNCTIONS

  const addToList = () => {
    toDoList.value.push({ title: text.value, isComplete: true });
    localStorage.setItem('toDoList', JSON.stringify(toDoList.value));
  }

  const onDelete = (index) => {
    toDoList.value.splice(index, 1);
    localStorage.setItem('toDoList', JSON.stringify(toDoList.value));
  }

  const toggleStatus = (index) => {
    toDoList.value[index].isComplete = !toDoList.value[index].isComplete;
  }

  // LIFECYCLE HOOKS

  onMounted(() => {
    const list = localStorage.getItem('toDoList');
    const parsedList = JSON.parse(list)
    toDoList.value = parsedList;
  })

</script>

<style lang="scss" scoped>
@import "~bootstrap/scss/bootstrap";

.to-do-list {
  max-width: 600px;
  margin: auto;

  ul {
    list-style-type: none;
    margin-top: 25px;
    padding: 0;

    li {
      display: flex;
      border: 1px solid gray;

      p {
        width: -webkit-fill-available;
        background: #fafafa;
        padding: 5px 10px;
        cursor: pointer;
        margin: 0;

        &.complete {
          background: #e2fbe2;
          text-decoration: line-through;
        }
      }

      .danger {
        background: red;
        width: 50px;
        color: white;
        font-size: 20px;
        text-align: center;
        font-weight: bold;
      }
    }
  }
}

</style>

