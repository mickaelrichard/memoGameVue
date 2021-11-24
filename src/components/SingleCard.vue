<template>
  <div class="card">
    <div :class="[flipped ? 'flipped' : '']">
      <img class="front" :src="getImgUrl(card.src)" alt="card front" />
      <img
        class="back"
        src="@/assets/cover.png"
        @click="handleClick"
        alt="cover"
      />
    </div>
  </div>
</template>
<script>
export default {
  name: "SingleCard",
  props: ["card", "flipped", "disabled"],
  data() {
    return {
      url: this.card.src,
    };
  },
  methods: {
    getImgUrl(pic) {
      return require("../assets/" + pic);
    },
    handleClick() {
      this.$emit("choice", this.card);
    },
  },
};
</script>
<style>
.card {
  position: relative;
}
.card img {
  width: 100%;
  display: block;
  border: 2px solid #fff;
  border-radius: 6px;
}

/* front of card - the picture */
.card .front {
  transform: rotateY(90deg);
  transition: all ease-in 0.2s;
  position: absolute;
}
.flipped .front {
  transform: rotateY(0deg);
  transition-delay: 0.2s;
}

/* back of card - cover */
.card .back {
  transition: all ease-in 0.2s;
  transition-delay: 0.2s;
}
.flipped .back {
  transform: rotateY(90deg);
  transition-delay: 0s;
}
</style>
