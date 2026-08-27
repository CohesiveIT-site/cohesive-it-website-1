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

## The contact form doesn't send anywhere yet

The form has a working UI (validation, a "message sent" confirmation) but isn't wired
to actually deliver email yet, there's no email service connected. The Lowburn
Terraces project uses EmailJS for this; the same approach would work here once you
have (or want) an EmailJS account, or Webador may have its own built-in form/email
handling that could replace this form entirely, worth checking before wiring up a
third-party service.
