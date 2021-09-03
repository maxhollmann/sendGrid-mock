<script>
  import { createEventDispatcher } from 'svelte';

  import NameAndEmail from './NameAndEmail.svelte'
  import Datetime from './Datetime.svelte'

  export let mail;
  export let selected;

  const dispatch = createEventDispatcher();

  function select() {
    dispatch('select', { mail });
  }
</script>

<div class="flex flex-row
            mb-2 p-3
            rounded cursor-pointer
            shadow
            transition duration-300"
     class:bg-blue-50={selected}
     class:hover:bg-blue-50={selected}
     class:bg-white={!selected}
     class:hover:bg-gray-100={!selected}
     on:click={select}
     >

  <div class="flex flex-col flex-grow">
    <div class="pb-1 font-medium">
      <NameAndEmail data={mail.personalizations[0].to[0]} />
    </div>
    <div class="text-xs">
      From: <NameAndEmail data={mail.from} />
    </div>
  </div>
  <Datetime t={mail.datetime} />
</div>
