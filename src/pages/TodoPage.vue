<template>
  <q-page class="bg-grey-3 column">
    <div class="row q-pa-sm bg-primary">
      <q-input v-model="newTask" @keyup.enter="addTask" filled placeholder="Add Task" bg-color="white" class="col" square dense>
        <template v-slot:append>
          <q-btn @click="addTask" round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list separator bordered class="bg-white">
      <q-item v-for="(task,index) in tasks" :key="task.title" @click="task.done = !task.done" clickable :class="{'done bg-blue-1': task.done}" v-ripple>
        <q-item-section avatar>
          <q-checkbox v-model="task.done" class="no-pointer-events" color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section side  v-if="task.done" @click.stop="deleteTask(index)">
          <q-btn flat dense round color="primary" icon="delete" />
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if="!tasks.length" class="no-tasks absolute-center">
      <q-icon name="check" color="primary" size="100px"></q-icon>
      <div class="text-h5 text-primary text-center">
        No Tasks
      </div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  data(){
    return{
      newTask:'',
      tasks:[
        // {
        //   title:'Get Bananas',
        //   done:false,
        // },
        // {
        //   title:'Eat Bananas',
        //   done:false,
        // },
        // {
        //   title:'Buy Bananas',
        //   done:false,
        // },
      ]
    }
  },
  methods:{
    deleteTask(index){
      this.$q.dialog({
        title: 'Confirm',
        message: 'Really Delete?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.tasks.splice(index,1);
        this.$q.notify('Task Deleted')
      })
    },
    addTask(){
      this.tasks.push({
        title:this.newTask,
        done:false
      })
      this.newTask=''
    }
  }
})
</script>
<style lang="scss">
.done{
  .q-item__label{
    text-decoration: line-through;
    color: #bbb;
  }
}
.no-tasks{
    opacity: 0.5;
  }
</style>
