# reed-web-demo

A minimal browser demo for the [`reed-web`](https://www.npmjs.com/package/reed-web)
npm package: hold the mic, speak, release — clean text comes back. It consumes the
**published package** (not a local checkout), so it doubles as a smoke test of the
real install path.

## Run it

The demo needs a [reed-backend](https://github.com/MiDa-Inc/reed-backend) to talk
to. For a keyless local one (mock providers — returns canned text, no API keys):

```sh
docker build -t reed-backend ../reed-backend
docker run --rm -p 8080:8080 reed-backend
```

Then:

```sh
npm install
npm run dev        # vite → http://localhost:5173
```

Open the page, hold the mic, speak, release. With the mock backend every take
comes back as "This is a mock transcription." — point the endpoint field at a
backend started with `TRANSCRIBE_PROVIDER=groq` (+ keys) for real transcription.

## Notes

- Mic access requires a secure context — `localhost` qualifies, plain `http://` IPs don't.
- The backend's CORS defaults to `*` (`WEB_ORIGIN` env var) so the vite origin is allowed out of the box.
- Silence is skipped client-side by reed-web (−45 dBFS floor): release without speaking and nothing is uploaded.
