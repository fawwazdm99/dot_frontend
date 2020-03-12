<template>
  <q-page-container class="q-pa-lg">
    <div class="jumbotron">
      <q-img src="../assets/jumbo.jpg" height="100vh">
        <div class="absolute-bottom text-subtitle1 text-center">
          <div class="text-center">
            <h3>Hi, Welcome to colleger</h3>
            <h6>Create your own college task and manage it!</h6>
          </div>
        </div>
      </q-img>
    </div>
    <div class="full-width row wrap justify-center items-start content-start q-pa-lg">
      <TaskList />
    </div>
  </q-page-container>
</template>

<script>
import TaskList from 'components/TaskList';
export default {
  name: 'Home',
  components:{
    TaskList
  },
  
  
}
</script>

<style lang="scss" scoped>
</style>
