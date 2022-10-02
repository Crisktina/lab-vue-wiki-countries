<template>

  <div class="d-flex flex-column">
    <div class="d-flex justify-content-center mb-4">
        <div class="spinner-border mb-4" role="status"></div>
  </div>
  <p class="text-center">{{text}}</p>
  </div>
</template>

<script>
defineProps({
  text:{
    type:String,
  },
});
</script>