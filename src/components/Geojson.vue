<template>
  <div id="geojson">
    <div id="full"></div>
    <div id="slice"></div>
  </div>
</template>

<script>
import * as d3 from "d3";
// import * as turf from "@turf/turf";
export default {
  name: "Geojson",
  mounted() {
    this.generateFull();
    // this.generateSlice();
  },
  methods: {
    async generateSlice() {
      const geojson = await d3.json("/taiwan.json");
      // const origin = JSON.parse(JSON.stringify(geojson));
      // geojson.features[0].properties = { name: "南海诸岛" };
      // const [left, top, right, bottom] = [
      //   106,
      //   23.5, // 北回归线
      //   122,
      //   3,
      // ];
      // let coordinates = [];
      // geojson.features[0].geometry.coordinates.forEach((item, index) => {
      //   let line = item[0];
      //   // 最长国界线特殊处理， 顺时针画的
      //   if (index === 0) {
      //     let startIndex = -1;
      //     let endIndex = -1;
      //     line.forEach((lnglat, index) => {
      //       const [lng, lat] = lnglat;
      //       if (lng >= left && lng <= right && lat >= bottom && lat <= top) {
      //         if (startIndex === -1) {
      //           startIndex = index;
      //         }
      //         endIndex = index;
      //       }
      //     });
      //     const lines = line.slice(startIndex, endIndex + 1);
      //     // 由于维度是弧形, 且相距很远 需要加上一点 top 偏移
      //     lines.unshift([lines[0][0], top + 0.15]);
      //     lines.push([left, top], [lines[0][0], top + 0.15]);
      //     coordinates.push([lines]);
      //   } else if (index === 6) {
      //     console.log(1);
      //   }
      //   // 台湾特殊处理 (逆时针画的)
      //   else if (index === 7) {
      //     let startIndex = -1;
      //     let endIndex = -1;
      //     line.forEach((lnglat, index) => {
      //       const [lng, lat] = lnglat;
      //       if (!(lng >= left && lng <= right && lat >= bottom && lat <= top)) {
      //         if (startIndex === -1) {
      //           startIndex = index;
      //         }
      //         endIndex = index;
      //       }
      //     });
      //     const lineStart = line.slice(0, startIndex + 1);
      //     const lineEnd = line.slice(endIndex + 1);
      //     // 相距近，不需要加偏
      //     lineStart.push([lineStart[lineStart.length - 1][0], top]);
      //     lineEnd.unshift([lineEnd[0][0], top]);
      //     coordinates.push([lineStart.concat(lineEnd)]);
      //   } else {
      //     const lines = line.filter((lnglat) => {
      //       const [lng, lat] = lnglat;
      //       return lng >= left && lng <= right && lat >= bottom && lat <= top;
      //     });
      //     lines.length && coordinates.push([lines]);
      //   }
      // });
      // coordinates.push([
      //   [
      //     [left, top],
      //     [left, bottom],
      //     [right, bottom],
      //     [right, top],
      //     [left, top],
      //   ],
      // ]);
      // const scale = 0.5;
      // const [translateLng, translateLat] = [19.5, 14];
      // coordinates = coordinates.map((item) => {
      //   const [line] = item;
      //   return [
      //     line.map((lnglat) => {
      //       const [lng, lat] = lnglat;
      //       return [
      //         (lng - left) * scale + left + translateLng,
      //         (lat - bottom) * scale + bottom + translateLat,
      //       ];
      //     }),
      //   ];
      // });

      // geojson.features[0].geometry.coordinates = coordinates;
      const svg = d3
        .select("#slice")
        .append("svg")
        .style("width", "100%")
        .style("height", "100%");
      const width = document.getElementById("slice").offsetWidth;
      const height = document.getElementById("slice").offsetHeight;
      let projection = d3
        .geoMercator()
        .center([104, 31])
        .scale(height * 0.9)
        .translate([width / 2, height / 2]);
      let path = d3.geoPath().projection(projection);

      svg
        .selectAll("path")
        .data(geojson.features.concat(origin.features))
        .enter()
        .append("path")
        .attr("stroke", "#000")
        .attr("stroke-width", 1)
        .attr("fill", "none")
        .attr("d", path);
    },
    async generateFull() {
      const geojson = await d3.json("/china.json");
      const svg = d3
        .select("#full")
        .append("svg")
        .style("width", "100%")
        .style("height", "100%");
      const width = document.getElementById("full").offsetWidth;
      const height = document.getElementById("full").offsetHeight;
      let projection = d3
        .geoMercator()
        .center([104, 31])
        .scale(height * 0.9)
        .translate([width / 2, height / 2]);
      let path = d3.geoPath().projection(projection);

      svg
        .selectAll("path")
        .data(geojson.features)
        .enter()
        .append("path")
        .attr("stroke", "#000")
        .attr("stroke-width", 1)
        .attr("fill", "none")
        .attr("d", path);
    },
  },
};
</script>
<style lang="less" scoped>
#geojson {
  height: 100%;
  display: flex;
  #full {
    width: 100%;
  }
  // #slice {
  //   width: 50%;
  // }
}
</style>
