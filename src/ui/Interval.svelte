<script>
  import { onDestroy, afterUpdate } from 'svelte';
  import { pageActive } from './stores.js';

  export let interval = 1000;
  export let fn = () => {}
  export let onlyWhenPageActive = true;
  export let restartOnActivate = true;
  export let onCreate = true;

  let clearToken;

  function restart() {
    // console.log(`restart, clearToken=${clearToken}`)
    if (clearToken)
      stop()
    start()
  }

  function start() {
    // console.log("starting")
    if (!clearToken)
      clearToken = setInterval(doIt, interval)
  }

  function stop() {
    // console.log("stop")
    clearInterval(clearToken)
    clearToken = null
  }

  function doIt() {
    if (!onlyWhenPageActive || $pageActive) {
      // console.log("doing it at " + new Date())
      fn()
    }
  }

  let firstUpdate = true
  pageActive.subscribe(value => {
    // console.log(`subscribe value=${value}, first=${firstUpdate}`)
    if (firstUpdate) {
      firstUpdate = false
      return
    }

    if (value && restartOnActivate) {
      stop()
      fn()
      start()
    }
  });

  if (onCreate)
    fn()
  start()

  onDestroy(stop)
  afterUpdate(restart)
</script>
