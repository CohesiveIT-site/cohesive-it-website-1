# Deploying the Cohesive IT site to Webador

Webador doesn't support uploading a standalone `index.html` and asset files the way a
normal web host does. It's a drag-and-drop builder: the only way to get custom-coded
HTML/CSS/JS onto a page is a site-wide "Head" custom code field plus Embed Code
elements you drop onto a page. This folder has the site split into the pieces that
model requires, with both images already embedded directly in the code (as base64)
so there's nothing separate to upload.

The contact form uses **Webador's own native form widget** (not custom code), so the
site is split into three pieces instead of two, with a gap in the middle where that
widget goes.

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

## Step 2: Add the first content block

1. On the page you want this to be (e.g. your homepage), drag an **Embed Code**
   element onto the page.
2. Click the element, choose **Edit embed**, and paste in the entire contents of
   `2-body-embed-part1.html`.

This covers everything from the navigation down through the "Let's take IT off your
plate" contact heading, location, and hours, everything except the form itself.

## Step 3: Add Webador's native form widget

Directly below the block from Step 2 (and above the footer in Step 4), add Webador's
own **Form** widget:

1. Drag the **Form** (or **Contact Form**) element onto the page, positioned right
   after the embed block from Step 2.
2. Click the gear/settings icon on the form and set the destination address to
   `website@cohesiveit.co.nz`.
3. Turn on the **Robot check (Captcha)** option to cut down on spam.
4. Style the fields/button using Webador's own form styling options. It won't be a
   pixel-perfect match to the rest of the site (Webador's form styling is more
   limited than custom CSS), but keep it close: dark background, light text, rounded
   button, to sit naturally between the two green sections around it.
5. Add `no-reply@webador.com` to your email address book/allow-list, Webador's own
   help docs note submissions can otherwise get caught by spam filters.

## Step 4: Add the footer

1. Drag another **Embed Code** element onto the page, directly below the form widget.
2. Paste in the entire contents of `3-body-embed-part2-footer.html`.
3. Save and publish, then view the page live (Webador only shows "HTML code" as a
   placeholder in the editor preview, you need to check the published page to see it
   render).

## Step 5: Favicon

Webador has its own favicon upload setting (separate from custom code). Upload
`../assets/cohesive-icon.png` there rather than relying on anything in the pasted code.

## If the mobile menu doesn't respond after publishing

Some embed widgets strip `<script>` tags for security. If that happens here, move just
the `<script>...</script>` block at the very end of `2-body-embed-part1.html` into
**Settings > Advanced > Add custom HTML code**, placed at **Body - end**, instead of
leaving it inside the embed element. That script only handles the mobile menu toggle
and the nav's solid-background-on-scroll effect, nothing else depends on it now that
the contact form is Webador's own widget.
