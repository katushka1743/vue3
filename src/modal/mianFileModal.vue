<script setup>
import { ref, reactive } from 'vue'
import modal from './modal.vue';

const local = reactive ({
  modalShow: false,
  modalText: 'ggg',
})

function fun() {
  local.modalShow = true

}




</script>



<template>
  <button @click="fun">Active</button>
  <div>
    <modal
    v-if="local.modalShow"
    modalType="answer"
    :modalText="local.modalText"
    @modalYes="local.modalShow=false"
    @modalNo="local.modalShow=false"
    />
  </div>
</template>




<style scoped>


</style>
