<template>
  <div
    v-bind="$attrs"
    draggable
    :drag-in-progress="dragInProgress"
    @dragstart="startDrag"
    @dragend="endDrag"
    @touchstart="touchStart"
    @touchend="touchEnd"
    @touchmove="touchMove"
    :class="{ [classOnDrag]: dragInProgress }"
    :style="{ display: isInvisible ? 'none' : 'inherit' }"
    ref="draggable"
    style="position: relative"
  >
    <slot></slot>
  </div>
</template>

<script>
export default {
  name: "Draggable",
  props: {
    classOnDrag: String,
    payload: {
      type: Object,
      default: {},
    },
  },
  emits: ["startDrag", "endDrag"],
  data() {
    return {
      dragInProgress: false,
      isInvisible: false,
      originalPosition: {},
    };
  },
  computed: {
    elementWidth() {
      return this.$refs.draggable.offsetWidth;
    },
    elementHeight() {
      return this.$refs.draggable.offsetHeight;
    },
  },
  methods: {
    setOriginalPosition() {
      this.originalPosition.left = this.$refs.draggable.offsetLeft;
      this.originalPosition.top = this.$refs.draggable.offsetTop;
    },
    startDrag(event) {
      if (event.dataTransfer) {
        setTimeout(() => {
          this.isInvisible = true;
        }, 0);
        this.dragInProgress = true;
        event.dataTransfer.dropEffect = "move";
        event.dataTransfer.effectAllowed = "move";
        event.dataTransfer.setData("payload", JSON.stringify(this.payload));
      }
      this.$emit("startDrag", this.payload);
    },
    endDrag() {
      this.isInvisible = false;
      this.dragInProgress = false;
      this.$emit("endDrag", this.payload);
    },
    setElementPosition(top, left) {
      this.$refs.draggable.style.top = top;
      this.$refs.draggable.style.left = left;
    },
    touchStart(event) {
      this.$refs.draggable.style.position = "relative";
      this.startDrag(event);
    },
    touchMove(event) {
      if (!event.targetTouches) return;
      if (event.targetTouches.length !== 1) return;
      const touchLocation = event.targetTouches[0];
      this.setElementPosition(
        touchLocation.pageY -
          this.elementHeight / 2 -
          (this.originalPosition.top || 0) +
          "px",
        touchLocation.pageX -
          this.elementWidth / 2 -
          (this.originalPosition.left || 0) +
          "px"
      );
    },
    touchEnd(event) {
      this.$refs.draggable.style.position = "static";
      if (
        this.originalPosition.left !== undefined &&
        this.originalPosition.top !== undefined
      )
        this.setElementPosition(
          this.originalPosition.top,
          this.originalPosition.left
        );
      if (!event.changedTouches || event.changedTouches.length !== 1) return;
      const payload = this.payload;
      const touchLocation = event.changedTouches[0];
      const x = touchLocation.pageX;
      const y = touchLocation.pageY;
      const detail = {
        position: { x, y },
        payload,
      };
      const dropEvent = new CustomEvent("MobileDrop", {
        detail,
      });
      document.dispatchEvent(dropEvent);
      this.endDrag();
    },
  },
  mounted() {
    this.setOriginalPosition();
    window.addEventListener("resize", this.setOriginalPosition);
  },
  unmounted() {
    window.removeEventListener("resize");
  },
};
</script>
