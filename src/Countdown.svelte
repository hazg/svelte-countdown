<script>
  import { onMount } from "svelte"
  import dayjs from "dayjs"
  import duration from "dayjs/plugin/duration"
    dayjs.extend(duration)

  export let from, dateFormat

  let remaining = {
    years: 0,
    months: 0,
    weeks: 0,
    days: 0,
    hours: 0,
    minutes: 0,
    seconds: 0,
    done: false,
  }

  var timer

  onMount(() => {
    if (!dateFormat) {
      dateFormat = "YYYY-MM-DD H:m:s"
    }
    let target = dayjs(from, dateFormat)
    let local = dayjs()

    let diff = target.valueOf() - local.valueOf()
    function updateTime() {
      if (diff > 0) {
        let r = dayjs.duration(diff)
        remaining = {
          years: r.years(),
          months: r.months(),
          weeks: r.weeks(),
          days: r.days(),
          hours: r.hours(),
          minutes: r.minutes(),
          seconds: r.seconds(),
          done: false,
        }
        diff -= 1000
      } else {
        remaining = {
          years: 0,
          months: 0,
          weeks: 0,
          days: 0,
          hours: 0,
          minutes: 0,
          seconds: 0,
          done: true,
        }
        clearInterval(timer)
      }
    } updateTime()
    timer = setInterval(updateTime, 1000)
  })
</script>

<slot {remaining} />
