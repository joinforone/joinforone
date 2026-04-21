# JoinForOne

A full Next.js + Stripe + Supabase starter for the JoinForOne $1 experiment.

## What this does

- Lets a user enter an alias and country
- Creates a Stripe Checkout Session
- Redirects the user to Stripe
- Receives `checkout.session.completed` in a webhook
- Inserts the paid entry into Supabase
- Shows a success page using `session_id`
- Displays recent paid entries on the homepage

## 1. Install

```bash
npm install
```

## 2. Add environment variables

Copy `.env.example` to `.env.local` and fill in your real values.

```bash
cp .env.example .env.local
```

Required values:

- `NEXT_PUBLIC_SITE_URL`
- `NEXT_PUBLIC_SUPABASE_URL`
- `NEXT_PUBLIC_SUPABASE_PUBLISHABLE_KEY`
- `SUPABASE_SERVICE_ROLE_KEY`
- `STRIPE_SECRET_KEY`
- `STRIPE_WEBHOOK_SECRET`
- `STRIPE_CURRENCY`
- `STRIPE_UNIT_AMOUNT`

## 3. Create the table in Supabase

Run the SQL in `supabase/schema.sql`.

## 4. Run locally

```bash
npm run dev
```

Visit:

```text
http://localhost:3000
```

## 5. Start Stripe webhook forwarding locally

Install Stripe CLI, then run:

```bash
stripe listen --forward-to localhost:3000/api/webhook
```

Stripe CLI will print a webhook signing secret. Put that value in:

```text
STRIPE_WEBHOOK_SECRET=whsec_...
```

## 6. Production webhook URL

When deployed, use:

```text
https://joinforone.co/api/webhook
```

Subscribe at minimum to:

- `checkout.session.completed`

## Notes

- This version uses **custom Stripe Checkout Sessions**, not a fixed Payment Link, because that is the correct way to capture alias/country and show a true entry number after payment.
- Do not expose your Stripe secret key or Supabase service role key in the browser.
