<template>
  <q-page class='bg-grey-3 column'>
    <div class='row q-pa-sm bg-primary'>
      <q-input @keyup.enter='addTask' bg-color='white' class='col' square filled  v-model="newTask" placeholder="Add task" dense>

        <template v-slot:append>
          <q-btn @click='addTask' round dense flat icon="add" />
        </template>
      </q-input>
    </div>
    <q-list separator bordered class='bg-white'>
      <q-item
        clickable
        @click='task.done = !task.done'
        v-ripple v-for='(task, index) in tasks'
        :key='task.title'
        :class="{'done bg-blue-1' : task.done}"
      >
        <q-item-section avatar>
          <q-checkbox class='no-pointer-events ' v-model="task.done" val="teal" color="primary" />
        </q-item-section>
        <q-item-section>
          <q-item-label>{{ task.title }}</q-item-label>
        </q-item-section>
        <q-item-section side v-if='task.done'>
          <q-btn @click.stop='deleteTask(index)' flat round color='danger' dense icon='delete' />
        </q-item-section>
      </q-item>
    </q-list>
    <div v-if='!tasks.length' class='no-task absolute-center'>
      <q-icon name='check' color='primary' size='100px' />
      <div class='text-h5 text-primary text-center'>
        No tasks
      </div>
    </div>
  </q-page>
</template>

<script>
import { defineComponent } from 'vue'

export default defineComponent({
  name: 'IndexPage',
  data(){
    return {
      newTask: '',
      tasks: []
    }
  },
  methods: {
    deleteTask(index){
      this.$q.dialog({
        title: 'Confirm',
        message: 'Are you sure?',
        cancel: true,
        persistent: true
      }).onOk(()=>{
        this.tasks.splice(index, 1)
        this.$q.notify('Task deleted')
      })
    },
    addTask(){
      this.tasks.push({
        title: this.newTask,
        done: false
      })
      this.newTask = ''
    }
  }
})
</script>
<style>
.done{
  .q-item__label{
    text-decoration: line-through;
    color: #bbb;
  }
}
</style>
