# radhe

talk to your terminal. it orders dinner and groceries.

a claude code plugin. claude becomes a calm caretaker — one persona, two MCP servers (swiggy food + instamart), one slash command. you say `i'm hungry`, dinner shows up. you say `out of milk`, milk shows up. radhe figures out the lane from what you said.

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

first run: a browser tab opens automatically for swiggy login. one tap. radhe pulls your name, phone, and saved delivery addresses from swiggy and caches them in `~/.radhe/prefs.json`. no separate address setup. that's the whole onboarding — covers both food and instamart (one swiggy account, one OAuth, both MCPs unlocked).

every run after that:

```
you ▸ /radhe
radhe ▸ radhe v0.1.4 — welcome back, abhishek. what do you need?
you ▸ something light. high protein. under ₹400.
radhe ▸ [5 restaurant cards, ranked bayesian]
you ▸ #2
radhe ▸ [cart, total]  you'll pay the rider in cash. confirm?
you ▸ yes
radhe ▸ order placed. tracking: ...
```

or:

```
you ▸ /radhe
radhe ▸ radhe v0.1.4 — welcome back, abhishek. what do you need?
you ▸ weekly stock-up. usual.
radhe ▸ [basket grouped by STAPLES / FRESH / PROTEIN / SNACKS, brand-loyal,
         eggs auto-added because cadence says you're due]
        all-in: ₹2,840 (incl. delivery + handling + gst)
        running this month: ₹6,040 / ₹8,000
        macro shape: ~480g protein, ~1100g carbs, ~310g fat over the week
        you'll pay the rider in cash. confirm?
you ▸ yes
radhe ▸ order placed. eta 18 min.
```

## what makes it different

### food

**bayesian ranking.** a 4.8 with 12 ratings ranks below a 4.3 with 4,000. star ratings are noisy when sample size is small. radhe corrects with a shrinkage average (`R, v, C=4, m=50`). the ⭐ in the card is the raw rating; the *order* is bayesian.

**balanced pick.** of the 5 cards, one shows kcal + protein. order whatever you want — radhe just adds the info quietly.

### groceries (the deeper bit)

**dietary identity is non-negotiable.** if you're vegetarian / jain / vegan / eggetarian, radhe filters non-conforming items out completely. allergens (gluten, lactose, nuts) and medical flags (diabetic, hypertension) get the same medical-grade respect.

**brand loyalty.** indians stick to specific brands of staples (atta, milk, oil, ghee). once radhe learns yours, she defaults to them silently — only breaking loyalty if out of stock or >25% pricier than the next best.

**replenishment cadence.** people order eggs every ~5 days, milk every ~2-3, atta monthly. radhe tracks when you last ordered each staple and silently adds items you're due for during a stock-up — never padding the basket, only what cadence math suggests.

**nutrient density × value.** if you've told radhe `nutrition_focus: high_protein`, ranking is grams-protein-per-₹100, not lowest price. paneer at ₹350/kg with 18g/100g protein beats whey at ₹3000/kg if you're on a budget.

**clean ingredient bias.** between two similar products, radhe silently prefers fewer additives, no added sugar, no palm oil, no maida-as-first-ingredient. no lecture; just better ranking.

**trolley discipline.** baskets get padded by 30-40% with impulse adds. radhe groups your basket by STAPLES / FRESH / PROTEIN / SNACKS / BEVERAGES / HOUSEHOLD with subtotals — you see the shape at a glance and notice "wait, ₹400 in snacks?" without being told.

**budget anchor.** if `grocery_budget_monthly` is set, you see `running this month: ₹A / ₹B` once at confirmation. no warnings, no nagging — just visibility.

**macro literacy.** humans underestimate carbs and overestimate protein in their carts. if `nutrition_focus` is set, the basket's macro shape is surfaced one line at confirmation.

### shared

**no address setup.** swiggy already knows your saved addresses. on first login, radhe pulls them in. used silently for both food and instamart.

**memory.** everything radhe learns about you lives in `~/.radhe/prefs.json` — name, phone, addresses, dietary identity, allergens, medical, brand loyalty, cadence, pantry, monthly spend. delete the file to reset; radhe will re-sync from swiggy on the next run.

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

- prefs (including dietary, medical, budget): local file, `~/.radhe/prefs.json`
- swiggy account, addresses, order history: held by swiggy under their normal terms
- nothing else leaves your laptop except swiggy MCP calls and the optional geocoder hit

## license

MIT
