<template>
  <div class="my-2">
    <button class="button is-danger" @click="generateJSONExport()">
      Export Annotations
    </button>
  </div>
</template>
<script>
import { mapState } from "vuex";
export default {
  name: "Export",
  computed: {
    ...mapState(["annotations", "classes"]),
  },
  methods: {
    generateJSONExport() {
      const filename = "training_data.csv";
      // const output = {
      //   // "classes": this.classes.map(c => c.name),
      //   annotations: this.annotations,
      // };
      let result = [];
      this.annotations.forEach((a) => {
        if (a.length > 0) {
          if (a[0].length > 0) {
            if (a[0][0].length > 0) {
              // console.log(a[0])
              result.push(a[0]);
            }
          }
        }
      });

      let output1 = [];
      result.forEach((b) => {
        b.forEach((c) => {
          let obj = {};
          obj["Line"] = c[0];
          obj["Begin Offset"] = c[1];
          obj["End Offset"] = c[2];
          obj["Type"] = c[3];
          output1.push(obj);
        });
      });
      console.log(this.$hostname);
      // const jsonStr = JSON.stringify(output1);
      var JsonFields = ["Line", "Begin Offset", "End Offset", "Type"];

      var csvStr = JsonFields.join(",") + "\n";
      output1.forEach((element) => {
        var Line = element.Line;
        var BeginOffset = element["Begin Offset"];
        var EndOffset = element["End Offset"];
        var Type = element.Type;

        csvStr +=
          Line + "," + BeginOffset + "," + EndOffset + "," + Type + "\n";
      });

      let element = document.createElement("a");
      element.setAttribute(
        "href",
        "data:text/csv;charset=utf-8," + encodeURIComponent(csvStr)
      );
      element.setAttribute("download", filename);

      element.style.display = "none";
      document.body.appendChild(element);
      element.click();
      document.body.removeChild(element);
    },
  },
};
</script>
