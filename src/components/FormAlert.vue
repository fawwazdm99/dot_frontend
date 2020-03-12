<template>
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
</template>

<script>
export default {
    name:'formalert',
    data(){
        alert=false
    },
}
</script>

<style>
</style>