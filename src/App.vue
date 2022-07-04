<template>
  <div class="app">
    <dropable class="my-dropable" @onDrop="dropHandler">
      <draggable
        @startDrag="startDragHandler"
        @endDrag="endDragHandler"
        :payload="{ testKey: 'testVal' }"
        class="my-draggable"
        class-on-drag="is-dragging"
      />
    </dropable>
    <dropable class="my-dropable" @onDrop="dropHandler"> </dropable>
    <dropable class="my-dropable" @onDrop="dropHandler">
      <draggable
        @startDrag="startDragHandler"
        @endDrag="endDragHandler"
        :payload="{ testKey: 'testVal' }"
        class="my-draggable"
        class-on-drag="is-dragging"
      />
    </dropable>
    <dropable class="my-dropable" @onDrop="dropHandler"> </dropable>
    <dropable class="my-dropable" @onDrop="dropHandler">
      <draggable
        @startDrag="startDragHandler"
        @endDrag="endDragHandler"
        :payload="{ testKey: 'testVal' }"
        class="my-draggable"
        class-on-drag="is-dragging"
      />
    </dropable>
    <dropable class="my-dropable" @onDrop="dropHandler"> </dropable>
    <dropable class="my-dropable" @onDrop="dropHandler">
      <draggable
        @startDrag="startDragHandler"
        @endDrag="endDragHandler"
        :payload="{ testKey: 'testVal' }"
        class="my-draggable"
        class-on-drag="is-dragging"
      />
    </dropable>
    <dropable class="my-dropable" @onDrop="dropHandler"> </dropable>
  </div>
</template>

<script>
import draggable from "./components/Draggable.vue";
import dropable from "./components/Dropable.vue";

export default {
  name: "App",
  components: {
    draggable,
    dropable,
  },
  methods: {
    startDragHandler(payload) {
      console.log("Start drag payload:", payload);
    },
    endDragHandler(payload) {
      console.log("End drag payload:", payload);
    },
    dropHandler(payload) {
      console.log("Drop handler payload:", payload);
    },
  },
};
</script>

<style>
.my-dropable {
  width: calc(25vw - 10px);
  height: calc(50vh - 10px);
  display: flex;
  border: 1px solid red;
  flex-direction: row;
}

.my-draggable {
  margin: 10px;
  width: 100%;
  border: 1px solid brown;
}

.app {
  display: flex;
  flex-direction: row;
  flex-wrap: wrap;
}

.is-dragging {
  border-width: 10px;
}
</style>
