# radhe

talk to your terminal. it orders dinner.

a claude code plugin. claude becomes a calm, taste-driven dinner agent. it connects to the official swiggy MCP, ranks restaurants properly (bayesian, not raw stars), remembers your address, and orders in three sentences.

india only. cash on delivery only.

## install

in claude code:

```
/plugin marketplace add bestkanthed/radhe-skill
/plugin install radhe@bestkanthed
```

restart claude code if prompted.

## use

```
/radhe
```

first run: a browser tab opens for swiggy login. one tap. radhe then pulls your name, phone, and saved delivery addresses straight from swiggy and caches them locally. no separate address setup. that's the whole onboarding.

every run after that:

```
you ▸ /radhe
radhe ▸ welcome back. what's for dinner?
you ▸ something light. high protein. under ₹400.
radhe ▸ [5 restaurant cards, ranked]
you ▸ #2
radhe ▸ [cart, total]
        you'll pay the rider in cash. confirm?
you ▸ yes
radhe ▸ order placed. tracking: swiggy.com/order/...
```

## what makes it different

**bayesian ranking.** a 4.8 with 12 ratings ranks below a 4.3 with 4,000. star ratings are noisy when the sample is small. radhe corrects for that with a shrinkage average (`R, v, C=4, m=50`). the ⭐ in the card is the raw rating; the *order* is bayesian.

**balanced pick.** of the 5 cards, one shows kcal + protein. order whatever you want — radhe just adds the info quietly.

**no address setup.** swiggy already knows your saved addresses (Home, Office, …). on first login, radhe pulls them into `~/.radhe/prefs.json`. the default delivery address is chosen automatically. you only get asked when you're somewhere new.

**memory.** name, phone, addresses, and taste rules (`no paneer`, `₹500 weeknight cap`, `vegetarian`) live in `~/.radhe/prefs.json`. delete the file to reset (radhe will re-sync from swiggy on the next run).

**cash on delivery only.** no card details ever leave your machine. pay the rider.

**radhe is quiet.** lowercase. one question per turn. no emoji. no preaching. you say `i want junk`, you get the burger without a wellness lecture.

## optional

precise geocoding is opt-in and only matters for ad-hoc addresses (a friend's place, a hotel) that aren't saved to swiggy. set one of:

```bash
export MAPBOX_TOKEN="..."
# or
export GOOGLE_MAPS_API_KEY="..."
```

without either, radhe falls back to swiggy's own address resolution from text + pincode.

## requires

- claude code 2.0+
- a swiggy account (indian phone number)

## privacy

- prefs and address: local file, `~/.radhe/prefs.json`
- swiggy account, addresses, order history: held by swiggy under their normal terms
- nothing else leaves your laptop except swiggy MCP calls and the optional geocoder hit

## license

MIT
