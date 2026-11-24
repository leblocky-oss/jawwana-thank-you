# Jawwana thank-you cards: deploy notes

Two interactive thank-you pages (slide to open → envelope → card flips). Static files only: no build step, no backend, no cookies, no tracking.

| Folder | Who it's for | Card |
|---|---|---|
| `circle/` | Beta founders who **joined** the founding circle | "You're one of us now! / صرت واحد مننا!" |
| `thanks/` | Beta founders who **didn't join** | "Thanks for giving us a shot! / شكراً إنك عطيتنا فرصة!" |

Root `index.html` and `404.html` show a plain Jawwana logo and an Instagram link, in case someone opens the bare domain.

## 1. Pick the address

Anything works. For example:

- `https://thanks.jawwana.sa/circle/` and `https://thanks.jawwana.sa/thanks/`, or
- `https://jawwana.sa/thanks/circle/` and `https://jawwana.sa/thanks/thanks/`

All paths inside the pages are relative, so the folder can live at the root or in any subfolder.

## 2. One find-and-replace (for WhatsApp link previews)

WhatsApp, iMessage, LinkedIn and X need a **full URL** for the preview image. In `circle/index.html` and `thanks/index.html`, replace:

```
__SITE_URL__
```

with the address where this folder lives, **with no trailing slash**. Examples: `https://thanks.jawwana.sa` or `https://jawwana.sa/thanks`.

(It appears 2 times per file. If you skip this step the pages still work; only the link preview image won't show.)

## 3. Deploy: pick whichever you already use

**Cloudflare Pages / Netlify (drag and drop)**
Upload the whole `jawwana-thank-you` folder. `_headers` is picked up automatically. Add the custom domain in the dashboard.

**Vercel**
Drag the folder into a new project, or run `vercel --prod` inside it. `vercel.json` is included.

**Firebase Hosting**
Inside the folder: `firebase init hosting`, choose the existing project, answer **No** to overwriting `firebase.json`, then `firebase deploy --only hosting`. For a subdomain, add a separate site with `firebase hosting:sites:create` and connect the domain in the console.

**Existing website / any server (Nginx, Apache, cPanel)**
Copy the folder's contents into the public web directory, at the root or in a subfolder. Nothing else is needed.

## 4. Check before sending

- [ ] Both pages open on an **iPhone (Safari)** and an **Android phone (Chrome)**
- [ ] Slide to open: the flap follows your finger, opens by itself at about 78%, the card comes out, rests, then flips
- [ ] The Instagram button opens @jawwana.sa
- [ ] Send each link to yourself on WhatsApp: the preview shows the purple card image
- [ ] HTTPS is on (all the hosts above do this automatically)

## Notes

- **Not indexed:** pages carry `noindex` (meta tag plus header), so they won't appear in Google.
- **Privacy (PDPL):** no forms, cookies, analytics or personal data. The only outside request is Google Fonts (Montserrat, Almarai).
- **Adding TikTok / LinkedIn later:** in each `index.html`, find `const SOCIALS` and paste the profile URL into the empty `url:""`. The button appears automatically.
- **Accessibility:** if a phone has "Reduce motion" on, the animation is skipped and the card shows directly.
- **Size:** about 300 KB per page, with images built in (no extra requests).
