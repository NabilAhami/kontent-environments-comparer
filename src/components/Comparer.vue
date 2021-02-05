<template>
  <div class="comparer">
    <div class="inputs">
      <div class="input-column">
        <input
          v-model="sourceEnvironmentDeliveryId"
          placeholder="Source environment ID"
          type="text"
        />
        <input
          v-model="sourceEnvironmentManagementId"
          placeholder="Source management ID"
          type="text"
        />
      </div>
      <button @click="compare">Compare</button>
      <div class="input-column">
        <input
          v-model="targetEnvironmentDeliveryId"
          placeholder="Target environment ID"
          type="text"
        />
        <input
          v-model="targetEnvironmentManagementId"
          placeholder="Target management ID"
          type="text"
        />
      </div>
    </div>
    <Diff :sourceTypes="sourceTypes" :targetTypes="targetTypes" />
  </div>
</template>

<script>
import Diff from "./Diff.vue";

export default {
  name: "Comparer",
  components: {
    Diff,
  },
  data() {
    return {
      sourceEnvironmentDeliveryId: "",
      sourceEnvironmentManagementId: "",
      targetEnvironmentDeliveryId: "",
      targetEnvironmentManagementId: "",
      sourceTypes: [],
      targetTypes: [],
    };
  },
  methods: {
    async compare() {
      const sourceTypesArray = await fetch(
        `https://manage.kontent.ai/v2/projects/${this.sourceEnvironmentDeliveryId}/types`,
        {
          method: "get",
          headers: {
            "content-type": "application/json",
            Authorization: `bearer ${this.sourceEnvironmentManagementId}`,
          },
        }
      )
        .then((result) => {
          console.log(result);
          return result.json();
        })
        .then((json) => {
          console.log(json);
          return json.types;
        });

      this.sourceTypes = sourceTypesArray;

      const targetTypesArray = await fetch(
        `https://manage.kontent.ai/v2/projects/${this.targetEnvironmentDeliveryId}/types`,
        {
          method: "get",
          headers: {
            "content-type": "application/json",
            Authorization: `bearer ${this.targetEnvironmentManagementId}`,
          },
        }
      )
        .then((result) => {
          console.log(result);
          return result.json();
        })
        .then((json) => {
          console.log(json);
          return json.types;
        });

      this.targetTypes = targetTypesArray;
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.inputs {
  display: flex;
  margin: 1em;
  padding: 1em;
  justify-content: center;
}

.inputs > * {
  margin: 1em;
}

.input-column {
  display: flex;
  flex-flow: column;
}
</style>
