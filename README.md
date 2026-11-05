# Pink City Day Planner 🏰

A single-page itinerary planner that turns a visitor's free time, budget and interests into a realistic, timed plan for exploring Jaipur. It respects opening hours and travel time, and it surfaces lesser-known places alongside the famous ones.

**Live demo:** _add your link here (GitHub Pages or the shared artifact link)_

## The problem

Visitors with only a few hours in a city often:

- pick stops that are closed, far apart, or do not fit their time,
- crowd into the same famous landmarks,
- never hear about hidden spots that locals love.

Planning this by hand across maps, timings and ticket prices is slow. This tool does it in one tap.

## Features

- **Personalised plans.** Choose start time, hours available (2 to 9), entry-fee budget, pace (relaxed, normal, quick) and interests (heritage, food, shopping, photography, culture, hidden gems).
- **Opening-hours aware.** Stops that are closed at arrival or would not finish before closing are skipped. If a place opens shortly after you arrive, the plan waits for it.
- **Travel-time aware.** Walking or auto/cab, with time estimated between stops.
- **Budget aware.** Stops are added only while the estimated entry cost stays within budget.
- **Starting point.** Start from any listed place or from your current GPS location.
- **Insider tips.** Every stop has a short description and a practical tip.
- **Maps links.** Each stop opens in Google Maps.
- **Share or copy.** Export the plan as text using the native share sheet or clipboard.
- **Community contributions.** Locals can add hidden spots (name, type, coordinates, hours, cost, tip). They are saved in the browser and used in future plans.
- **Responsive and theme-aware.** Works on phones and follows light or dark mode.

## How it works

The planner is a greedy scheduler that runs entirely in the browser:

1. Each place gets a score: 1 point as a base, plus 3 for every tag matching a selected interest, plus 1 extra for hidden gems when that interest is on.
2. Starting from your chosen location and start time, it looks at every unvisited place and computes:
   - travel time (distance × 1.3 road factor, at 4.5 km/h walking or 18 km/h plus 6 minutes by auto/cab),
   - arrival time, with a short wait allowed if the place opens within 40 minutes,
   - visit duration (scaled by pace),
   - whether it finishes before both closing time and the end of your time window,
   - whether it fits the remaining budget.
3. It picks the feasible place with the best **score per minute spent** (travel plus visit), moves there, and repeats until nothing else fits.
4. If you select interests, places with no matching tag are excluded.

Distances use the haversine formula on straight-line coordinates, so travel times are estimates, not live routing.

## Run it

No build step and no dependencies.

```bash
git clone https://github.com/<your-username>/<your-repo>.git
cd <your-repo>
open index.html      # or just double-click the file
```

To host it free on **GitHub Pages**: rename `planner.html` to `index.html`, push, then go to _Settings → Pages_ and select your branch.

## Customise for another city

All place data is in the `BASE` array near the top of the script. Each entry looks like this:

```js
{
  n: "Hawa Mahal",            // name
  t: ["heritage", "photo"],   // tags: heritage, food, shopping, photo, culture, hidden
  lat: 26.9239, lng: 75.8267, // coordinates
  d: 40,                      // typical minutes to spend
  o: H(9), c: H(16, 30),      // opens, closes (H(hour, minute))
  $: 50,                      // entry fee in rupees
  spend: 70,                  // optional: typical food or shopping spend
  tip: "Short insider tip",
  w: "One-line description"
}
```

Replace the entries, update the title and copy, and the planner works for any city.

## Data and limitations

- The 13 starter places use **approximate** hours and prices from general knowledge. Verify them before relying on them, and replace them with official or locally collected data.
- Travel time is a straight-line estimate, with no live traffic or routing.
- User-added places are stored in `localStorage`, so they are private to one browser and are not shared between users.
- Closed days (such as weekly market closures) and festival timings are not modelled.
- The greedy approach gives good, fast plans, not guaranteed optimal ones.

## Roadmap

- Shared, moderated community database for local recommendations
- Day-of-week and festival-aware opening hours
- Live routing and traffic through a maps API
- Multi-language support (Hindi first)
- Offline use as an installable web app
- Verified listings for artisans, guides and family-run eateries

## Tech

Plain HTML, CSS and JavaScript in a single file. No frameworks, no tracking, no API keys.

## Contributing

Pull requests are welcome, especially corrections to place data and new city datasets. Please include a source for any hours or prices you change.

## License

MIT. Add a `LICENSE` file to your repo.
