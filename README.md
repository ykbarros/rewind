# Rewind

Hear the song, guess the year, build your timeline. A phone-friendly music party game with two playlists:

- **K-Pop:** 254 Korean hits from 1976 to 2025, from Cho Yong-pil and Seo Taiji and Boys through H.O.T., BIGBANG and Girls' Generation to BTS, BLACKPINK, NewJeans and ROSÉ.
- **USA Hits:** 370 American hits from 1954 to 2025, from Elvis and Motown to Beyoncé, Taylor Swift and Sabrina Carpenter.

How a turn works: flip the music phone face down and a mystery song plays (a 45-second clip from the chorus, or the full song). Then place it in your timeline, oldest to newest. If you get the spot right, you keep the song. Call the exact year too and you win a bonus song. First to the target (6, 8, 10 or 12 songs) wins.

Play it three ways:
- **Versus:** 2 or more players, each with their own timeline.
- **Teams:** two teams of 2 or more.
- **Co-op:** 1 or more players build one timeline together with 2, 3 or 5 lives. A wrong spot costs a life and an exact year wins one back.

You can narrow either playlist to certain eras. Short rules show before you start, and the **?** button brings them back mid-game.

## Two ways to play
- **One phone:** the phone plays the music and gets passed around. Flip it face down to start the song, so nobody sees the title. After that, the player taps **Place it**.
- **Rooms:** the host taps *Start a room* and shares the 4-letter code or invite link. The host's phone plays the music. Everyone else places songs on their own phone and watches the others place theirs live. Rooms are peer-to-peer (WebRTC via [PeerJS](https://peerjs.com/)), so there's no server to run. Anyone who drops can rejoin.

## Music
Songs play through YouTube's embedded player on the music phone. If that browser is signed in to YouTube Premium, songs should play without ads. To find each song, the game needs a free **YouTube Data API v3 key**, which you paste in once under *Set up music* (the steps are in the app). The key is stored only in that browser.

- Each new song costs one search (about 100 units). The free quota of 10,000 units a day covers roughly 100 new songs a day. Songs found once are remembered on that device.
- If a video won't play outside YouTube, the game switches to the next search result on its own. At the reveal, *Wrong video?* lets you pick a different one, and your pick is remembered.
- **Flip to play:** when the phone goes face down, the song starts. iPhones ask for motion access once. If a browser blocks sound that wasn't started by a tap, or there's no motion sensor (laptops), you tap the record instead.

A single self-contained `index.html` with no build step.

## Dev notes
- Songs live at the top of the first `<script>` in `index.html`, one per line: `year|Artist|Title`, with an optional 4th field to override the YouTube search. The year is when that version of the song first came out (album or single, whichever came first).
- `?net=local` swaps PeerJS for a same-browser BroadcastChannel transport, so you can play a room across several tabs with no internet.
- `?peerhost=127.0.0.1:9000` points PeerJS at a self-hosted PeerServer (`npx peer --port 9000`).
- `window.__rewind` exposes the state for automated tests.
