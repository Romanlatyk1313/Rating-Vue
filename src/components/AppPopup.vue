<template>
  <div>
    <div v-if="isOpen" class="backdrop">
      <div class="popup" @click.stop>
        <slot></slot>
        <div>
          <slot name="actions" :confirm="confirm">
            <div>
              <button @click="confirm">Ok</button>
            </div>
          </slot>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    isOpen: {
      type: Boolean,
      required: true,
    },
  },
  emits: {
    ok: null,
    close: null,
  },

  mounted() {
    document.addEventListener("keydown", this.handleKeydown);
  },
  beforeUnmount() {
    document.removeEventListener("keydown", this.handleKeydown);
  },

  methods: {
    handleKeydown(e) {
      if (this.isOpen && e.key === "Escape") {
        this.close();
      }
    },

    close() {
      this.$emit("close");
    },
    confirm() {
      this.$emit("ok");
    },
  },
};
</script>

<style>
.popup {
  top: 40%;
  width: 400px;
  height: 200px;
  left: 40%;
  position: absolute;
  display: flex;
  justify-content: center;
  align-items: flex-end;
  z-index: 101;
  background-color: white;
  border-radius: 10px;
}

.backdrop {
  position: fixed;
  top: 0;
  left: 0;
  bottom: 0;
  right: 0;
  background-color: rgba(0, 0, 0, 0.8);
  z-index: 100;
}
</style>