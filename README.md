# Chat Slam

A little pro-wrestling match for chat, as an OBS browser source. A chatter types `!wrestle somebody` and a
pixel-art arena grows in along the bottom of the screen: a ring with turnbuckles and ropes under swaying
spotlights, and a crowd on the bleachers holding up signs and taking photos. The two of them run in, leap over
the top rope, and the ref rings the bell.

Then it's a simulated match, with health bars:

- dropkicks, clotheslines, hip tosses, body slams, suplexes, elbow drops and flying crossbodies off the top
  turnbuckle, each with a comic "WHAM!";
- the loser gets a comeback, and there's a near fall or two ("1… 2… KICKOUT!");
- then the winner's own finishing move (the same chatter always has the same one), and the ref counts 3.

The winner holds up the title belt in a shower of confetti; the loser lies there seeing stars. Nobody gets
hurt. Then it all shrinks away and there's nothing on screen until the next match.

Everything is a full 3D voxel model, drawn at a low resolution and seen flat on with no perspective, so it looks
like 2D pixel art; only the floor and the mat show a little depth (the 2.5D look). The wrestlers are little
chibis in a singlet of their chat color, and the same chatter always gets the same look (some get a luchador
mask). Twitch chatters who have set their pronouns with the [Chat Pronouns](https://pronouns.alejo.io/) extension
get a look to match. (Turn that off with `pronouns=0`.)

It's the sister of [Duel](https://github.com/Ixitxachitl/duel-game) and shares its settings, chat and voxel code.

Lives at <https://slam.ixitxachitl.com/>.

## Setup

In OBS: **Add → Browser**, URL `https://slam.ixitxachitl.com/?channel=yourchannel`, at your canvas size (e.g.
1920×1080). The background is transparent inside OBS.

It pops up in a small box along the bottom of the screen, in the middle, and everything stays inside it. Set
**Where it shows** (`layout=wide`) to use the whole screen instead, with the sky and lights going right up to the
top; **Box width** (`boxsize`, % of the screen, default 40) sets how big the box is.

Right-click the source → **Interact** and move the mouse over it:

- The bar in the bottom-right corner starts a match between two names you type, and **Stop** ends one early.
- The **gear** opens the settings: your Twitch / Kick channel, TikTok or YouTube, the command, the cooldown,
  who can start a match, pronoun lookups, the show's name on the ring apron, the pixel size and the volume.

Every setting can also be a URL parameter, which overrides it. The full list is in the comment at the top of
`index.html`. `?demo=1` starts a match every so often, for trying it out.

## Chat

| Command          | What                                                                                     |
| ---------------- | ---------------------------------------------------------------------------------------- |
| `!wrestle name`  | starts a match with `name` (`@name` works too), if they've chatted in the last 10 minutes |
| `!wrestle`       | picks someone at random from the people who chatted in the last 10 minutes                |

- After a match there's a **cooldown** (60 seconds by default) before the next one. The broadcaster skips it,
  and so can mods if you turn that on.
- **Who can start one** is everyone by default, or mods and VIPs, mods, or only the broadcaster. **These
  people can too** adds particular names.
- You can't wrestle yourself, and while a match is on, other `!wrestle`s are ignored.

Chat is read anonymously: Twitch and Kick directly, YouTube with a Data API key (or a WebSocket bridge), TikTok
through a bridge such as TikFinity.

Custom integrations (Streamer.bot, etc.) can call `window.slam.start(challenger, opponent, { winner })` (winner
1 or 2, otherwise a coin flip) and `window.slam.stop()`. These skip the cooldown and the 10-minute rule.

## Screenshots

Headless Chrome doesn't run animation frames under `--virtual-time-budget`, so add `timer=1` (frames on timers
instead):

```sh
chrome --headless=new --hide-scrollbars --window-size=1920,1080 --use-angle=swiftshader --enable-unsafe-swiftshader \
  --virtual-time-budget=13000 --screenshot=shot.png "https://slam.ixitxachitl.com/?demo=1&volume=0&timer=1"
```
