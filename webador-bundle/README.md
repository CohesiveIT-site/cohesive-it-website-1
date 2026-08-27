# Deploying the Cohesive IT site to Webador

Webador doesn't support uploading a standalone `index.html` and asset files the way a
normal web host does. It's a drag-and-drop builder: the only way to get custom-coded
HTML/CSS/JS onto a page is a site-wide "Head" custom code field plus an "Embed Code"
element you drop onto a page. This folder has the site split into those two pieces,
with both images already embedded directly in the code (as base64) so there's nothing
separate to upload.

## Before you start: check the page template

This is the one thing I can't verify without access to your Webador account. Webador
pages normally come with the theme's own header, navigation, and footer built in. This
site brings its own header/nav and footer, so if Webador adds its own on top, you'll
likely end up with **two navigation bars and two footers** stacked on the page.

Look for a "blank" or "custom" page template option in the page settings, or a way to
hide the site header/footer on a specific page, before pasting anything in. If Webador
genuinely can't do this on your plan, let me know and I can prepare a version that
drops the custom header/footer and only shows the boxed sections in between (or we
look at hosting it outside Webador instead, which guarantees a pixel-perfect result).

## Step 1: Add the Head code

1. In the Webador editor, go to **Settings > Advanced**.
2. Click **Add custom HTML code**, then **+ Add HTML code**.
3. Give it a title (e.g. "Cohesive site styles/fonts") and set the placement to **Head**.
4. Paste in the entire contents of `1-head-code.html` from this folder.

This piece loads the two Google Fonts (Space Grotesk, IBM Plex Sans/Mono), the
Tailwind CSS engine, the site's custom color palette, and all the animation/interaction
CSS the page needs.

Webador manages your page title and meta description separately in its own page
settings, that's not included here, so set those through Webador's normal SEO fields
instead.

## Step 2: Add the page content

1. On the page you want this to be (e.g. your homepage), drag an **Embed Code**
   element onto the page. Ideally it should be the only element on the page, so it
   isn't sandwiched between Webador's own header and footer, see the note above.
2. Click the element, choose **Edit embed**, and paste in the entire contents of
   `2-body-embed.html`.
3. Save and publish, then view the page live (Webador only shows "HTML code" as a
   placeholder in the editor preview, you need to check the published page to see it
   render).

This piece is everything visible on the page (navigation, hero, all sections, footer)
plus the small script at the end that runs the mobile menu, the scroll-based nav
background, and the contact form's submit handling.

## Step 3: Favicon

Webador has its own favicon upload setting (separate from custom code). Upload
`../assets/cohesive-icon.png` there rather than relying on anything in the pasted code.

## If the mobile menu or contact form don't respond after publishing

Some embed widgets strip `<script>` tags for security. If that happens here, move just
the `<script>...</script>` block at the very end of `2-body-embed.html` into
**Settings > Advanced > Add custom HTML code**, placed at **Body - end**, instead of
leaving it inside the embed element.

## Step 4: Connect the contact form to email (website@cohesiveit.co.nz)

The form is wired to send via [EmailJS](https://www.emailjs.com) (the same service the
Lowburn Terraces site uses), but it needs three values from a real EmailJS account
before it'll actually deliver anything, right now `2-body-embed.html` has placeholder
text in their place. I can't create this account myself, connecting it to your inbox
needs you to log into it, so this part needs you:

1. Go to [emailjs.com](https://www.emailjs.com) and sign up (free tier covers 200
   emails/month, plenty for a contact form).
2. **Add an Email Service**: connect `website@cohesiveit.co.nz` (Gmail, Outlook, or
   any SMTP provider all work). This is what EmailJS sends *from* and *through*.
   Note the **Service ID** it gives you.
3. **Create an Email Template** with a destination address of
   `website@cohesiveit.co.nz`, and in the template body use these three variables
   (they match the form's field names exactly, don't rename them):
   `{{from_name}}`, `{{from_email}}`, `{{message}}`.
   Note the **Template ID**.
4. Go to **Account > General** in EmailJS and copy your **Public Key**.
5. In `2-body-embed.html` (or directly in `index.html` if you're working from the
   full site), find and replace all three placeholders:
   - `REPLACE_WITH_EMAILJS_PUBLIC_KEY` → your Public Key
   - `REPLACE_WITH_EMAILJS_SERVICE_ID` → your Service ID
   - `REPLACE_WITH_EMAILJS_TEMPLATE_ID` → your Template ID
6. Re-paste the updated code into the Webador embed element and republish.

Until these are filled in, the form still works from a visitor's point of view
(validation, a loading state), but submitting it will show "Sorry, something went
wrong sending your message. Please email website@cohesiveit.co.nz directly." instead
of succeeding, tested and confirmed that's the actual behavior with the placeholders
still in place, not a guess.

If you'd rather send me the three values directly, I can drop them in myself and
regenerate this bundle.
