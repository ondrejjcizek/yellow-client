<script>
 import { humanSize } from '@/core/utils/fileUtils.js';
 import Input from '@/core/components/Input/Input.svelte';
 import Button from '@/core/components/Button/Button.svelte';
 import { uploadChunkSize, hideMessageTextInNotifications, defaultFileDownloadFolder, photoRadius } from '../messages.js';
 import Switch from '@/core/components/Switch/Switch.svelte';
 import { open } from '@tauri-apps/plugin-dialog';
 import { TAURI } from '@/core/tauri.ts';
 import Select from '@/core/components/Select/Select.svelte';
 import Option from '@/core/components/Select/SelectOption.svelte';
 export let close;
 let chunkSize = $uploadChunkSize;

 function clickSetChunkSize() {
  uploadChunkSize.set(chunkSize);
  close();
 }

 async function defaultFileDownloadFolderButtonClick() {
  const file = await open({
   directory: true,
   multiple: false,
   title: 'Select default file download folder',
  });
  if (file) {
   defaultFileDownloadFolder.set(file);
  }
 }
</script>

<style>
 .group {
  display: flex;
  flex-direction: column;
  gap: 5px;
 }

 input[type='range'] {
  width: 100%;
  max-width: 300px;
 }
</style>

<div class="group">
 <div class="label">
  <span class="bold">File upload chunk size:</span>
  <span>{humanSize(chunkSize)}</span>
 </div>
 <Input type="number" bind:value={chunkSize} />
 <input class="zoom" type="range" min="1024" max="52428800" step="1024" bind:value={chunkSize} />
</div>
<Button text="Save" onClick={clickSetChunkSize} />

<div class="group">
 <div class="label">
  <span class="bold">Privacy</span>
 </div>
 <Switch bind:checked={$hideMessageTextInNotifications} label="Hide message text in notifications" />
</div>

<div class="group">
 <div class="label">
  <span class="bold">Photo radius:</span>
 </div>
 <Select bind:value={$photoRadius}>
  <Option value="50%" text="Circle" selected={true} />
  {#each Array(7) as _, i}
   <Option value="{i * 5}px" text="{i * 5} px" />
  {/each}
 </Select>
</div>

{#if TAURI}
 <div class="group">
  <div class="label">
   <span class="bold">Default file download folder (todo)</span>
  </div>
  {$defaultFileDownloadFolder}<br />
  <span class="label">This is the folder where files will be downloaded by default.</span>
  <Button text="Change" onClick={defaultFileDownloadFolderButtonClick} />
 </div>
{/if}
