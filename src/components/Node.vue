<template>
  <div
    class="node"
    :class="extraClasses"
    @mousedown="onMouseDown(row, col)"
    @mouseenter="onMouseEnter(row, col)"
    @mouseup="onMouseUp"
  ></div>
</template>

<script>
export default {
  name: 'Node',

  props: {
    row: {
      type: Number,
      required: true
    },
    col: {
      type: Number,
      required: true
    },
    isStart: {
      type: Boolean,
      required: true
    },
    isFinish: {
      type: Boolean,
      required: true
    },
    isWall: {
      type: Boolean,
      required: true
    },
    onMouseDown: {
      type: Function,
      required: true
    },
    onMouseEnter: {
      type: Function,
      required: true
    },
    onMouseUp: {
      type: Function,
      required: true
    },
  },

  computed: {
    extraClasses () {
      return {
        'node-start': this.isStart,
        'node-finish': this.isFinish,
        'node-wall': this.isWall
      }
    }
  }
}
</script>

<style scoped>
.node {
  width: 25px;
  height: 25px;
  outline: 1px solid rgb(175, 216, 248);
  display: inline-block;
}

.node-finish {
  background-color: red;
}

.node-start {
  background-color: green;
}

.node-wall {
  background-color: rgb(12, 53, 71);
}

</style>