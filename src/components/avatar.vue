<template>
  <div class="v-avatar">
    <div
      :style="{ borderColor: `var(--${color})` }"
      class="wrapper">
      <img
        v-if="src != null && !error"
        v-show="!error && loading === false"
        :src="src"
        :alt="alt"
        @load="loading = false"
        @error="onImageLoadingError">
      <i
        v-else
        class="material-icons">person</i>
    </div>
    <div
      v-if="indicator"
      :style="{ backgroundColor: `var(--${color})` }"
      class="indicator"/>
  </div>
</template>

<script>
export default {
  name: "v-avatar",
  props: {
    src: {
      type: String,
      default: null
    },
    alt: {
      type: String,
      required: true
    },
    indicator: {
      type: Boolean,
      default: false
    },
    color: {
      type: String,
      default: "accent"
    },
    size: {
      type: Number,
      default: 40
    }
  },
  data() {
    return {
      loading: false,
      error: false
    };
  },
  methods: {
    onImageLoadingError(error) {
      this.error = error;
      this.loading = false;
    }
  }
};
</script>

<style lang="scss" scoped>
.v-avatar {
  position: relative;
  width: 40px;
  height: 40px;
}

.wrapper {
  border-radius: 50%;
  border: 3px solid;
  height: 100%;
  width: 100%;
  overflow: hidden;
}

img {
  height: 100%;
  width: 100%;
  object-fit: cover;
}

i {
  position: absolute;
  left: 0;
  right: 0;
  top: 50%;
  transform: translateY(-55%);
  color: var(--accent);
  opacity: 0.7;
  text-align: center;
}

.indicator {
  position: absolute;
  bottom: 3%;
  right: 3%;
  border-radius: 50%;
  border: 2px solid white;
  width: 12px;
  height: 12px;
}
</style>
