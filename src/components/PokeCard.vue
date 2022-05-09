<script setup lang="ts">
defineProps<{
  name: string;
  id: number;
  image: string;
}>();
</script>

<template>
 <div class="card">
    <img
      :src="image"
      alt="Pokemon image"
    />
    <footer>
      <p>{{ name }}</p>
      <p>#{{id}}</p>
    </footer>
  </div>

</template>

<style scoped lang="scss">
.card {
  padding: 12px;
  width: 200px;
  background: #fff;
  display: flex;
  flex-direction: column;

  img {
    width: 100%;
    align-self: center;
  }

  footer {
    display: flex;
    justify-content: space-between;
  }

}



</style>
