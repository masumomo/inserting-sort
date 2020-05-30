<template>
  <v-container>
    <v-row class="text-center">
      <v-col class="mb-4">
        <v-btn class="mx-2 center" v-on:click="sort">Sorting</v-btn>
        <v-btn class="mx-2 center" v-on:click="play">Next step</v-btn>
        <v-btn class="mx-2 center" v-on:click="initialize"
          >Initialize data</v-btn
        >
      </v-col>
    </v-row>
    <v-row class="text-center">
      <v-col class="mb-4">
        <h1 class="display-2 ml9 font-weight-bold mb-3">
          <span class="text-wrapper">
            <span class="letters">Sorting algolism visualization</span>
          </span>
        </h1>
      </v-col>
      <v-col class="mb-5" cols="12">
        <v-row justify="center" class="subheading mx-3 sorting-container">
          <v-card
            class="mx-2 sort-item"
            depressed
            v-for="item in sortingArrays[currentStep]"
            :key="item.id"
            :id="item.id"
            width="50px"
            height="50px"
          >
            {{ item.number }}</v-card
          >
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import { shuffle } from "underscore";
import Shuffle from "shufflejs";

const arrayLength = 7;

export default {
  name: "InsertingSort",
  mounted: function() {
    this.initialize();
  },
  methods: {
    initialize() {
      this.sortingArrays = [];
      this.currentStep = 0;
      const initialArray = [];
      for (let i = 0; i < arrayLength; i++) {
        initialArray.push({
          id: i,
          idx: i,
          number: Math.floor(Math.random() * 20),
        });
      }
      this.sortingArrays.push(initialArray);
      const element = document.querySelector(".sorting-container");
      this.shuffleInstance = new Shuffle(element, {
        itemSelector: ".sort-item",
      });
      this.sort();
    },
    play() {
      this.currentStep++;
      const options = {
        by: (element) => {
          const itemIdx = parseInt(element.getAttribute("id"));
          return this.sortingArrays[this.currentStep][itemIdx].idx;
        },
      };
      this.shuffleInstance.sort(options);
    },
    sort() {
      //TODO actual sorting
      for (let i = 0; i < this.sortingArrays[0].length; i++) {
        console.log("nextArray 1 :>> ", this.sortingArrays[i][0].number);
        console.log("nextArray 2 :>> ", this.sortingArrays[i][1].number);
        const nextArray = shuffle(this.sortingArrays[i]);
        for (let j = 0; j < arrayLength; j++) {
          nextArray.idx = j;
        }
        this.sortingArrays.push(nextArray);
      }
    },
  },
  data: () => ({
    shuffleInstance: null,
    currentStep: 0,
    sortingArrays: [],
    from: 2,
    to: 1,
  }),
};
</script>
<style scoped></style>
