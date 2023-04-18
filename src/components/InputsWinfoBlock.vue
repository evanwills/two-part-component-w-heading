<script setup lang="ts">
import { ref, onBeforeMount, computed } from 'vue'
// import { Props } from './InputsWinfoBlock.d'

const props = withDefaults(defineProps<{
  id: string,
  heading: string,
  hLevel?: string,
  swap?: boolean,
  alternate?: boolean,
  left?: boolean,
  right?: boolean,
  alternateRev?: boolean,
  inputPercent?: string,
}>(), { hLevel: '2', inputPercent: '50' });

let _hLevel = 2;
let _colWidthInput = 1;

onBeforeMount(() => {
  const level = parseInt(props.hLevel);
  if (isNaN(level) || level < 1 || level > 6) {
    throw new Error('hLevel attribute must be unfdefined or a number between 1 and 6 inclusive')
  } else {
    _hLevel = level;
  }

  const percent = parseFloat(props.inputPercent);
  if (isNaN(percent) || percent < 10 || percent > 90) {
    throw new Error('inputPercent attribute must be unfdefined or a number between 10 and 90 inclusive. "' + props.inputPercent + '"')
  } else {
    _colWidthInput = Math.round(((percent * 2) / 100) * 1000) / 1000;
    console.log('props.inputPercent:', props.inputPercent)
    console.log('_colWidthInput:', _colWidthInput)
  }
});

const headID = computed(() : string => `${props.id}--head`);
const disclaimerID = computed(() => `${props.id}--disclaimer`);
const wrapClass = computed(() : string => {
  let output = 'inputs-w-info';

  if (props.left === true){
      output += ' inputs-w-info--left';
  } else if (props.right === true) {
      output += ' inputs-w-info--right';
  } else {
    output += (props.swap === true)
      ? ' inputs-w-info--swap'
      : '';
  }


  return output;
});
const getCSSvar = () : string => {
  const alt = Math.round((2 - _colWidthInput) * 1000) / 1000;
  return `--input-fr: ${_colWidthInput}fr; --info-fr: ${alt}fr;`;
}
</script>

<template>
  <section :class="wrapClass" :id="id" :style="getCSSvar()">
    <header>
      <h1 v-if="_hLevel === 1" class="inputs-w-info__h" :id="headID">{{ heading }}</h1>
      <h2 v-if="_hLevel === 2" class="inputs-w-info__h" :id="headID">{{ heading }}</h2>
      <h3 v-if="_hLevel === 3" class="inputs-w-info__h" :id="headID">{{ heading }}</h3>
      <h4 v-if="_hLevel === 4" class="inputs-w-info__h" :id="headID">{{ heading }}</h4>
      <h5 v-if="_hLevel === 5" class="inputs-w-info__h" :id="headID">{{ heading }}</h5>
      <h6 v-if="_hLevel === 6" class="inputs-w-info__h" :id="headID">{{ heading }}</h6>
    </header>

    <div class="inputs-w-info__child inputs-w-info__disclaim" :id="disclaimerID">
      <slot name="info"></slot>
    </div>

    <div class="inputs-w-info__child inputs-w-info__inputs">
      <slot name="inputs"></slot>
    </div>
  </section>
</template>

<style>
.read-the-docs {
  color: #888;
}
.inputs-w-info {
  box-sizing: border-box;
  column-gap: 2rem;
  display: flex;
  flex-direction: column;
  grid-template-columns: 50% 50%;
  grid-template-columns: var(--info-fr) var(--input-fr);
  grid-template-areas: "head head"
                       "disclaimer inputs";
  margin-bottom: 2rem;
  row-gap: 1rem;
  text-align: left;
  width: 100%;
}
.inputs-w-info > header {
  grid-area: head;
  position: relative;
  text-align: center;
}
.inputs-w-info > header::before {
  border-top: 0.1rem solid #fff;
  content: '';
  display: block;
  left: 0;
  position: absolute;
  right: 0;
  top: 55%;
  width: 100%;
}
.inputs-w-info__h {
  background-color: #242424;
  display: inline-block;
  margin: 0;
  padding: 0.5rem 1rem;
  position: relative;
  z-index: 1;
}
.inputs-w-info__child {
  border: 0.05rem solid #fff;
  padding: 1.5rem;
}
.inputs-w-info__disclaim {
  grid-area: disclaimer;
}
.inputs-w-info__child :first-child {
  margin-top: 0;
}
.inputs-w-info__inputs {
  grid-area: inputs;
}

@media screen and (max-width: 47.999999rem) {
  .inputs-w-info {
    display: flex;
    flex-direction: column;
  }
}

@media screen and (min-width: 48rem) {
  .inputs-w-info {
    display: grid;
    grid-template-areas: "head head"
                        "disclaimer inputs";
    grid-template-columns: 50% 50%;
    grid-template-columns: var(--info-fr) var(--input-fr);
  }

  .inputs-w-info--swap,
  .inputs-w-info--alternate > .inputs-w-info:nth-of-type(even),
  .inputs-w-info--alternate > .inputs-w-info--swap:nth-of-type(odd),
  .inputs-w-info--alternate--rev > .inputs-w-info:nth-of-type(odd),
  .inputs-w-info--alternate--rev > .inputs-w-info--swap:nth-of-type(even) {
    grid-template-areas: "head head"
                        "inputs disclaimer";
    grid-template-columns: 50% 50%;
    grid-template-columns: var(--input-fr) var(--info-fr);
  }
  .inputs-w-info--alternate > .inputs-w-info:nth-of-type(odd),
  .inputs-w-info--alternate >  .inputs-w-info--swap:nth-of-type(even),
  .inputs-w-info--alternate--rev > .inputs-w-info:nth-of-type(even),
  .inputs-w-info--alternate--rev > .inputs-w-info--swap:nth-of-type(odd) {
    grid-template-areas: "head head"
                        "disclaimer inputs";
  }
  .inputs-w-info--left {
    grid-template-areas: "head head"
                         "inputs disclaimer" !important;
  }
  .inputs-w-info--right {
    grid-template-areas: "head head"
                         "disclaimer inputs" !important;
  }
}

/* @container (min-width: 40rem) {
  .inputs-w-info {
    display: grid;
    grid-template-areas: "head head"
                        "disclaimer inputs";
  }
  .inputs-w-info--alternate:nth-of-type(odd),
  .inputs-w-info--alternate.inputs-w-info--swap:nth-of-type(even) {
    grid-template-areas: "head head"
                        "disclaimer inputs";
  }

  .inputs-w-info--swap,
  .inputs-w-info--alternate:nth-of-type(even),
  .inputs-w-info--alternate.inputs-w-info--swap:nth-of-type(odd) {
    grid-template-areas: "head head"
                        "inputs disclaimer";
    grid-template-columns: var(--input-fr) auto;
  }
} */
</style>
