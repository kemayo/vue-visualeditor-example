<template>
  <div class="hello">
    <button @click="onClick">{{ label }}</button>
  </div>
</template>

<script>
export default {
  name: 'SimpleButton',
  data() {
      return {
        label: "Push me",
        count: 0,
      };
  },
  methods: {
    onClick() {
        this.count++;
        this.label = "I was pushed (" + this.count + " times)";
    },
  },
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
</style>
