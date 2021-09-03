<script>
  import Autolinker from 'autolinker'

  import NameAndEmail from './NameAndEmail.svelte'

  export let mail

  const autolinker = new Autolinker({
    newWindow: true,
    stripPrefix: false,
    stripTrailingSlash: false,
    className: 'underline',

    urls: true,
    email: true,
    phone: false,
    mention: false,
    hashtag: false,
  });

  $ : json = autolinker.link(JSON.stringify(mail, null, 2))
</script>

<div class="bg-gray-00 p-3 flex-grow w-1/2 h-full
            overflow-auto"
     >
  {#if mail}
    <pre>{@html json}</pre>
  {/if}
</div>
