<script>
  import 'smelte/src/tailwind.css'
  import { DataTable } from "smelte"

  import axios from 'axios'

  import TrackPageActive from './TrackPageActive.svelte'
  import Interval from './Interval.svelte'
  import Mail from './Mail.svelte'
  import Details from './Details.svelte'

  let mails = [];
  let loading = true;
  let selected;

  const poll = false;

  function mailsChanged(oldMails, newMails) {
    if (oldMails.length != newMails.length)
      return true

    return oldMails.some((mail, i) => JSON.stringify(mail) != JSON.stringify(newMails[i]))
  }

  function setMails(data) {
    if (mailsChanged(mails, data)) {
      mails = data

      if (selected) {
        const selectedJson = JSON.stringify(selected)
        const idx = mails.findIndex(mail => JSON.stringify(mail) == selectedJson)
        if (idx != -1) {
          selected = mails[idx]
        }
      }

      if (!selected && mails.length > 0) {
        selected = mails[0]
      }
    }
  }

  function load(indicate=false) {
    console.log("load")

    if (indicate)
      loading = true

    axios.get('/api/mails').then(response => {
      setMails(response.data)
      loading = false
    })
  }

  function selectMail(event) {
    selected = event.detail.mail;
  }

  load(true)
</script>

<svelte:head>
  <title>SendGrid Mock</title>
</svelte:head>

<TrackPageActive />

<Interval fn={load}
          interval={5000}
          onCreate={false}
          />

<div class="h-screen w-screen flex flex-col">
  <!-- <div class="bg-gray-300 h-16 w-full flex-grow-0 flex-shrink-0"> -->
  <!-- </div> -->

  <div class="flex-grow w-full h-full flex-grow flex flex-row">
    <div class="bg-gray-200
                p-3 flex-grow w-1/2 max-w-xl h-full
                overflow-auto">
      {#if mails.length == 0 && loading}
        Loading...
      {:else}
        {#each mails as mail (mail.datetime)}
          <Mail mail={mail}
                selected={mail == selected}
                on:select={selectMail}
                />
        {/each}
      {/if}
    </div>

    <Details mail={selected} />
  </div>
</div>
