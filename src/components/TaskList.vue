<template>
  <q-page-container>
    <div class="row q-gutter-lg">
      <div class="order-sm-last">
        <form @submit.prevent="addTask" class="q-gutter-lg">
          <q-input bottom-slots label="Task Title" v-model="newTaskTitle" maxlength="35" counter>
            <template v-slot:prepend>
              <q-icon name="fas fa-font" />
            </template>
            <template v-slot:append>
              <q-icon name="close" @click="newTaskTitle = ''" class="cursor-pointer" />
            </template>
          </q-input>

          <q-input
            bottom-slots
            label="Task Description"
            v-model="newTaskDescription"
            type="textarea"
          >
            <template v-slot:prepend>
              <q-icon name="fas fa-align-left" />
            </template>
            <template v-slot:append>
              <q-icon name="close" @click="newTaskDescription = ''" class="cursor-pointer" />
            </template>
          </q-input>

          <q-date :landscape="$q.screen.gt.xs" v-model="newTaskDate" title="When it is?"></q-date>

          <q-btn type="submit">Submit</q-btn>
        </form>
      </div>
      <div>
        <div v-for="(task,index) in tasks" :key="task.id">
          <div class="row">
            <div class="col-8 task-list">
              <h6 class="list-title">{{task.title}}</h6>

              <p class="list-date">{{task.dateString}}</p>
              <p>{{task.description}}</p>
            </div>
            <div class="col">
              <q-icon name="fas fa-edit" class="remove-icon" @click="editTask(index)"></q-icon>
              <q-icon name="fas fa-times" class="remove-icon" @click="removeTask(index)"></q-icon>
            </div>
          </div>
        </div>
      </div>
    </div>
    <!-- some dialog -->
    <q-dialog v-model="popup" persistent>
      <q-card style="min-width: 350px">
        <q-card-section>
          <div class="text-h6">Edit my task</div>
        </q-card-section>

        <q-card-section class="q-pt-none">
          <q-input bottom-slots label="Task Title" v-model="editTaskTitle" maxlength="35" counter>
            <template v-slot:prepend>
              <q-icon name="fas fa-font" />
            </template>
          </q-input>
        </q-card-section>
        <q-card-section class="q-pt-none">
          <q-input bottom-slots label="Task Title" v-model="editTaskDescription" type="textarea">
            <template v-slot:prepend>
              <q-icon name="fas fa-font" />
            </template>
          </q-input>
        </q-card-section>
        <q-card-section class="q-pt-none">
          <q-date v-model="editTaskDate" title="When it is?"></q-date>
        </q-card-section>

        <q-card-actions align="right" class="text-primary">
          <q-btn flat label="Cancel" v-close-popup />
          <q-btn flat label="Save" v-close-popup @click="saveEditTask" />
        </q-card-actions>
      </q-card>
    </q-dialog>
    <q-dialog v-model="alert">
      <q-card>
        <q-card-section>
          <div class="text-h6">Oops!</div>
        </q-card-section>

        <q-card-section class="q-pt-none">Make sure you complete the form</q-card-section>

        <q-card-actions align="right">
          <q-btn flat label="OK" color="primary" v-close-popup />
        </q-card-actions>
      </q-card>
    </q-dialog>
    <!-- end of dialog -->
  </q-page-container>
</template>

<script>
import Vue from 'vue'

export default {
    name:'TaskList',
    data(){
    return {
      popup: false,
      alert:false,
      editTaskId : '',
      editTaskTitle : '',
      editTaskDescription:'',
      editTaskDate:'',
      editTaskIndex:'',

      newTaskTitle:'',
      newTaskDate:'',
      newTaskDescription:'',

      idTask:1,
      tasks:[]
    }
  },
  watch:{
    tasks:{
      handler(){
        
        localStorage.setItem('tasks', JSON.stringify(this.tasks))
        },
        deep:true,
        
    }
    
  },
  mounted:
    function(){
      console.log('APP Mounted!')
      if(localStorage.getItem('tasks')!=null||JSON.parse(localStorage.getItem('tasks').length!=0)){
        this.tasks = JSON.parse(localStorage.getItem('tasks'))
        this.idTask = this.tasks[this.tasks.length-1].id+1
      }
      
      
    }     
    
  ,
  methods:{
    dateConvert(taskDate){
      let indoDay = ['Minggu', 'Senin', 'Selasa', 'Rabu', 'Kamis', 'Jumat', 'Sabtu'];
        let indoMonth = ['Januari', 'Februari', 'Maret','April','Mei','Juni','Juli','Agustus','September','Oktober','November','Desember'];
          let date = new Date(taskDate).getDate();
          let day = new Date(taskDate).getDay();
          let month = new Date(taskDate).getMonth();
          let year = new Date(taskDate).getFullYear();
          let stringDate = indoDay[day]+' , '+date.toString()+' '+indoMonth[month]+' '+year.toString()
          return stringDate
    },
      addTask(){
        
        if(this.newTaskTitle.trim().length==0||this.newTaskDescription.trim().length==0||this.newTaskDate.trim().length==0){
          this.alert=true
          return
        }
        this.tasks.push({
            id:this.idTask,
            title:this.newTaskTitle,
            description:this.newTaskDescription,
            dateString:this.dateConvert(this.newTaskDate),
            date:this.newTaskDate
        })
        this.newTaskTitle=''
        this.newTaskDate=''
        this.newTaskDescription=''
        this.idTask++         
      },
      removeTask(index){
          this.tasks.splice(index,1)
      },
      editTask(index){
        this.popup = true
        console.log(this.tasks)
        this.editTaskId = this.tasks[index].id
        this.editTaskTitle = this.tasks[index].title
        this.editTaskDescription = this.tasks[index].description
        this.editTaskDate = this.tasks[index].date
        this.editTaskIndex=index
      },
      saveEditTask(){
        let editedTask = {
          id:this.editTaskId,
          title : this.editTaskTitle,
          description:this.editTaskDescription,
          dateString:this.dateConvert(this.editTaskDate),
          date:this.editTaskDate
        }
        Vue.set(this.tasks,this.editTaskIndex,editedTask)
        
      }
  }
}
</script>

<style lang="scss" scoped>
.task-list {
  margin-bottom: 1em;
}
.task-list p {
  margin-bottom: 0;
}
.list-date {
  color: $grey-7;
}
.list-title {
  font-weight: bold;
  text-transform: uppercase;
  margin-bottom: 0;
  margin-top: 0;
}
.remove-icon {
  cursor: pointer;
}
</style>