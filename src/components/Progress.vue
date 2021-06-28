<template>
  <div class="line">
    <div class="progress" :style="{
				width: progression + '%',
				background: length > 10 ? '#d9ff57' : '#cfe'
			}"></div>
  </div>
</template>

<script>
export default {
  name: "Counter",
  props: ['progression', 'length'],
  data(){
    return{

    }
  }
}
</script>

<style scoped>
  .line {
    border: 2px solid #ccc;
    height: 20px;
    border-radius: 3px;
    margin-bottom: 10px;
  }
  .progress{
    width: 50%;
    height: 100%;
    transition: 1s;
  }
</style>