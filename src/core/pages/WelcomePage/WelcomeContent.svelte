<script>
 import BaseButton from '../../components/Button/BaseButton.svelte';
 import Icon from '../../components/Icon/Icon.svelte';
 import { product, version, build, commit, link } from '../../core.js';

 function clickLogo() {
  window.open(link, '_blank');
 }
</script>

<style>
 .welcome {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 5px;
  height: 100vh;
  background: url('/img/background.webp') repeat;
  background-size: 400px;
 }

 .welcome .logo {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 5px;
 }

 .welcome .logo .product {
  font-size: 40px;
  font-weight: bold;
 }

 .welcome .version {
  display: flex;
  gap: 5px;
  font-size: 16px;
 }
</style>

<div class="welcome">
 <BaseButton onClick={clickLogo}>
  <div class="logo">
   <Icon img="img/logo.svg" alt={product} size="200" />
   <div class="product">{product}</div>
  </div>
 </BaseButton>
 <div class="version">
  <div>Version:</div>
  <div class="bold">{version}</div>
 </div>
 <div class="version">
  <div>Build:</div>
  <div class="bold">{build}</div>
 </div>
 <div class="version">
  <div>Commit:</div>
  <div class="bold">{commit}</div>
 </div>
</div>
