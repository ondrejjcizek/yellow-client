<script>
 import Button from '@/core/components/Button/Button.svelte';
 import { getContext, setContext } from 'svelte';

 export let close;
 export let params;

 let nextText = 'Next';
 let currentStep = 0;
 let steps = params.steps;
 let setTitle = getContext('setTitle');
 let pageChanged = getContext('pageChanged');

 function setNextText(text) {
  console.log('setNextText:', text);
  nextText = text;
 }

 setContext('wizard', { setNextText });

 $: setTitle(steps[currentStep].title);

 async function nextStep() {
  if (currentStep < steps.length - 1) currentStep += 1;
  console.log('currentStep:', currentStep, 'pageChanged:', pageChanged);
  if (pageChanged) await pageChanged();
 }

 async function prevStep() {
  if (currentStep > 0) currentStep -= 1;
  console.log('currentStep:', currentStep, 'pageChanged:', pageChanged);
  if (pageChanged) await pageChanged();
 }
</script>

<style>
 .wizard {
  display: flex;
  flex-direction: column;
  gap: 10px;
  max-width: 100vw;
  max-height: 100vh;
  background-color: #fff;
  color: #000;
 }

 .progress-bar {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 20px;
 }

 .progress-bar .step {
  display: flex;
  align-items: center;
 }

 .progress-bar .step .circle {
  width: 30px;
  height: 30px;
  border-radius: 50%;
  background-color: #ccc;
  text-align: center;
  line-height: 30px;
  position: relative;
 }

 .progress-bar .step .circle.active {
  border: 1px solid #d80;
  background-color: #fd3;
  color: #000;
 }

 .progress-bar .step .line {
  width: 50px;
  height: 2px;
  background-color: #ccc;
 }

 .navigation {
  display: flex;
  gap: 10px;
 }

 .navigation .gap {
  flex-grow: 1;
 }
</style>

<div class="wizard">
 <div class="progress-bar">
  {#each steps as step, index}
   <div class="step">
    <div class="circle {index === currentStep ? 'active' : ''}">
     {index + 1}
    </div>
    {#if index < steps.length - 1}
     <div class="line"></div>
    {/if}
   </div>
  {/each}
 </div>
 <div class="content">
  <svelte:component this={steps[currentStep].component} {params} />
 </div>
 <div class="navigation">
  {#if currentStep > 0}
   <Button text="Previous" onClick={prevStep} />
  {/if}
  <div class="gap"></div>
  {#if currentStep < steps.length - 1}
   <Button data-testid="wizard-next" text={nextText} onClick={nextStep} />
  {:else}
   <Button data-testid="wizard-next" text="Finish" onClick={close} />
  {/if}
 </div>
</div>
