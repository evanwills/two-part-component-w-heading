<template>
    <div class="inputs-block__info" ref="contentInfo">
      <h3>This is a heading</h3>
      <p>This is some blurb about the content</p>
    </div>
    <div class="inputs-block__content-wrap" :style="getWrapStyle()">
      <div :class="getWrapClass()">
        <div class="inputs-block__values inputs-block__values--values" ref="inputValues">
          <ul>
            <li>
              <span class="inputs-block__label">First name:</span>
              <span class="inputs-block__input">John</span>
            </li>
            <li>
              <span class="inputs-block__label">Last name:</span>
              <span class="inputs-block__input">Doe</span>
            </li>
          </ul>

          <p class="inputs-block__btns">
            <button v-on:click="toggleInputsOpen"
                    :tabindex="(inputsOpen) ? -1 : 0">Edit</button>
          </p>
        </div>
        <div class="inputs-block__values inputs-block__values--inputs" ref="inputInputs">
          <ul
              ref="contentInputs">
            <li>
              <label for="first-name" class="inputs-block__label">First name:</label>
              <input type="text"
                     id="first-name"
                     name="first-name"
                     class="inputs-block__input"
                     value="John"
                    :tabindex="(inputsOpen) ? 0 : -1 " />
            </li>
            <li>
              <label for="last-name" class="inputs-block__label">Last name:</label>
              <input type="text"
                     id="last-name"
                     name="last-name"
                     class="inputs-block__input"
                     value="Doe"
                    :tabindex="(inputsOpen) ? 0 : -1 " />
            </li>
          </ul>

          <p class="inputs-block__btns">
            <button v-on:click="saveClick" :tabindex="(inputsOpen) ? 0 : -1">Save</button>
            <button v-on:click="cancelClick" :tabindex="(inputsOpen) ? 0 : -1">Cancel</button>
          </p>
        </div>
      </div>
    </div>
</template>

<script setup lang="ts">
import { ref, onMounted, Ref, getCurrentInstance, defineComponent } from 'vue'


let inputsOpen : boolean = false;
let contentWidth : string = 'auto';
let maxHeight : string = 'auto';
const inputValues : Ref<null> = ref(null);
const inputInputs : Ref<null> = ref(null);
const contentInfo : Ref<null> = ref(null);
const instance = getCurrentInstance();
// let resizeWatcher : ResizeObserver|null = null

const toggleInputsOpen = () : void => {
  console.group('toggleInputsOpen()');
  console.log('inputsOpen (before):', inputsOpen);

  inputsOpen = true;
  instance?.proxy?.$forceUpdate();

  console.log('inputsOpen (after):', inputsOpen);
  console.groupEnd();
  // toggleInputsOpenInner();
};

const saveClick = () : void => {
  console.group('toggleInputsOpen()');
  console.log('inputsOpen (before):', inputsOpen);

  inputsOpen = false;
  instance?.proxy?.$forceUpdate();


  console.log('inputsOpen (after):', inputsOpen);
  console.groupEnd();

  // toggleInputsOpenInner();
};

const getWrapClass = () : string => {
  console.log()
  const tmp = 'inputs-block__content';
  let output = `${tmp} ${tmp}--`;

  output += (inputsOpen)
    ? 'editing'
    : 'viewing';

  if (contentWidth !== 'auto') {
    output += ` ${tmp}--slide`
  }

  return output;
};

const getWrapStyle = () : string => {
  console.group('getWrapStyle():')
  console.log('contentWidth:', contentWidth);
  console.log('maxHeight:', maxHeight);
  console.groupEnd();
  return `--values-width: ${contentWidth}; --values-height: ${maxHeight}`;
};

const cancelClick = () : void => {
  console.group('toggleInputsOpen()');
  console.log('inputsOpen (before):', inputsOpen);

  inputsOpen = false;
  instance?.proxy?.$forceUpdate();

  console.log('inputsOpen (after):', inputsOpen);
  console.groupEnd();
}

const updateHeightWidth = () => {
  const valHeight = ((inputValues.value as unknown) as HTMLElement).clientHeight;
  const inHeight = ((inputInputs.value as unknown) as HTMLElement).clientHeight;
  const tmpWidth = Math.round((((contentInfo.value as unknown) as HTMLElement).clientWidth / 16) * 1000) / 1000;
  let tmpHeight = (valHeight > inHeight)
    ? valHeight
    : inHeight;

  contentWidth = `${tmpWidth.toString()}rem`;

  tmpHeight =  Math.round((tmpHeight / 16) * 1000) / 1000;

  maxHeight = `${tmpHeight.toString()}rem`;
  instance?.proxy?.$forceUpdate();
}


onMounted(() => {
  console.log('values', inputValues.value);
  console.log('inputs', inputInputs.value);
  console.log('contentInfo', contentInfo.value);
  updateHeightWidth();

  // resizeWatcher = new ResizeObserver(() => {
  //   updateHeightWidth();
  // });
});

</script>

<style>
/* .inputs-block__inputs {
} */
.inputs-block__btns {
  column-gap: 1rem;
  display: flex;
  flex-direction: row;
  height: 2.5rem;
  justify-content: space-between;
  row-gap: 1rem;
}
.inputs-block__btns > button {
  flex-grow: 1;
}
.inputs-block__content-wrap {
  box-sizing: border-box;
  overflow: hidden;
}
.inputs-block__content {
  box-sizing: border-box;
  column-gap: 2rem;
  display: inline-flex;
  flex-direction: row;
  max-width: fit-content;
  width: 200%;
}
.inputs-block__values {
  display: flex;
  flex-direction: column;
  width: 100%;
}
.inputs-block__content {
  transition: transform ease-in-out 0.3s;
}
@media (prefers-reduced-motion) {
  .inputs-block__content {
    transition: none;
  }
}
.inputs-block__content--viewing {
  transform: translateX(0);
}
.inputs-block__content--editing {
  transform: translateX(calc(calc(50% + 1rem) * -1));
}
.inputs-block__values ul {
  margin: 0;
  padding: 0;
  flex-grow: 1;
  list-style-type: none;
}
.inputs-block__values li {
  margin: 0;
  padding: 0.5rem 0;
  display: flex;
  column-gap: 0.5rem;
  row-gap: 0.5rem;
}
.inputs-block__label {
  font-weight: bold;
  white-space: nowrap;
  width: 6rem;
}
.inputs-block__values--inputs {
  opacity: 0;
  width: 0;
}
.inputs-block__values--inputs li {
  flex-direction: column;
}
.inputs-block__content-wrap {
  height: var(--values-height);
  position: relative;
}

.inputs-block__content--slide {
  position: absolute;
  top: 0;
  left: 0;
  width: calc(calc(var(--values-width) * 2) + 2rem);
}
.inputs-block__content--slide .inputs-block__values {
  width: var(--values-width);
  height: var(--values-height);
  opacity: 1;
}
</style>
