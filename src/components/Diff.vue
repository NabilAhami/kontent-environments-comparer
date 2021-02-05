<template>
  <h1>Diff</h1>
  <div v-html="diffResult" />
</template>

<script>
import { createTwoFilesPatch } from "diff";
import { html, parse } from "diff2html";
import "diff2html/bundles/css/diff2html.min.css";

export default {
  name: "Diff",
  props: {
    sourceTypes: Array,
    targetTypes: Array,
  },
  computed: {
    diffResult: function () {
      if (this.sourceTypes.length === 0 || this.targetTypes.length === 0) {
        return "N/A";
      }
      const json = [];

      this.sourceTypes.forEach((sourceType) => {
        const targetTypeCandidate =
          this.targetTypes.find(
            (target) => target.codename === sourceType.codename
          ) || {};
        var diffInput = createTwoFilesPatch(
          sourceType.codename,
          sourceType.codename, // maybe change as a filename
          JSON.stringify(sourceType, undefined, 2),
          JSON.stringify(targetTypeCandidate, undefined, 2)
        );
        json.push(...parse(diffInput));
      });

      const sourceTypesCodenames = this.sourceTypes.map(
        (sourceType) => sourceType.codename
      );
      const extraTargetTypes = this.targetTypes.filter(
        (targetType) => !sourceTypesCodenames.includes(targetType.codename)
      );
      extraTargetTypes.forEach((extraTargetType) => {
        var diffInput = createTwoFilesPatch(
          extraTargetType.codename,
          extraTargetType.codename, // maybe change as a filename
          JSON.stringify({}, undefined, 2),
          JSON.stringify(extraTargetType, undefined, 2)
        );
        json.push(...parse(diffInput));
      });

      const htmlDiffOutput = html(json, {
        drawFileList: true,
        matching: "lines",
        outputFormat: "side-by-side",
      });
      return htmlDiffOutput;
    },
  },
};
</script>

<style scoped>
</style>