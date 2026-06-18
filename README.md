# homerun-pinger

A tiny watchdog that pings an [HDHomeRun](https://www.silicondust.com/) (or any device with an
HTTP endpoint) and fires an [IFTTT](https://ifttt.com/) alert when it stops responding. Run it on a
schedule with cron and get notified the moment the device goes offline.

## How it works

Each run does a single `GET` against `HOMERUN_URL`. Anything other than a `200` — or a connection
failure — triggers the IFTTT Webhooks event, which can then send you a push, email, etc. Activity
is logged to `/var/tmp/homerunpinger.log`.

## Setup

1. Install the dependency:
   ```bash
   pip install requests
   ```
2. Create an IFTTT **Webhooks** applet and grab your key from
   <https://ifttt.com/maker_webhooks> → *Documentation*.
3. Edit the constants at the top of `ifttt.py`:
   - `IFTTT_KEY` — your Webhooks key
   - `HOMERUN_URL` — the URL expected to return `200` when the device is healthy
   - `EVENT` — must match the event name in your IFTTT applet
4. Schedule it with cron at whatever cadence you like ([crontab.guru](https://crontab.guru) helps):
   ```cron
   */5 * * * * /usr/bin/python3 /path/to/ifttt.py
   ```

## License

[MIT](LICENSE) © Corbin Johnson
