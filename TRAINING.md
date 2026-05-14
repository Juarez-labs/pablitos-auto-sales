# Training Walkthrough: Setting Up Decap CMS for Frank

This is the internal walkthrough for **you (Jose)** — not Frank. It covers:

1. Moving the live site from Cloudflare Pages → Netlify (one-time)
2. Enabling Netlify Identity + git-gateway (one-time)
3. Testing the admin yourself before you train Frank
4. The 30-minute Zoom training script for Frank

Read it all once before you start. The whole setup takes ~20 minutes if you don't get stuck. Test the CMS yourself end-to-end before you invite Frank.

---

## Part 1 — Connect the repo to Netlify (~5 minutes)

We already pushed everything to GitHub at:
**https://github.com/Juarez-labs/pablitos-auto-sales**

Now connect it to Netlify so they auto-deploy whenever a change is committed.

**Steps:**

1. Go to **https://app.netlify.com/signup** and sign up with the **Juarez-labs** GitHub account (or sign in if you have a Netlify account already).

2. On the Netlify dashboard, click **"Add new site"** → **"Import an existing project"**.

3. Choose **"Deploy with GitHub"**. Authorize Netlify if it asks.

4. Pick the **`pablitos-auto-sales`** repo from the list.

5. On the build settings screen:
   - **Branch to deploy:** `main`
   - **Build command:** *(leave blank)* — there's no build step
   - **Publish directory:** `.` (just a dot, meaning the root)

   Netlify will read `netlify.toml` in the repo for the rest of the config.

6. Click **"Deploy site"**.

After ~30 seconds, you'll get a random Netlify URL like `pablitos-auto-sales-abc123.netlify.app`. Visit it — the site should look identical to the Cloudflare version.

**Optional rename:** In Site settings → "Change site name", rename it to `pablitos-auto-sales` so the URL becomes `pablitos-auto-sales.netlify.app` (cleaner). Frank's real domain comes later.

---

## Part 2 — Enable Netlify Identity (~3 minutes)

Identity is Netlify's free login system. It's what powers the email/password login at `/admin/`.

**Steps:**

1. In your Netlify site dashboard, click **"Integrations"** (sometimes called "Add-ons" depending on UI version).

2. Find **"Identity"** and click **Enable**.

3. Once enabled, you'll see an **Identity** tab in the site navigation. Click it.

4. Under **Registration preferences**, set it to **"Invite only"** (you don't want random people signing up).

5. Scroll down to **"Services"** → find **"Git Gateway"** → click **Enable Git Gateway**.

   Netlify will ask you to authenticate with GitHub. Authorize it.

   This is the bit that lets Decap CMS commit changes to the repo on Frank's behalf when he uses the admin.

6. Back at the top of the Identity page, click **"Invite users"**. Invite **yourself first** using your own email so you can test before adding Frank.

   You'll get an email with a confirmation link. Click it, set a password, and you're in.

---

## Part 3 — Test it yourself (~5 minutes)

This is the moment of truth. Test the full flow before you involve Frank.

**Steps:**

1. Go to **`https://pablitos-auto-sales.netlify.app/admin/`** (or whatever your Netlify URL is).

2. Click **"Login with Netlify Identity"** → enter your email + password.

3. You should land in the Decap CMS UI. There'll be a single collection on the left: **🚗 Inventory**.

4. Click **🚗 Inventory** → click **"Cars on the Lot"** (the file collection).

5. You'll see a list of 9 cars. Each one is collapsed by default. Click any to expand.

6. **Test 1 — Edit an existing car:**
   - Open "2014 Nissan Altima"
   - Change the price from `3800` to `3700`
   - Click **"Publish"** (top-right green button)
   - Wait ~30 seconds, then refresh `https://pablitos-auto-sales.netlify.app/` — confirm the Altima now shows $3,700

7. **Test 2 — Add a new car:**
   - Back in admin, click **"Add new Cars"** (the small button at the bottom of the list)
   - Fill in: Year 2009, Make Honda, Model `Civic LX`, Miles 142000, Price 3200, Transmission Automatic, Engine 4-cyl
   - For the photo: click the upload button, upload any car photo (you can drag one in)
   - Features: leave the defaults
   - Click **"Publish"**
   - Refresh the homepage — confirm the new car appears in the grid

8. **Test 3 — Remove a car:**
   - Open the car you just added
   - Click the trash icon (top right of the field)
   - Click **"Publish"**
   - Confirm it disappears from the homepage

9. **Test 4 — Mobile:**
   - Open `/admin/` on your phone. Confirm you can log in and the editor is usable on a phone screen.

If all four tests pass, you're ready to bring Frank in.

**If something fails**, the most common issues:
- **"Failed to load config"** at `/admin/` → check `admin/config.yml` exists in the repo
- **Login works but "no collections found"** → check `admin/config.yml` has the `collections:` block
- **Photo upload fails** → check Identity → Services → Git Gateway is enabled
- **Changes don't appear on site after Publish** → wait 60 seconds for Netlify rebuild; check Deploys tab for any errors

---

## Part 4 — Domain setup (do this before training Frank)

Once Frank has paid and given you his domain preference:

1. **If he picks `pablitosautosales.com`** (still available, ~$11/year):
   - Buy it at **Cloudflare Registrar** (cheapest, no markup) or Namecheap or GoDaddy.
   - In Netlify → Site settings → Domain management → Add custom domain → `pablitosautosales.com`.
   - Follow Netlify's DNS instructions: either point the registrar's nameservers to Netlify, OR add an A record at the registrar pointing to Netlify's load balancer.
   - Wait ~10 minutes for SSL.

2. **If he already has a different domain**: same process, swap the name.

3. **Email forwarding** (`frank@pablitosautosales.com` → his real Gmail):
   - At Cloudflare Registrar, enable Email Routing — free, 5 min setup.
   - At other registrars, ImprovMX is free for 1 alias.

---

## Part 5 — Training Frank (30-minute Zoom)

When you have Frank on Zoom for his walkthrough, follow this script. Share your screen and walk him through it.

### Opening (2 min)

> "Hey Frank. This is going to take about 30 minutes. I'm going to show you how to update your website yourself — adding cars, changing prices, marking sold ones. By the end you'll have done all of it once. Then it's yours. Sound good?"

### Step 1 — Get him into the admin (3 min)

1. Send him a Netlify Identity invite email from the Netlify dashboard (Identity tab → Invite users → his email).
2. Tell him to check his inbox and click the confirmation link.
3. He sets a password.
4. He lands on `pablitosautosales.com/admin/` and sees the editor.

> "This is your dashboard. Bookmark this page — `/admin/` at the end of your URL. This is where you'll come every time you have a new car or a sale."

### Step 2 — Walk him through the layout (3 min)

> "On the left is your inventory list. Click 'Cars on the Lot'. You'll see all your current cars. Each one expands when you click it."

Click into one car. Show him the fields:
- Year, Make, Model — the basics
- Mileage — just the number, no commas
- Price — just the number in dollars
- Photo — uploads from his phone or computer
- Features — those little tags on the card
- Tag — "Available", "Sold", "Best Value", whatever
- Tag style — "Standard" for navy, "Deal" for yellow

### Step 3 — Add a car together (8 min)

Pick a real car that just came in (have him check his Instagram or text you a photo before the call):

1. Click **"Add new Cars"** at the bottom of the list.
2. Fill in each field together.
3. Have **him** click the photo upload and pick the photo from his phone.
4. Walk him through the feature tags — explain each gets a little badge.
5. He clicks **"Publish"** himself.
6. Open `pablitosautosales.com` in a new tab → refresh → there's his car.

> "That's it, Frank. You just put a car on your website. Took us about two minutes including talking. Once you've done it a couple times, it's about 60 seconds per car."

### Step 4 — Mark a sold car (3 min)

Pick a car that's actually sold:

1. Open it in the editor.
2. Change the **Tag** from "Available" to "Sold".
3. Change the **Tag style** from "Standard" to "Deal" (this makes the SOLD badge yellow so it pops).
4. Publish.
5. Refresh the site — the badge changes.

> "If you want to remove a sold car instead of marking it, just hit the trash icon and publish. Up to you — some dealers like to show recently sold to prove they move inventory."

### Step 5 — Edit a price (2 min)

Have him drop the price on something.

1. Open the car.
2. Change Price.
3. Optionally update Price Note ("cash" → "or $200/mo down").
4. Publish.

### Step 6 — Photo tips (3 min)

> "A few things I learned watching what works on other dealer sites:
> - Take photos in daylight. Cloudy days are actually best — no harsh shadows.
> - Three-quarter front view — turn the wheels slightly toward the camera.
> - Get the whole car in frame. Avoid cropping the wheels or roof.
> - Don't worry about filters. The site has its own look already.
> - Your phone is fine — you don't need a real camera."

### Step 7 — Questions + sign off (5 min)

> "What's not clear? Any features you wish were there?"

Last thing — make sure he has:
- ✅ Bookmarked `/admin/` in his phone browser
- ✅ Your number saved
- ✅ Knows that for the first 30 days, anything weird → text Jose

End the call. Send a follow-up text:

> "Thanks Frank — site's officially yours. 30 days of free support starts today. Anything comes up just text me. Good luck with the lot."

---

## Common questions Frank might ask

**"What if I publish a typo by accident?"**
Re-open the car, fix it, publish again. Takes 30 seconds.

**"What if I delete the wrong car?"**
Tell him to text you — you can restore it from Git history. (As a Git-backed CMS, nothing is ever truly deleted.)

**"Can my brother / cousin / employee help me update it?"**
Yes. In Netlify Identity → Invite users → add their email. You can have 5 users on the free tier.

**"What if I want a different photo for the same car?"**
Open the car, click the photo, click "Replace", upload the new one, publish.

**"Why does it take 30 seconds for changes to show up?"**
Netlify rebuilds the site each time. It's free and reliable but not instant. Refresh after 30 seconds.

**"Can I do this from my phone?"**
Yes — the admin works on mobile. Bookmark `/admin/` on his phone home screen for one-tap access.

---

## After Frank is up and running

You're done. Update the project doc with:
- Frank's domain (when he picks one)
- Date of launch
- Any quirks discovered during training

For ongoing support: 30 days of free email/text help (per Setup Only terms). After that, it's $50/hour ad-hoc.

If Frank ever wants you to add a feature (vehicle detail pages, a contact form, more sections), you have the repo — just clone, work, commit, and Netlify auto-deploys.
