<script>
  import Interval from './Interval.svelte'

  export let t
  const date = new Date(t)

  const updateInterval = (new Date() - date) / 1000 < 60 ? 1000 : 60*1000

  function formatDate() {
    const now = new Date()
    const secAgo = (now - date) / 1000

    if (secAgo < 0)
      return 'now'
    if (secAgo < 60)
      return `${Math.floor(secAgo)}s ago`
    if (secAgo / 60 < 60)
      return `${Math.floor(secAgo / 60)}m ago`

    function day(d) {
      const month = d.getMonth() + 1
      const day = d.getDate()
      const year = d.getFullYear()

      return `${month}/${day}/${year}`
    }

    function time(d) {
      const hours = d.getHours()
      const minutes = d.getMinutes()
      const ampm = hours >= 12 ? 'pm' : 'am'

      return `${hours}:${minutes} ${ampm}`
    }

    const dateDay = day(date)
    if (dateDay != day(now))
      return dateDay
    else
      return time(date)
  }

  function update() {
    formatted = formatDate()
  }

  let formatted = '';
</script>

<Interval fn={update} interval={updateInterval} />

<div class="flex-grow-0 overflow-hidden
            text-blue-700 text-right">
  {formatted}
</div>
