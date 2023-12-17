<template>
  <div>
    <form @submit.prevent="storeForm" >
      <submit-btn :processloading="processingForm"
                  faiconposition="left"
                  stylebutton="btn_cl_left btn-green-md1"
                  textbutton="Search" ></submit-btn>
    </form>
  </div>
</template>
<script setup>
import { ref } from 'vue'
import SubmitBtn from '@/components/SubmitBtn.vue'
const processingForm = ref(false)
const storeForm = () => {
  processingForm.value = true
  // VERIFY ERRORS
  // PROCESSING FALSE
  const wait = setTimeout(() => {
    clearTimeout(wait)
    processingForm.value = false
  }, 2000)
}
</script>
<style scoped>
</style>
