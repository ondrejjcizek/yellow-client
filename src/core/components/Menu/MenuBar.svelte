<script>
 import BaseButton from '../Button/BaseButton.svelte';
 import Icon from '../Icon/Icon.svelte';
 import Switch from '@/core/components/Switch/Switch.svelte';

 import { product, debug } from '../../core.js';
 import { addNotification } from '../../notifications.ts';
 import { log } from '../../tauri.ts';
 export let isMenuOpen;
 let menuIconColor = '';

 function handleClick() {
  isMenuOpen = true;
 }
</script>

<style>
 .bar {
  display: flex;
  align-items: center;
  height: var(--menu-height);
  min-height: var(--menu-height);
  background-color: #fd1;
  color: #222;
 }

 .product {
  font-size: 22px;
  font-weight: bold;
 }
</style>

<div class="bar">
 <Icon img="img/menu.svg" alt="☰" colorVariable="--icon-black" size="30" padding="10" onClick={handleClick} />
 <div class="product">{product}</div>
 {#if import.meta.env.VITE_YELLOW_CLIENT_DEBUG}(debug mode: <Switch bind:checked={$debug} />){/if}
 {#if $debug}
  <BaseButton
   onClick={async () => {
    //log.debug('addNotification...');
    await addNotification({
     body: 'bla bla',
     callback: event => {
      log.debug('debug notification callback ' + event);
     },
    });
   }}>addNotification</BaseButton
  >
 {/if}
</div>
