<script>
 import { debug } from '../../core.js';
 import { setContext, tick } from 'svelte';
 import BaseButton from '@/core/components/Button/BaseButton.svelte';
 import Icon from '@/core/components/Icon/Icon.svelte';
 export let show = false;
 export let params = null;
 export let title = null;
 export let body = '';
 export let width;
 export let height;
 export let onShowChange = null;
 let modalEl;
 let posX = 0;
 let posY = 0;
 let isDragging = false;
 let left;
 let top;
 let showContent = false;

 $: showUpdated(!!show);

 function setShow(value) {
  if (onShowChange) {
   onShowChange(value);
  } else {
   show = value;
  }
 }

 async function showUpdated(show) {
  console.log('showUpdated', show);
  if (show) {
   await tick();
   modalEl.focus();
   showContent = true;
   await tick();
   await positionModal();
  } else {
   showContent = false;
  }
 }

 function setTitle(value) {
  title = value;
 }

 setContext('setTitle', setTitle);
 setContext('pageChanged', positionModal);
 setContext('Popup', { close });

 async function positionModal() {
  console.log('positionModal');
  await tick();
  if (modalEl) {
   const modalRect = modalEl.getBoundingClientRect();
   top = (window.innerHeight - modalRect.height) / 2;
   left = (window.innerWidth - modalRect.width) / 2;
   // TODO: modal width and height are wrong when width and height are set (for example when showing modal with sticker set)
   console.log('MODAL SIZE:', modalRect.width + ' x ' + modalRect.height);
   console.log('WINDOW SIZE:', window.innerWidth + ' x ' + window.innerHeight);
  }
 }

 export function close() {
  setShow(false);
 }
 export function open() {
  setShow(true);
 }

 function onkeydown(event) {
  console.log('Modal onkeydown', event);
  if (event.key === 'Escape') {
   event.preventDefault();
   event.stopPropagation();
   console.log('ESC', event);
   setShow(false);
  }
 }

 function dragStart(event) {
  isDragging = true;
  event.preventDefault();
  const modalRect = modalEl.getBoundingClientRect();
  posX = event.clientX - modalRect.left;
  posY = event.clientY - modalRect.top;
  window.addEventListener('mousemove', drag);
  window.addEventListener('mouseup', dragEnd);
 }

 function drag(event) {
  if (isDragging) {
   let x = event.clientX - posX;
   let y = event.clientY - posY;
   const modalWidth = modalEl.offsetWidth;
   const modalHeight = modalEl.offsetHeight;
   const windowWidth = window.innerWidth;
   const windowHeight = window.innerHeight;
   left = Math.max(0, Math.min(windowWidth - modalWidth, x));
   top = Math.max(0, Math.min(windowHeight - modalHeight, y));
  }
 }

 function dragEnd() {
  isDragging = false;
  window.removeEventListener('mousemove', drag);
  window.removeEventListener('mouseup', dragEnd);
 }
</script>

<style>
 .modal {
  z-index: 100;
  display: flex;
  flex-direction: column;
  position: fixed;
  top: 0;
  left: 0;
  max-width: 100vw;
  max-height: 100vh;
  overflow: auto;
  border: 1px solid #000;
  border-radius: 10px;
  box-shadow: var(--shadow);
  background-color: #fff;
  /* TODO: this works, but it affects initial position */
  /*animation: modalAppear 0.2s ease-out;*/
 }
 /*
 @keyframes modalAppear {
  from {
   transform: scale(0);
  }
  to {
   transform: scale(1);
  }
 }
*/
 .modal .header {
  display: flex;
  align-items: center;
  gap: 10px;
  font-weight: bold;
  background-color: #fd3;
  color: #000;
  cursor: pointer;
 }

 .modal .header .title {
  padding: 0 10px;
  flex-grow: 1;
 }

 .modal .body {
  display: flex;
  flex-direction: column;
  gap: 10px;
  padding: 10px;
  overflow-y: auto;
  background-color: #fff;
  color: #000;
 }

 @media (max-width: 768px) {
  .modal {
   min-width: 100%;
   min-height: 100%;
   width: 100%;
   height: 100%;
   max-height: 100%;
   max-height: 100%;
   border: 0px;
   border-radius: 0px;
  }
 }
</style>

{#if show}
 <div class="modal" role="none" tabindex="-1" style:top={top && top + 'px'} style:left={left && left + 'px'} style:width={width && width} style:height={height && height} style:max-width={width && width} style:max-height={height && height} bind:this={modalEl} on:keydown={onkeydown}>
  {#if showContent}
   <div class="header" role="none" tabindex="-1" on:mousedown={dragStart}>
    <div class="title">{title}</div>
    <Icon img="img/close.svg" alt="X" colorVariable="--icon-black" size="20" padding="10" onClick={close} />
   </div>
   <div class="body">
    <slot>
     {#if $debug}params: <code>{JSON.stringify({ params })}</code>{/if}
     {#if params}
      <svelte:component this={body} {close} {params} />
     {:else}
      <svelte:component this={body} {close} />
     {/if}
    </slot>
   </div>
  {/if}
 </div>
{/if}
