<script>
 import { identifier, online } from '../../messages.js';
 import Button from '@/core/components/Button/Button.svelte';
 import Modal from '@/core/components/Modal/Modal.svelte';
 import ModalNewConversation from '../../modals/NewConversation.svelte';
 import { active_account } from '@/core/core.js';

 const illustrations = ['man', 'woman', 'pigeon'];
 let showNewConversationModal = false;

 function clickNew() {
  showNewConversationModal = true;
 }
</script>

<style>
 .welcome {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 10px;
  height: 100vh;
  background: url('/img/background.webp') repeat;
  background-size: 400px;
 }

 .welcome .illustration {
  width: 350px;
  max-width: (100% - 20px);
  height: 350px;
  max-height: (100% - 20px);
 }

 .welcome .label {
  display: flex;
  flex-direction: column;
  gap: 8px;
  padding: 10px;
  border: 1px solid #fd1;
  border-radius: 20px;
  text-align: center;
  font-size: 20px;
  background-color: rgba(255, 221, 17, 0.3);
 }
</style>

<div class="welcome">
 <img class="illustration" src="modules/{identifier}/img/illustration-{illustrations[Math.floor(Math.random() * illustrations.length)]}.svg" alt="Illustration" />
 <div class="label">
  {#if $online}
   <div>Select your conversation<br />or</div>
   <Button text="Start a new one" padding="5px" onClick={clickNew} />
  {:else if $active_account}
   <div>This module is offline</div>
  {:else}
   <div>Select account...</div>
  {/if}
 </div>
</div>
<Modal title="New Conversation" body={ModalNewConversation} bind:show={showNewConversationModal} />
