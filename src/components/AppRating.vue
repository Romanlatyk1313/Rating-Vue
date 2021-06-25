<template>
  <div>
    <div><h2>RATING US</h2></div>
    <div class="rating">
      <div class="reting__body">
        <div
          class="rating__active"
          v-bind:style="{ width: widthRating + '%' }"
        ></div>
        <div class="rating__items">
          <input
            type="radio"
            class="rating__item"
            @click="changeValue"
            :value="1"
            v-model="rating"
            name="rating"
          />
          <input
            type="radio"
            class="rating__item"
            @click="changeValue"
            :value="2"
            v-model="rating"
            name="rating"
          />
          <input
            type="radio"
            class="rating__item"
            @click="changeValue"
            :value="3"
            v-model="rating"
            name="rating"
          />
          <input
            type="radio"
            class="rating__item"
            @click="changeValue"
            :value="4"
            v-model="rating"
            name="rating"
          />
          <input
            type="radio"
            class="rating__item"
            @click="changeValue"
            :value="5"
            v-model="rating"
            name="rating"
          />
        </div>
      </div>
    </div>
    <div class="rating__value">{{ `${rating} of 5` }}</div>
  </div>
</template>

<script>
export default {
  name: "Rating",

  data() {
    return {
      rating: "0",
    };
  },
  computed: {
    widthRating() {
      return +this.rating / 0.05;
    },
  },

  methods: {
    changeValue() {
      this.$emit("changeValue", this.rating);
    },
  },
};
</script>

<style>
.rating {
  display: flex;
  align-items: flex-end;
  font-size: 80px;
  line-height: 0.75;
}
.reting__body {
  position: relative;
}
.reting__body::before {
  content: "★★★★★";
  display: block;
}
.rating__active {
  position: absolute;

  height: 100%;
  top: 0;
  left: 0;
  overflow: hidden;
}
.rating__active::before {
  content: "★★★★★";
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  color: #ffd300;
}
.rating__items {
  display: flex;
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
}

.rating__item {
  flex: 0 0 18%;
  height: 100%;
  opacity: 0;
}
.rating__value {
  font-size: 28px;
  color: black;
  text-align: center;
}
h2 {
  text-align: center;
}
</style>