<script>
 import BaseButton from '../Button/BaseButton.svelte';
 import Icon from '../Icon/Icon.svelte';
 export let items = [];
 export let activeIndex = null;

 function handleClick(index) {
  activeIndex = activeIndex === index ? null : index;
 }
</script>

<style>
 :root {
  --accordion-border-color: #b90;
 }

 .accordion {
  border: 1px solid var(--accordion-border-color);
  border-radius: 10px;
  overflow: auto;
 }

 .accordion .item {
  border-bottom: 1px solid var(--accordion-border-color);
 }

 .accordion .item .header {
  display: flex;
  align-items: center;
  padding: 10px;
  background-color: #fd1;
 }

 .accordion .item .header .title {
  flex-grow: 1;
  font-weight: bold;
 }

 .accordion .item .content {
  padding: 10px;
  display: none;
 }

 .accordion .item .content.active {
  display: block;
 }
</style>

<div class="accordion">
 {#each items as i, index}
  <div class="item">
   <BaseButton onClick={() => handleClick(index)}>
    <div class="header">
     <div class="title">{i.name}</div>
     <Icon img="img/down.svg" alt="▼" colorVariable="--icon-black" size="12" />
    </div>
   </BaseButton>
   <div class="content {activeIndex === index ? 'active' : ''}">
    <slot prop={i} />
   </div>
  </div>
 {/each}
</div>
