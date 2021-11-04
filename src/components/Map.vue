<template>
  <div class="map">
    <h3>Карта офиса</h3>
    <div v-if="!isLoading" class="map-root" @click="openProfile">
      <MapSVG ref="svg" />
      <TableSVG v-show="false" ref="table" />
    </div>
    <div v-else>Loading...</div>
  </div>
</template>

<script>
import MapSVG from "@/assets/images/map.svg";
import TableSVG from "@/assets/images/workPlace.svg";
import * as d3 from "d3";
import tables from "@/assets/data/tables.json";
import legend from "@/assets/data/legend.json";
import people from "@/assets/data/people.json";

export default {
  components: {
    MapSVG,
    TableSVG,
  },
  data() {
    return {
      isLoading: false,
      svg: null,
      g: null,
      tables: [],
      tableSVG: null,
    };
  },
  mounted() {
    this.svg = d3.select(this.$refs.svg);
    this.g = this.svg.select("g");
    this.tables = tables;
    this.tableSVG = d3.select(this.$refs.table);

    if (this.g) {
      this.drawTables();
    } else {
      console.log("drawTables ERROR");
    }
  },
  methods: {
    drawTables() {
      const svgTablesGroup = this.g.append("g").classed("groupPlaces", true);

      this.tables.map((table) => {
        const svgTable = svgTablesGroup
          .append("g")
          .attr("transform", `translate(${table.x}, ${table.y}) scale(0.5)`)
          .attr("id", table._id)
          .classed("employee-place", true);
        svgTable
          .append("g")
          .attr("transform", `rotate(${table.rotate || 0})`)
          .html(this.tableSVG.html())
          .attr(
            "fill",
            legend.find((it) => {
              return it.group_id === table.group_id;
            })?.color ?? "transparent"
          );
      });
    },
    openProfile(ev) {
      if (ev.target.parentElement.classList.contains("table")) {
        // id of current employee
        const tableId = Number(ev.target.closest(".employee-place").id);
        const person = people.find((person) => person.tableId === tableId);
        this.$emit("update:person", person);
        this.$emit("update:isUserOpened", true);
      } else {
        this.$emit("update:isUserOpened", false);
      }
    },
  },
};
</script>

<style scoped>
.map {
  height: 100%;
  width: 100%;
  padding: 24px;
  overflow: hidden;
  box-sizing: border-box;
  display: flex;
  flex-direction: column;
}

.map-root {
  height: 100%;
  width: 100%;
  overflow: hidden;
  box-sizing: border-box;
}

h3 {
  margin-top: 0px;
}

::v-deep svg {
  height: 100%;
  width: 100%;
}

::v-deep .table {
  cursor: pointer;
}
</style>
