# Custom Domain Troubleshooting: postcardarchive.online

Your repo already has a **CNAME** file with `postcardarchive.online`. Use this checklist to get the custom domain working.

---

## Fix: "Both postcardarchive.online and its alternate name are improperly configured" (NotServedByPagesError)

GitHub checks **two** hostnames. Both must resolve to GitHub’s servers:

| Hostname | What GitHub expects |
|----------|----------------------|
| **postcardarchive.online** (apex) | A records → GitHub’s 4 IPs (below) |
| **www.postcardarchive.online** (alternate) | CNAME → `YOUR_GITHUB_USERNAME.github.io` |

**You must configure both** at your DNS provider. If only one is set (or values are wrong), the domain check will fail.

### Step A – Apex (postcardarchive.online)

At your domain registrar’s DNS panel, add **four A records** for the root/apex:

| Type | Name/Host | Value | TTL |
|------|-----------|--------|-----|
| A | `@` (or blank) | `185.199.108.153` | 3600 |
| A | `@` | `185.199.109.153` | 3600 |
| A | `@` | `185.199.110.153` | 3600 |
| A | `@` | `185.199.111.153` | 3600 |

- Remove any **other** A records for `@` that point to different IPs (e.g. parking page, old host).
- Some providers use “@”, others “blank” or the full domain for “root” — use what yours uses.

### Step B – www (www.postcardarchive.online)

Add **one CNAME record** for the www subdomain:

| Type | Name/Host | Value/Points to |
|------|-----------|------------------|
| CNAME | `www` | `snails91.github.io` |

- Target must be exactly **snails91.github.io** (no path, no trailing dot unless your DNS provider requires it).
- If you already have a CNAME for `www` pointing somewhere else (e.g. to the apex, or a typo), change it to **snails91.github.io**.

### Step C – Save and wait

1. Save DNS at your registrar.
2. Wait **5–60 minutes** (sometimes up to 24 hours) for propagation.
3. In the repo go to **Settings → Pages**, leave the custom domain as `postcardarchive.online`, and click **Save** (or run the domain check again).

Once both apex and www resolve correctly, the “Both … and its alternate name are improperly configured” / NotServedByPagesError should clear.

---

## 1. GitHub repository settings

1. Open your repo on GitHub: **shoebox**
2. Go to **Settings** → **Pages** (left sidebar).
3. Under **Custom domain**, type: `postcardarchive.online`
4. Click **Save**.
5. If GitHub shows a **DNS check** or **Verify** button, run it. Fix any errors it reports.
6. After DNS is correct, enable **Enforce HTTPS** (recommended).

**Source:** Confirm Pages is published from the correct branch (e.g. `main`) and folder (e.g. **/ (root)**). Your site files (`index.html`, etc.) are in the repo root, so source should be root.

---

## 2. DNS at your domain registrar

You’re using the **apex** domain (`postcardarchive.online`, no `www`). Configure DNS at the place where you bought **postcardarchive.online** (e.g. Namecheap, GoDaddy, Google Domains, Cloudflare).

### For apex: postcardarchive.online

Add these **four A records** (replace any existing A records for `@` if your host only allows one “@” and multiple IPs, add all four IPs if possible):

| Type | Name/Host | Value/Points to | TTL  |
|------|-----------|-----------------|------|
| A    | `@`       | `185.199.108.153` | 3600 |
| A    | `@`       | `185.199.109.153` | 3600 |
| A    | `@`       | `185.199.110.153` | 3600 |
| A    | `@`       | `185.199.111.153` | 3600 |

Some registrars use **“@”** for the root domain, others use **blank** or **“postcardarchive.online”**. Use whatever your DNS panel uses for the root.

### www (required for GitHub’s domain check)

GitHub verifies **both** the apex and **www.postcardarchive.online**. To pass the check, you must add:

| Type  | Name/Host | Value/Points to        |
|-------|-----------|-------------------------|
| CNAME | `www`     | `snails91.github.io` |

Replace with your GitHub username if different; for this repo the correct target is **snails91.github.io**.  
If this is a **project** site (e.g. `username.github.io/shoebox`), the CNAME target is still: `USERNAME.github.io` (no path).

---

## 3. CNAME file in the repo (already done)

- Your **CNAME** file contains only: `postcardarchive.online`  
- No `https://`, no `www`, no trailing slash.  
- This is correct for the apex domain.

---

## 4. Common problems

| Symptom | What to check |
|--------|----------------|
| “Domain’s DNS record is incorrect” in GitHub | A records for `@` must point exactly to the four IPs above. Wait for DNS to propagate (see below). |
| Site works at `username.github.io` but not at postcardarchive.online | DNS not set or not propagated. Double‑check A records at your registrar. |
| 404 on custom domain | In **Settings → Pages**, confirm **source** is the right branch and folder (e.g. main / root). |
| “Not yet available” or certificate errors | Turn on **Enforce HTTPS** only after the domain check in GitHub passes. DNS can take up to 24–48 hours. |
| CNAME conflict | If you use a CNAME for `www`, the **apex** must use **A** records only. Do not use a CNAME for the apex (`@`). |

---

## 5. DNS propagation

After changing DNS:

- Propagation often takes **5–60 minutes**, sometimes up to **24–48 hours**.
- Check status:
  - [https://dnschecker.org](https://dnschecker.org) — enter `postcardarchive.online`, type **A**, and confirm the four GitHub IPs appear globally.
  - Or run: `nslookup postcardarchive.online` (or `dig postcardarchive.online`) and confirm you see the same IPs.

---

## 6. Quick checklist

- [ ] **Settings → Pages**: Custom domain set to `postcardarchive.online`, source = correct branch + root.
- [ ] **DNS**: Four A records for `@` → `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`.
- [ ] **CNAME file**: Contains only `postcardarchive.online` (already correct).
- [ ] Waited for DNS propagation and re-ran **Verify** in GitHub Pages.
- [ ] **Enforce HTTPS** enabled after verification succeeds.

If you tell me your registrar (e.g. Namecheap, Cloudflare) and what you see in **Settings → Pages** (e.g. error message or “DNS check successful”), I can give step‑by‑step for that screen and DNS panel.
