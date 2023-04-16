<script setup lang="ts">
import { ref, onBeforeMount, computed, getCurrentInstance } from 'vue'
import { Props } from './InputsWDisclaimerBlock.d'

const props = withDefaults(defineProps<{
  id: string,
  heading: string,
  hLevel?: string,
  swap?: boolean,
  alternate?: boolean,
  inputPercent?: string,
}>(), { hLevel: '2', inputPercent: '50' });

let _hLevel = 2;
let _colWidthInput = 50;

onBeforeMount(() => {
  const level = parseInt(props.hLevel);
  if (isNaN(level) || level < 1 || level > 6) {
    throw new Error('hLevel attribute must be unfdefined or a number between 1 and 6 inclusive')
  } else {
    _hLevel = level;
  }

  const percent = parseFloat(props.inputPercent);
  if (isNaN(percent) || percent < 10 || percent > 90) {
    throw new Error('inputPercent attribute must be unfdefined or a number between 10 and 90 inclusive')
  } else {
    _colWidthInput = percent;
  }
});

const headID = computed(() : string => `${props.id}--head`);
const disclaimerID = computed(() => `${props.id}--disclaimer`);
const wrapClass = computed(() : string => {
  let output = 'inp-w-disclaim';

  output += (props.alternate === true)
    ? ' inp-w-disclaim--alternate'
    : '';

    output += (props.swap === true)
    ? ' inp-w-disclaim--swap'
    : '';

  return output;
});
const getCSSvar = () : string => `--input-percent: ${_colWidthInput}%;`;
</script>

<template>
  <section :class="wrapClass" :id="id" :style="getCSSvar()">
    <header>
      <h1 v-if="_hLevel === 1" class="inp-w-disclaim__h" :id="headID">{{ heading }}</h1>
      <h2 v-if="_hLevel === 2" class="inp-w-disclaim__h" :id="headID">{{ heading }}</h2>
      <h3 v-if="_hLevel === 3" class="inp-w-disclaim__h" :id="headID">{{ heading }}</h3>
      <h4 v-if="_hLevel === 4" class="inp-w-disclaim__h" :id="headID">{{ heading }}</h4>
      <h5 v-if="_hLevel === 5" class="inp-w-disclaim__h" :id="headID">{{ heading }}</h5>
      <h6 v-if="_hLevel === 6" class="inp-w-disclaim__h" :id="headID">{{ heading }}</h6>
    </header>

    <div class="inp-w-disclaim__child inp-w-disclaim__disclaim" :id="disclaimerID">
      <slot name="disclaimer"></slot>
    </div>

    <div class="inp-w-disclaim__child inp-w-disclaim__inputs">
      <slot name="ïnputs"></slot>
    </div>
  </section>
</template>

<style>
.read-the-docs {
  color: #888;
}
.inp-w-disclaim {
  box-sizing: border-box;
  column-gap: 2rem;
  display: flex;
  flex-direction: column;
  grid-template-columns: 50% 50%;
  grid-template-columns: auto var(--input-percent);
  grid-template-areas: "head head"
                       "disclaimer inputs";
  margin-bottom: 2rem;
  row-gap: 1rem;
  text-align: left;
}
.inp-w-disclaim > header {
  grid-area: head;
  position: relative;
  text-align: center;
}
.inp-w-disclaim > header::before {
  border-top: 0.1rem solid #fff;
  content: '';
  display: block;
  left: 0;
  position: absolute;
  right: 0;
  top: 55%;
  width: 100%;
}
.inp-w-disclaim__h {
  background-color: #242424;
  display: inline-block;
  margin: 0;
  padding: 0.5rem 1rem;
  position: relative;
  z-index: 1;
}
.inp-w-disclaim__child {
  border: 0.05rem solid #fff;
  padding: 1.5rem;
}
.inp-w-disclaim__disclaim {
  grid-area: disclaimer;
}
.inp-w-disclaim__child :first-child {
  margin-top: 0;
}
.inp-w-disclaim__inputs {
  grid-area: inputs;
}

@media screen and (max-width: 47.999999rem) {
  .inp-w-disclaim {
    display: flex;
    flex-direction: column;
  }
}

@media screen and (min-width: 48rem) {
  .inp-w-disclaim {
    display: grid;
    grid-template-areas: "head head"
                        "disclaimer inputs";
  }
  .inp-w-disclaim--alternate:nth-of-type(odd),
  .inp-w-disclaim--alternate.inp-w-disclaim--swap:nth-of-type(even) {
    grid-template-areas: "head head"
                        "disclaimer inputs";
  }

  .inp-w-disclaim--swap,
  .inp-w-disclaim--alternate:nth-of-type(even),
  .inp-w-disclaim--alternate.inp-w-disclaim--swap:nth-of-type(odd) {
    grid-template-areas: "head head"
                        "inputs disclaimer";
    grid-template-columns: 50% 50%;
    grid-template-columns: var(--input-percent) auto;
  }
}

/* @container (min-width: 40rem) {
  .inp-w-disclaim {
    display: grid;
    grid-template-areas: "head head"
                        "disclaimer inputs";
  }
  .inp-w-disclaim--alternate:nth-of-type(odd),
  .inp-w-disclaim--alternate.inp-w-disclaim--swap:nth-of-type(even) {
    grid-template-areas: "head head"
                        "disclaimer inputs";
  }

  .inp-w-disclaim--swap,
  .inp-w-disclaim--alternate:nth-of-type(even),
  .inp-w-disclaim--alternate.inp-w-disclaim--swap:nth-of-type(odd) {
    grid-template-areas: "head head"
                        "inputs disclaimer";
    grid-template-columns: var(--input-percent) auto;
  }
} */
</style>
