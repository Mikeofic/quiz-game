<template>
  <div>
    <section class="score">
      Jogador <span>{{ winCount }}</span> x <span>{{ loseCount }}</span> Máquina
    </section>
  </div>
</template>

<script>
export default {
  props: ["winCount", "loseCount"],
};
</script>

<style lang="scss" scoped>
section.score {
  border-bottom: 1px solid #0048ae;
  padding: 24px;
  font-size: 18px;

  span {
    padding: 8px;
    font-weight: bold;
    border: 1px solid #0048ae;
  }
}
</style>
