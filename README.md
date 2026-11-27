# PR & Design module: Angels NPO Management System

**Working prototype for discussion**
For: Brent (Head of Operations and Systems) and Julianna (HOD Marketing)
Prepared by: Annemarie Mostert, QMS Administrator
Date: 27 November 2026
Status: Prototype with sample data. Not connected to Supabase, email or SharePoint yet.

**Live prototype:** `https://<github-account>.github.io/Angels_PR_Design/` (see *Publishing on GitHub Pages* below)

---

## 1. Why this module

| Today | With the module |
|---|---|
| PR requests come in as Teams chat messages. There is no form, no reference number and no record of who asked for what. | Every request starts from one form in the NPO System and gets a reference number, e.g. **PR-2026-0127**. |
| Bev has to chase people for dates, sizes, copy, logos, venues, photos and consent. | Required fields mean a brief can't be submitted half done. The HOD checks it before it reaches Bev. |
| Approvals are a thumbs-up in a chat that scrolls away. | Every decision (approve, changes requested, reject) is logged with a name, a date and the draft round. |
| Files land wherever there's space: loose files, five different archive folder names, duplicates. | Each request gets one folder, `[Ref]_[Title]`, in the right library, created when the draft is approved. |
| The CEO can't find anything. | **Find files** searches all finished work by client, programme, title or reference, with a link to the folder. |

It follows the same ticket pattern as the **Asset Management** module (tabs, a request table, a pop-up with a trail, and roles in Settings), so staff already know how it works.

---

## 2. Three kinds of request

Staff choose one when they click **+ New PR request**:

1. **Design brief.** This covers adverts, flyers, posters, recruitment forms, RFQ adverts (catering, transport, venue), newsletters and Angels collateral. It replaces the *Graphic Design Briefing Template* and *PRB-FRM-001*.
   - Every advert comes with a **header design image**.
   - **Recruitment and RFQ adverts get a QR code and short link automatically.** For recruitment it opens the Angels application form. For an RFQ it opens the RFQ Hub link, and it stops working on the closing date.
2. **Client good news story.** A story for a client's quarterly report: highlights, a quote, stats, the three monthly report summaries and numbered photos. It replaces the *Client Good News Briefing Form*. The date it's due with Bev is worked out as 5 working days before the report date.
3. **Entrepreneur story request.** This creates a link to send to two entrepreneurs, or to fill in during a phone interview. Their answers, photos and consent come straight back into the request. It replaces the *SM Feedback Request* forms.

---

## 3. The workflow

```
Submitted → HOD approval → POPIA check → In design → Draft review ⇄ Changes requested → Final art → Delivered → Closed
                ↓                ↓
          Needs info (back to requester)
```

| Stage | Who acts | What happens |
|---|---|---|
| HOD approval | HOD of the requester's department | Approve the brief, or return it for more information |
| POPIA check | QMS Administrator | Checks that consent is on file for photos and personal information, and that recruitment forms collect only what is needed |
| In design / Changes requested | Designer (Bev Mallows, B-Inspired) | Uploads a draft, with rounds 1, 2, 3 and so on |
| Draft review | Requester or HOD | Approves the draft or requests changes (a comment is required) |
| Final art | Designer | Uploads the final files (print PDF, social media, WhatsApp, header) and marks the request delivered |
| Delivered → Closed | Requester | Receives the folder link and closes the request |

Every step is added to the request's **trail**, and the next person is emailed.

---

## 4. Trying the prototype

Use **Viewing as** (top right) to switch between roles. A quick run-through:

1. **Abongile (PCO):** click *+ New PR request* → *Design brief* → set the kind to *RFQ advert*. The RFQ and QR code sections appear. Try submitting with fields empty to see the checks.
2. **HOD Training & Development:** open *My tasks* → open the request → **Approve brief**.
3. **Annemarie (QMS):** *My tasks* → **POPIA check passed**.
4. **Bev Mallows (Designer):** *Design board* → open the card → **Upload draft 1**.
5. **Abongile:** **Request changes** with a comment, or **Approve draft**. The folder is created at this point.
6. **Bev:** **Mark delivered**. Then, as **Lizelle (CEO)**: *Find files* → search "Scatec".

*Settings → Reset demo data* puts the sample requests back. Data is kept only in your own browser.

---

## 5. Decisions for Brent and Julianna

1. **Who owns the module day to day?** Is HOD Marketing the overall owner? Does Marketing approve all public-facing work, or only work from departments with no HOD?
2. **Who approves briefs** for each department, and does Marketing approve again at draft review?
3. **Bev's access:** she is external (B-Inspired). She needs a login that sees PR requests only, and Angels needs a **POPIA operator agreement** (s21) with B-Inspired.
4. **Where final files live:** keep them in the Teams/SharePoint libraries with each request linking to its folder (recommended), or store them in Supabase.
5. **Folder routing:** Angels has no *Campaigns* folder (it has *Collateral* and *Events*). Should a client request go to `Clients / [Code] / [Programme] / [Ref]_[Title]`, as Seriti G is organised by programme?
6. **Client codes:** build the dropdown from the *Clients Legend* tab, and mark old codes Inactive.
7. **Short link and QR domain:** which domain will the QR codes use?
8. **Cut-over date:** from which date are chat requests no longer accepted ("no brief, no work")?
9. **Clean-up of the existing libraries:** one archive folder name, and no loose files at library roots.

---

## 6. POPIA rules built in

- **Consent:** photos of identifiable people can't go to design until consent (who obtained it, the date and what it covers) is recorded.
- **Recruitment applications:** these are stored in the Angels system only, never Google Forms. Unsuccessful applications are deleted or de-identified **12 months after selection** (s14). Counts are kept for reports without names. *Period to be confirmed by the Information Officer.*
- **Marketing consent:** recruitment and story forms carry a separate, **unticked** "Send me Angels news" box. Only people who tick it receive newsletters, email or WhatsApp news (s69 and the Information Regulator's direct-marketing guidance).
- **Withdrawing consent:** entrepreneurs are told they can withdraw it at any time by emailing pr@angelsinc.co.za.

---

## 7. Build notes for the NPO System

**Suggested tables (Supabase)**

| Table | Key fields |
|---|---|
| `pr_requests` | ref, type, kind, title, client_code, programme, requester_id, department, needed_by, draft_by, status, round, formats[], header_image, qr_url, qr_expires, photos_of_people, consent_by, consent_date, consent_scope[], folder_path, created_at |
| `pr_trail` | request_ref, at, by_user, action, note (append-only) |
| `pr_comments` | request_ref, at, by_user, text |
| `pr_story_responses` | request_ref, token, name, contact, business, answers, photo_links, consent_publish, consent_marketing, submitted_at |
| `clients` | client_code, client_name, active |
| `pr_settings` | designer_lead_days (5), review_reminder_days (2), applicant_retention_months (12), close_after_days (14) |

**Notifications (Resend):** HOD on submission; QMS on approval; designer after the POPIA check; requester on each draft; designer on changes requested; requester on delivery; a reminder after 2 working days at draft review.

**Folder routing:** a client is set → `Clients / [Client] / [Ref]_[Title]`. Otherwise the kind decides: CSI → `Angels CSI / [School]`, events → `Angels / Events`, newsletter → `Newsletters`, annual report → `Annual Report (AGM)`, brand assets → `Angels / CI Documents`, everything else → `Angels / Campaigns`. **See decision 5.**

**Lead time:** a draft date less than 5 working days away is flagged to PR, not blocked.

---

## 8. Publishing on GitHub Pages

This follows the same setup as Angels PERSAL:

1. Create a new repository named **Angels_PR_Design** in the same GitHub account.
2. Upload `index.html`, `README.md` and `.nojekyll`. (`.nojekyll` is a hidden file. If the upload skips it, the site still works.)
3. Go to **Settings → Pages**, set Source to *Deploy from a branch*, choose *main* and */ (root)*, and click **Save**.
4. After about a minute, the prototype is at `https://<github-account>.github.io/Angels_PR_Design/`.

The prototype is one self-contained HTML file. The only outside resource is the Inter font from Google Fonts.

## Files

| File | What it is |
|---|---|
| `index.html` | The working prototype |
| `README.md` | This document |
| `.nojekyll` | Tells GitHub Pages to serve the files as they are |
