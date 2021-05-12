<template>
  <td class="cell" @click="strike">{{ mark }}</td>
</template>

<script>
export default {
  // property accessible from parent (Grid)
  props: ["name"],
  data() {
    return {
      // We enable players to make their moves
      frozen: false,
      // Let's "save" the move made
      mark: "",
    };
  },
  methods: {
    strike() {
      if (!this.frozen) {
        // We get more X's or O's from Grid
        this.mark = this.$parent.activePlayer;
        this.frozen = true;

        // fires an event to notify the Grid component that a mark is placed
        Event.$emit("strike", this.name);
      }
    },
  },
  created() {
    Event.$on("clearCell", () => {
      this.mark = "";
      this.frozen = false;
    });
    Event.$on("freeze", () => (this.frozen = true));
  },
};
</script>

<style>
.cell {
  width: 33.333%;
  height: 90px;
  border: 1px solid #ffffff;
  font-size: 3.5em;
  font-family: "Gochi Hand", sans-serif;
  -webkit-box-shadow: 1px 0px 8px 3px rgba(255, 255, 255, 0.78);
  box-shadow: 1px 0px 8px 3px rgba(255, 255, 255, 0.78);
  text-shadow: 0px 0px 15px rgba(255, 255, 255, 0.75);
}
.cell:hover {
  background: radial-gradient(
    circle,
    rgba(18, 72, 252, 0.5032387955182073) 0%,
    rgba(191, 3, 255, 0.4724264705882353) 100%
  );
}
.cell::after {
  content: "";
  display: block;
}
.cell:first-of-type {
  border-left-color: transparent;
  border-top-color: transparent;
}
.cell:nth-of-type(2) {
  border-top-color: transparent;
}
.cell:nth-of-type(3) {
  border-right-color: transparent;
  border-top-color: transparent;
}
tr:nth-of-type(3) .cell {
  border-bottom-color: transparent;
}
</style>
