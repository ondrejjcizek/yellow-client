<script>
 import BaseButton from '@/core/components/Button/BaseButton.svelte';
 import Icon from '@/core/components/Icon/Icon.svelte';
 export let text;
 export let colorVariable;
 export let onClick;
</script>

<style>
 .dropdown {
  display: flex;
  align-items: center;
  gap: 5px;
  padding: 5px 10px;
  border: 0;
  border-radius: 10px;
  font-family: inherit;
  background-color: #ffa;
 }

 .dropdown .text {
  flex-grow: 1;
 }
</style>

<BaseButton {onClick}>
 <div class="dropdown">
  <div class="text">{text}</div>
  <Icon img="img/down.svg" alt="▼" size="15" padding="0" colorVariable={colorVariable && colorVariable} />
 </div>
</BaseButton>
