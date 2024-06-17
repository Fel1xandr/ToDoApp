<template>
  <body>
  <div id="app">
    <h1>Seznam úkolů</h1>
    <div class="task-wrapper">
    <!-- input zpouštějící funkci vytvoření nového úkolu "addTask" po stlačení klávesy "enter" -->
      <input v-model="newTask" @keyup.enter="addTask" placeholder="Přidej další úkol, například: umýt okna, nakoupit..." class="task-input"/>
      
    <!-- tlačítka pro filtrování úkolů, labely používám k jejich pojmenování -->
      <div class="filters">
        <label><input type="radio" value="all" v-model="filter"/> Všechny </label>
        <label><input type="radio" value="unfinished" v-model="filter"/> Nedokončené </label>
        <label><input type="radio" value="finished" v-model="filter"/> Dokončené </label>
      </div>

      <div class="tasks">
        <ul>
        <!-- v-for pro vyobrazení všech úkolů podle id -->
          <li v-for="task in filteredTasks" :key="task.id">
            <span :class="{ done: task.done }">{{ task.text }}</span>
            <div class="actions">
            <!-- tlačítko pro odstranění a checkbox pro označení jako hotové -->
              <button @click="removeTask(task.id)"> Odstranit </button>
              <input type="checkbox" v-model="task.done" />
            </div>
          </li>
        </ul>
      </div>
    </div>
  </div>
  </body>
</template>

<script>
export default {
  data() {
    return {
      newTask: '',
      tasks: [],
      nextId: 1,
      filter: 'all'
    };
  },
  computed: {
    //filtrování úkolů na všechny, nedokončené a dokončené
    filteredTasks() {
      switch (this.filter) {
        case 'unfinished':
          return this.tasks.filter(task => !task.done);
        case 'finished':
          return this.tasks.filter(task => task.done);
        default:
          return this.tasks;
      }
    }
  },
  methods: {
    //přidávání úkolu podle query z inputu, přiřazování nového id, vložení textu z query, nastavení jeho statusu na nedokončený a vyprázdnění inputu pro další zadávání
    addTask() {
      if (this.newTask.trim()) {
        this.tasks.push({
          id: this.nextId++,
          text: this.newTask.trim(),
          done: false
        });
        this.newTask = '';
      }
    },
    //odstranění úkolu podle jeho id
    removeTask(id) {
      this.tasks = this.tasks.filter(task => task.id !== id);
    }
  }
};
</script>

<style scoped>
#app {
  font-family: Arial, sans-serif;
  text-align: center;
  max-width: 500px;
  margin: 50px auto;
}
body { /* upravuje pozadí celé stránky */
  background-color: #06AED5;
  border-radius: 10px;
  display: flex;
  margin-top: 30px;
  margin: 30px 32% 0 32%;
}
h1 {
  color: #086788;
}
.task-wrapper { /* upravuje pozadí za úkoly */
  background-color: #FFF1D0; 
  padding: 20px;
  border-radius: 15px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}
.task-input { /* upravuje input box pro psaní úkolů */
  width: 100%;
  padding: 10px;
  margin: 10px 0;
  box-sizing: border-box;
  border-radius: 8px;
  border: 1px solid #ccc;
}
.filters {
  margin-bottom: 10px;
  display: flex;
  justify-content: space-around;
}
.filters input {
  margin-right: 5px;
  margin-left: 15px;
}
.tasks { /* mezery mezi jednotlivými úkoly */
  padding: 10px 0;
}
.actions {
  display: flex;
  align-items: center;
}
button { /* posunutí tlačítka pro odtranění úkolu */
  margin-left: 10px;
}
ul {
  padding: 0;
  list-style: none;
}
li { /* upravení celého listu vytvořených úkolů */
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 5px 0;
  margin-bottom: 5px;
}
.done { /* přeškrtnutí a zešedivění dokončeného úkolu*/
  text-decoration: line-through;
  color: gray;
}
</style>