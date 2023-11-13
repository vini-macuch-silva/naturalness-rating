<template>
  <Experiment title="Naturalness rating">
    <Screen :title="'Welcome'">
      Thank you for participating in our study. Before starting the experiment, please read the legal information on the next page carefully.
      <button @click="$magpie.nextScreen()">Show me the legal information</button>
    </Screen>

    <Screen>
      In this task, you will be asked to read short metaphoric statements presented in the absence of context and to rate how natural these statements sound. The whole experiment should take up to five minutes.
      <br />
      <br />
      The only condition on taking part is that you are <b>over 18</b>. Please follow this <a href="http://blake.ppls.ed.ac.uk/~pling/consent-vinicius.html">link</a> to read the consent form about participating in the experiment.
      <br />
      <br />
      By clicking the accept button below, you indicate that:
      <br />
      <span style="margin-left:2em">You are over 18.</span>
      <br />
      <span style="margin-left:2em">You have read the consent form.</span>
      <br />
      <span style="margin-left:2em">You voluntarily agree to participate and understand that you can stop your participation at any time.</span>
      <br />
      <span style="margin-left:2em">You agree that your <b>anonymous</b> data may be kept permanently in University of Birmingham archives and may be used by qualified researchers for teaching and research purposes.</span>
      <br />
      <br />
      If you do not agree to all of these points, please close your browser window now.
      <br />
      <br />
      <button @click="$magpie.nextScreen()">Begin the experiment</button>
    </Screen>

    <Screen>
      In order to complete the task, each trial you will be asked to read a metaphoric statement and to rate how natural you think it sounds. All you have to do is to move the slider closer to either end of the scale.
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
import items from '../trials/materials.csv';


let items_sorted = []
for (var i = 0; i < 25; i += 3) {
  items_sorted.push(items[i + _.sample([0, 1, 2])])
}

items_sorted = _.shuffle(items_sorted);
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