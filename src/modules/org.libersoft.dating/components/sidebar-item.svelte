<script>
 import { module } from '../module.js';
 import BaseButton from '@/core/components/Button/BaseButton.svelte';
 import Icon from '@/core/components/Icon/Icon.svelte';
 export let img = null;
 export let label = '';
 export let onClick;
 export let colorVariable = '--icon-black';
</script>

<style>
 .item {
  display: flex;
  align-items: center;
  gap: 10px;
  height: 30px;
  padding: 10px;
  background-color: #fffcf0;
  border-bottom: 1px solid #dd9;
 }
</style>

<BaseButton {onClick}>
 <div class="item">
  {#if img}
   <Icon img="modules/{module.identifier}/img/{img}" alt={label} {colorVariable} size="20" padding="0" />
  {/if}
  <div>{label}</div>
 </div>
</BaseButton>
