<template>
  <div
    @drop="dropHandler"
    @dragenter.prevent
    @dragover.prevent
    id="drop-handler"
    v-bind="$attrs"
    ref="dropable"
  >
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: "Dropable",
  emits: ["onDrop"],
  data() {
    return {
      eventListener: null,
    };
  },
  computed: {
    left() {
      return this.$refs.dropable.offsetLeft;
    },
    right() {
      return this.$refs.dropable.offsetLeft + this.$refs.dropable.offsetWidth;
    },
    top() {
      return this.$refs.dropable.offsetTop;
    },
    bottom() {
      return this.$refs.dropable.offsetTop + this.$refs.dropable.offsetHeight;
    },
  },
  methods: {
    dropHandler(event) {
      event.preventDefault();
      const payload = JSON.parse(event.dataTransfer.getData("payload"));
      this.$emit("onDrop", payload);
    },
    areCoordinatesInElement(x, y) {
      const horizontal = x > this.left && x < this.right;
      const vertical = y > this.top && y < this.bottom;
      return horizontal && vertical;
    },
    registerEventListener() {
      const vm = this;
      this.eventListener = document.addEventListener(
        "MobileDrop",
        (dropEvent) => {
          if (!dropEvent.detail || !dropEvent.detail.position) return;
          const { x, y } = dropEvent.detail.position;
          if (!vm.areCoordinatesInElement(x, y)) return;
          vm.$emit("onDrop", dropEvent.detail.payload);
        }
      );
    },
  },
  mounted() {
    this.registerEventListener();
  },
  unmounted() {
    if (this.eventListener) document.removeEventListener(this.eventListener);
  },
};
</script>