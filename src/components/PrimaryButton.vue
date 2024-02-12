<template>
  <button class="primary-btn" type="button" @click="handleClick">
    {{ this.btnText }}
  </button>
</template>

<script>
export default {
  props: ["btnText"],
  methods: {
    handleClick() {
      this.$emit("click");
    },
  },
};
</script>

<style scoped lang="scss">
.primary-btn {
  margin-top: 12px;
  height: 40px;
  min-width: 120px;
  padding: 0 16px;
  color: #fff;
  font-size: 16px;
  background-color: #1867c0;
  border: 1px solid #1867c0;
  border-radius: 4px;
  cursor: pointer;
  transition: background-color 0.3s ease;

  &:hover {
    background-color: #29e0a9;
  }
}
</style>
