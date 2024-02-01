<template>
  <Experiment title="Naturalness rating">
    <Screen :title="'Welcome'">
      Thank you for participating in our study. Before starting the experiment, please read the legal information on the next page carefully.
      <button @click="$magpie.nextScreen()">Show me the legal information</button>
    </Screen>

    <Screen>
      In this task, you will be asked to read short statements presented in the absence of context and to rate how natural these statements sound. The whole experiment should take no more than 5-10 minutes.
      <br />
      <br />
      The only condition for taking part is that you are <b>over 18</b>. There are no known risks to taking part and your data will be entirely anonymous. You are free to withdraw at any point before you complete the survey by closing your browser window. Since it is anonymous, it will not be possible to withdraw your data after you finish the survey. Data may be used for teaching and research purposes and will be stored securely by the research team at the University of Birmingham.
      <br />
      <br />
      By clicking the accept button below, you indicate that:
      <br />
      <span style="margin-left:2em">You are over 18.</span>
      <br />
      <span style="margin-left:2em">You voluntarily agree to participate and understand that you can stop your participation at any time by closing your browser window before the study is completed.</span>
      <br />
      <span style="margin-left:2em">You agree that your <b>anonymous</b> data may be stored securely in University of Birmingham archives and may be used by qualified researchers for teaching and research purposes.</span>
      <br />
      <br />
      If you do not agree to all of these points, please close your browser window now.
      <br />
      <br />
      <button @click="$magpie.nextScreen()">Begin the experiment</button>
    </Screen>

    <Screen>
      In order to complete the task, each trial you will be asked to read a statement and to rate how natural you think it sounds. All you have to do is to move the slider closer to either end of the scale.
      <button @click="$magpie.nextScreen()">Proceed to task</button>
    </Screen>

    <!-- We iterate over our experiment trials -->
    <template v-for="(trial, i) in items_sorted">
      <Screen>
        <Slide>
          <p>
            <strong>{{ trial.qud }}</strong>
          </p>
        <SliderScreen
            :key="i"
            :progress="i / items_sorted.length"
            question="How natural does this sentence sound to you?"
            option-left="Very unnatural"
            option-right="Very natural"
            initial=50
            />
        <Record :data="{
           'qud': trial.qud,
           'cue': trial.cue,
           'item_id': trial.item_id,
           'trial_nr': i + 1}"
           />
        </Slide>
      </Screen>
    </template>

    <Screen>
      Thank you for participating in our study!
      <button @click="$magpie.nextScreen()">Next</button>
    </Screen>

    <PostTestScreen />

    <SubmitResultsScreen />
  </Experiment>
</template>

<script>
import _ from 'lodash';
import items from '../trials/materials-v2.csv';


// let items_sorted = []
// for (var i = 0; i < 15; i += 3) {
//   items_sorted.push(items[i + _.sample([0, 1, 2])])
// }
let critical = _.slice(items, 0, 15)

let fillers = _.slice(_.shuffle(_.slice(items, 16, 55)), 0, 30)
// for (var i = 46; i < 85; i++) {
//     fillers.push(items[i])
// }

let items_sorted = []
items_sorted = _.shuffle(_.concat(critical, fillers));
// items_sorted = _.shuffle(items_sorted);
console.log(items_sorted);

export default {
  name: 'App',
  data() {
    return {
      items_sorted: items_sorted
    };
  },
  computed: {
    // Expose lodash to template code
    _() {
      return _;
    }
  }
};

</script>