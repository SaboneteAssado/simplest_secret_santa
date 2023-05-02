<template>
  <div class="mx-5 my-2">
    <div v-if="generated.length === 0">
      <div class="flex flex-col gap-4">
        <div>
          <div v-for="name of names" :key="JSON.stringify(names)">
            <div class="flex gap-2">
              <div class="cursor-pointer" @click="deleteName(name)">
                {{ name }}
              </div>
            </div>
          </div>
          <div class="mt-2">
            Add person name:
            <div class="flex gap-2">
              <input
                v-model="toAddName"
                class="border"
                placeholder="Person name"
              />
              <button class="cursor-pointer" @click="addName">Add name</button>
            </div>
          </div>
        </div>
        <div>
          <div v-for="rule of rules" :key="JSON.stringify(names)">
            <div class="cursor-pointer" @click="deleteRule(rule)">
              From {{ rule.from }} to {{ rule.to }}
            </div>
          </div>
          <div class="mt-2">
            Add rule:
            <div class="flex gap-2">
              <div>
                <input
                  v-model="ruleFrom"
                  class="border"
                  placeholder="From name"
                />
                <input v-model="ruleTo" class="border" placeholder="To name" />
              </div>
              <button class="cursor-pointer" @click="addRule">Add rule</button>
            </div>
          </div>
        </div>
      </div>
      <button class="cursor-pointer mt-4" @click="generate">Generate</button>
    </div>
    <div v-else>
      <div v-for="pair of generated">
        {{ pair.from }}: {{ stringToBase64(pair.to + " " + pair.seed) }}
      </div>
      <div class="mt-4">
        Use any online decoder like
        <a class="text-blue-500" href="https://www.base64decode.org/">This</a>
        . The format will be person + salt so the person giving the key cant
        recognize the name.
      </div>
      <div>
        <button class="cursor-pointer mt-4" @click="generated = []">
          Back
        </button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "IndexPage",
  data() {
    return {
      toAddName: "",
      ruleFrom: "",
      ruleTo: "",
      names: [],
      rules: [],
      generated: [],
    };
  },
  methods: {
    // adds a name
    addName() {
      this.names.push(this.toAddName);
      this.toAddName = "";
    },
    // adds a rule. atm a person can create rules with names that dont exist
    addRule() {
      if (!this.ruleTo || !this.ruleFrom) {
        alert("Incomplete Rule");
      } else {
        this.rules.push({
          from: this.ruleFrom,
          to: this.ruleTo,
        });
        this.ruleTo = "";
        this.ruleFrom = "";
      }
    },
    // choose random integer, used to pick a person
    getRandomInt(max) {
      return Math.floor(Math.random() * max);
    },
    // checks if rules dont happen and a person doesnt gift itself
    hasRuleOrRepeat(result) {
      for (const pair of result) {
        if (pair.from === pair.to) return true;
        for (const rule of this.rules)
          if (pair.from === rule.from && pair.to === rule.to) return true;
      }
      return false;
    },
    // generates the who gives whom and repeats if the rules are not respected
    generate() {
      this.generated = [];
      let result = this.generateAux(this.names);

      while (this.hasRuleOrRepeat(result))
        result = this.generateAux(this.names);

      this.generated = result;
    },
    // generates the actual giftings
    generateAux(defaultNames) {
      let generated = [];
      let poolNames = [...defaultNames];

      for (const currName of defaultNames) {
        const nameSelected = poolNames[this.getRandomInt(poolNames.length)];
        generated.push({
          from: currName,
          to: nameSelected,
          seed: Math.random(),
        });
        poolNames = poolNames.filter((name) => name !== nameSelected);
      }
      return generated;
    },
    stringToBase64(str) {
      return Buffer.from(str).toString("base64");
    },
    base64ToString(base64String) {
      return Buffer.from(base64String, "base64").toString();
    },
    deleteName(deleteName) {
      this.names = this.names.filter((name) => name !== deleteName);
    },
    deleteRule(deleteRule) {
      this.rules = this.rules.filter(
        (rule) => rule.to !== deleteRule.to && rule.from !== deleteRule.to
      );
    },
  },
};
</script>
