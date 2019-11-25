<template>
  <div class="pathfinder-wrapper">
    <button @click="visualizeDjikstra">Djikstra</button>
    <template v-for="(row, rowIndex) in grid">
      <div :key="rowIndex">
        <template v-for="({
          row,
          col,
          isStart,
          isFinish,
          isWall,
          mouseIsPressed
        }, nodeIndex) in row">
          <Node
            :key="nodeIndex"
            :row="row"
            :col="col"
            :is-start="isStart"
            :is-finish="isFinish"
            :is-wall="isWall"
            :on-mouse-down="handleOnMouseDown"
            :on-mouse-enter="handleOnMouseEnter"
            :on-mouse-up="handleOnMouseUp"
          />
        </template>
      </div>
    </template>
  </div>
</template>

<script>
import { djikstra, getNodesInShortestPathOrder } from '../utils'
import Node from './Node'

const TOTAL_ROWS = 25
const TOTAL_COLS = 50
const START_NODE_ROW = 10
const START_NODE_COL = 15
const FINISH_NODE_ROW = 10
const FINISH_NODE_COL = 35

export default {
  name: 'PathFinder',

  components: {
    Node
  },

  data: () => ({
    grid: []
  }),

  mounted() {
    // generate grid
    this.grid = this.getGrid()
  },

  methods: {
    getGrid() {
      const grid = []
      for (let row = 0; row < TOTAL_ROWS; row++) {
        const currentRow = []
        for (let col = 0; col < TOTAL_COLS; col++) {
          currentRow.push(this.getNode(row, col))
        }
        grid.push(currentRow)
      }
      return grid
    },
    getNode(row, col) {
      return {
        row,
        col,
        previousNode: null,
        distance: Infinity,
        isStart: row === START_NODE_ROW && col === START_NODE_COL,
        isFinish: row === FINISH_NODE_ROW && col === FINISH_NODE_COL,
        isVisited: false,
        isWall: false
      }
    },
    getNewGridWithWallToggled(grid, row, col) {

      const newGrid = grid.slice()
      // get clicked on node
      const node = newGrid[row][col]
      const newNode = {
        ...node,
        isWall: !node.isWall
      }
      
      // update clicked on node
      newGrid[row][col] = newNode

      return newGrid
    },
    handleOnMouseDown(row, col) {
      // get new grid will walls
      this.grid = this.getNewGridWithWallToggled(this.grid, row, col)
      this.mouseIsPressed = true
    },
    handleOnMouseEnter(row, col) {
      if (!this.mouseIsPressed) {
        return
      }
      this.grid = this.getNewGridWithWallToggled(this.grid, row, col)
    },
    handleOnMouseUp() {
      this.mouseIsPressed = false
    },
    animateShortestPath(nodesInShortestPathOrder) {
      for (let i = 0; i < nodesInShortestPathOrder.length; i++) {
        setTimeout(() => {
          const node = nodesInShortestPathOrder[i]
          document.getElementById(`node-${node.row}-${node.col}`).className = 'node node-shortest-path'
        }, 50 * i)
      }
    },
    animateDjikstra(visitedNodesInOrder, nodesInShortestPathOrder) {
      for (let i = 0; i <= visitedNodesInOrder.length; i++) {
        if (i === visitedNodesInOrder.length) {
          setTimeout(() => {
            this.animateShortestPath(nodesInShortestPathOrder)
          }, 10 * i)
          return
        }
        setTimeout(() => {
          const node = visitedNodesInOrder[i]
          document.getElementById(`node-${node.row}-${node.col}`).className = 'node node-visited'
        }, 10 * i)
      }
    },
    visualizeDjikstra() {
      const startNode = this.grid[START_NODE_ROW][START_NODE_COL]
      const finishNode = this.grid[FINISH_NODE_ROW][FINISH_NODE_COL]
      const visitedNodesInOrder = djikstra(this.grid, startNode, finishNode)
      const nodesInShortestPathOrder = getNodesInShortestPathOrder(finishNode)
      this.animateDjikstra(visitedNodesInOrder, nodesInShortestPathOrder)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="scss" scoped>
.pathfinder-wrapper {}
</style>
