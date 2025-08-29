<template>
  <teleport to="#screen-overlays">
      <transition name="fade">
      <div v-if="visible" class="fixed top-5 end-5 bg-white text-gray-800 px-4 py-2 rounded-lg shadow-lg shadow-neutral-800 font-semibold text-lg" >
        {{ message }}
      </div>
    </transition>
  </teleport>
</template>

<script>
import emitter from '../eventBus';

export default {
  name: "Toast",
  data() {
    return {
      message: "",
      visible: false,
      timeoutId: null
    }
  },
  mounted() {
    emitter.on("toast", this.showToast)
  },
  beforeUnmount() {
    emitter.off("toast", this.showToast)
  },
  methods: {
    showToast(msg) {
      this.message = msg;
      this.visible = true;
      clearTimeout(this.timeoutId)
      this.timeoutId = setTimeout(() => {
        this.visible = false;
      }, 3000)
    }
  }
}
</script>

<style>
.fade-enter-active, .fade-leave-active {
  transition: opacity 0.3s;
}
.fade-enter-from, .fade-leave-to {
  opacity: 0;
}
</style>
