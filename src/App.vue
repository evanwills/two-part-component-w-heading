<script setup lang="ts">
import InputsWinfoBlock from './components/InputsWinfoBlock.vue';
import InputsBlock from './components/InputsBlock.vue';
</script>

<template>
  <div class="in-w-disc--wrap inputs-w-info--alternate">
    <InputsWinfoBlock id="in-w-disc1" heading="Transportation">
      <template #info>
        <h4>Transport preferences</h4>
        <p>We are asking for information about your preferred mode(s) of transport.</p>
        <p>We are collecting this information to help us provide the right sort of bicycle for you.</p>
      </template>
      <template #inputs>
        <InputsBlock></InputsBlock>
      </template>
    </InputsWinfoBlock>
    <InputsWinfoBlock id="in-w-disc2" heading="Transportation" input-percent="45">
      <template #info>
        <h4>Transport preferences</h4>
        <p>We are asking for information about your preferred mode(s) of transport.</p>
        <p>We are collecting this information to help us provide the right sort of bicycle for you.</p>
      </template>
      <template #inputs>
        <p>
          <label for="transport-type">Preferred Transport type:</label>
          <select id="transport-type">
            <option>-- please choose --</option>
            <option value="walk">Walking</option>
            <option value="bicycle">Bicycle</option>
            <option value="unicycle">Unicycle</option>
            <option value="trike">Trike</option>
            <option value="skates">Roller Skates or Inline skates</option>
            <option value="ebike">eBike</option>
            <option value="eschooter">eScooter / eSkateboard / etc</option>
            <option value="train">Train</option>
            <option value="tram">Light rail</option>
            <option value="bus">Bus</option>
            <option value="ferry">Ferry</option>
            <option value="vandal">Environmental vandalism</option>
          </select>
        </p>
        <p>
          <label for="distance">Distance traveled each week (km):</label>
          <input type="number" id="distance" step="0.1" min="0" max="100" />
        </p>
      </template>
    </InputsWinfoBlock>
  </div>
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}
</style>
