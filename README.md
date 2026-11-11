<p align="center">
  <img src="spot-logo.jpg" alt="Spot" width="360">
</p>

# Spot

**Book a personal trainer for one session. No packages. No pressure.**

Spot is a booking site for people who want a single session with a vetted local personal trainer: beginners, people nervous about committing long term, and travelers who just want a good workout while in town. Clients see fixed prices and pay when they book. Trainers pay nothing unless they complete a session.

This repo holds the MVP site. It's plain HTML with no build step and no dependencies beyond Google Fonts.

| File | Purpose |
|---|---|
| `index.html` | Landing page |
| `booked.html` | "You're all set" page shown after a client pays |
| `spot-logo.jpg` | Logo used in this README |

---

## What's on the page

| Section | Purpose |
|---|---|
| Hero + search | Value proposition; the search bar scrolls to the trainer list |
| Sessions | Fixed-price menu; buttons scroll to Trainers |
| How it works | Three-step explainer: book, pay before the session, train |
| Trainers | Trainer cards with vibe tags, filter chips, and each trainer's Calendly link |
| FAQ | Answers aimed at nervous first-timers |
| For trainers | Founding and Standard fee tiers, take-home example, application link |
| Waitlist | Email + ZIP capture for areas not served yet |

---

## Setup checklist

Search `index.html` and `booked.html` for `TODO` and `YOUR_` to find each placeholder.

- [ ] **Trainer calendars:** each trainer makes their own free Calendly account with one event type (e.g. "Spot session") and asks for session type in the booking form; paste their links in place of `YOUR_TRAINER_CALENDLY_…` in `index.html`
- [ ] **Booking notifications:** have each trainer auto-forward Calendly booking emails to a shared Spot inbox, so you see every booking
- [ ] **Stripe Payment Links:** create one per session type (First Timer, Form Check, Travel Sweat, Bring a Friend). These aren't on the site; you send them to clients after they book
- [ ] **Post-payment redirect:** on each Payment Link, under **After the payment**, choose **Don't show confirmation page** and redirect to `https://<your-site>/booked.html`
- [ ] **QR codes:** download each Payment Link's QR code and give them to trainers as a day-of backup
- [ ] **Waitlist form:** create a free [Formspree](https://formspree.io) form and replace `YOUR_FORM_ID`
- [ ] **Trainer application:** create a [Tally](https://tally.so) form and replace `YOUR_TALLY_FORM`
- [ ] **Trainer roster:** replace the three sample trainers with real profiles
- [ ] **Photos:** replace the hero and trainer photo placeholders with real images
- [ ] **Prices:** confirm the session menu prices
- [ ] **Brand:** replace "Spot" if you choose a different name

---

## Deploying with GitHub Pages

1. Push or upload `index.html` to the `main` branch.
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, then select `main` and `/ (root)`.
4. Save. The site goes live at `https://<username>.github.io/<repo-name>/` within a few minutes.

**Custom domain:** add it under **Settings → Pages → Custom domain**, then point your domain's DNS at GitHub Pages and enable **Enforce HTTPS**.

### Editing from an iPad

Everything can be done in Safari. Tap **aA → Request Desktop Website** for the full GitHub interface. Open `index.html`, tap the pencil icon to edit, then commit. The live site updates automatically.

To add photos, use **Add file → Upload files**, then swap each placeholder `<div>` for an `<img>` tag:

```html
<img src="dana.jpg" alt="Dana R., personal trainer" class="photo" style="object-fit:cover">
```

---

## Customizing

### Colors

All colors are CSS variables at the top of the `<style>` block:

```css
:root {
  --accent: #1F5C46;       /* buttons, links, highlights */
  --accent-dark: #163F31;  /* hover states */
  --accent-soft: #E6F0EA;  /* tag backgrounds */
  ...
}
```

### Adding a trainer

Copy an existing `<article class="trainer">` block in `index.html` and set its button to the trainer's Calendly link. The `data-tags` attribute controls which filter chips show that trainer:

```html
<article class="trainer" data-tags="beginner women">
```

Available tags: `beginner`, `women`, `travel`. To add a new filter, add a matching `<button class="chip" data-filter="newtag">` in the chip group.

---

## Booking & payment flow

**Book first, pay before the session.**

1. Client picks a trainer on the landing page and books a slot on that trainer's Calendly, noting their session type.
2. Calendly emails the client and trainer; the trainer's email auto-forwards to the Spot inbox.
3. Spot texts or emails the client the Stripe Payment Link for their session type, ideally within a few hours of booking.
4. Payment is due before the session starts. Send one reminder the evening before or the morning of.
5. After paying, Stripe emails a receipt and redirects the client to `booked.html`.
6. Still unpaid at the session? The trainer shows the Payment Link's QR code at the start, and the client pays on their phone before training.
7. Each week, match Stripe payments to completed sessions, then pay trainers minus the platform fee.

**Message template (step 3):**
> Hi [name], you're booked with [trainer] on [day] at [time]. Here's your secure payment link for your [session type] session ($[price]): [link]. Payment is due before your session starts. See you there!

> **Why this setup:** Calendly's free plan allows one event type and one calendar per account, and doesn't include payments. So each trainer uses their own free Calendly for scheduling, and all money flows through Spot's Stripe Payment Links. Total software cost: $0 plus Stripe fees (2.9% + 30¢ per charge).
>
> **Trade-off:** with no card on file, a no-show or late cancellation can't be charged. Protect trainers by restricting repeat no-shows from booking again. If no-shows become a problem, the fix is saving a card at booking, which needs a paid scheduler or a small custom Stripe integration.
>
> **Manual work:** sending payment links, reminders, and rebooking or rating requests by hand. Budget a few minutes per booking.
>
> **When to upgrade:** once volume makes manual matching painful, move to a paid scheduler that collects payment at booking into one account, then to Stripe Connect for automatic payout splits.

---

## Trainer fee structure

| Tier | First session with a new client | Repeat sessions |
|---|---|---|
| **Founding Trainer** (first 10 only) | 15% | 15% |
| **Standard** | 25% | 15% |

- Spot covers card processing fees.
- The first-session refund guarantee comes out of Spot's share, never the trainer's.
- The founding rate is permanent while the trainer stays active, non-transferable, and applies only to sessions booked through Spot.

---

## Key metrics to track

- **First → second session rate** (the most important number)
- Visitor → booking conversion
- Bookings by session type
- Waitlist sign-ups by ZIP code (shows where to expand)
- Trainer utilization (booked slots ÷ listed slots)

---

## Roadmap

- [ ] Launch with 10 founding trainers in Loudoun County
- [ ] Run a 60–90 day MVP on Stripe Payment Links + free Calendly
- [ ] Add real search and filtering by session, location, and time
- [ ] Migrate to Stripe Connect for automatic payouts
- [ ] Build a native app once repeat-booking data justifies it
