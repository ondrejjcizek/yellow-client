<script>
 import { module } from '../module.js';
 import Content from '../components/content.svelte';
 import Panel from '../components/panel.svelte';
 import PhotoCard from '../components/photo-card.svelte';

 const photo = {
  img: 'modules/' + module.identifier + '/img/photos/1.webp',
  name: 'Name',
  description: 'Description',
 };
</script>

<Panel label="Match game" />
<Content>
 <PhotoCard {photo} />
</Content>
