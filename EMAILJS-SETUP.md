# Contact Form — EmailJS Setup (5 minutes)

Your contact form is fully coded and production-ready. It only needs **your three EmailJS values**. No backend, no server, no cost for normal volumes.

---

## 1. Create an EmailJS account
Go to **https://www.emailjs.com** → **Sign Up** (free). Verify your email and log in.

## 2. Connect your Gmail account (Email Service)
1. In the dashboard, open **Email Services** → **Add New Service**.
2. Choose **Gmail**.
3. Click **Connect Account** and sign in with the Gmail address where you want to receive messages (`mustaisim.ecommerce@gmail.com`). Approve the permissions.
4. Give it any name and click **Create Service**.
5. **Copy the Service ID** shown for this service (looks like `service_xxxxxxx`).

## 3. Create an Email Template
1. Open **Email Templates** → **Create New Template**.
2. Set these fields:
   - **To Email:** `mustaisim.ecommerce@gmail.com`
   - **From Name:** `{{user_name}}`
   - **Reply To:** `{{user_email}}`  ← important, so you can reply straight to the sender
   - **Subject:** `New enquiry: {{project_type}} — from {{user_name}}`
   - **Content (body):**
     ```
     You received a new message from your portfolio website.

     Name:    {{user_name}}
     Email:   {{user_email}}
     Project: {{project_type}}

     Message:
     {{message}}
     ```
3. Click **Save**, then **Copy the Template ID** (looks like `template_xxxxxxx`).

> The variables `{{user_name}}`, `{{user_email}}`, `{{project_type}}`, `{{message}}` match your form's field names exactly — don't rename them. (If your existing template still uses `{{name}}`, `{{email}}`, or `{{subject}}`, update it to these four names, or the Email and Project Type fields will arrive blank.)

## 4. Get your Public Key
Open **Account** → **General** → copy the **Public Key** (looks like a short random string).

## 5. You now have three values
| Value | Where it came from | Example |
|---|---|---|
| **Public Key** | Account ▸ General | `A1b2C3d4E5...` |
| **Service ID** | Email Services | `service_ab12cd3` |
| **Template ID** | Email Templates | `template_xy98zw7` |

## 6. Paste them into your project
Open **`index.html`**, and near the contact-form script find this block (search for `EMAILJS_PUBLIC_KEY`):

```js
const EMAILJS_PUBLIC_KEY  = 'YOUR_PUBLIC_KEY';
const EMAILJS_SERVICE_ID  = 'YOUR_SERVICE_ID';
const EMAILJS_TEMPLATE_ID = 'YOUR_TEMPLATE_ID';
```

Replace the three placeholders with your real values, keeping the quotes:

```js
const EMAILJS_PUBLIC_KEY  = 'A1b2C3d4E5...';
const EMAILJS_SERVICE_ID  = 'service_ab12cd3';
const EMAILJS_TEMPLATE_ID = 'template_xy98zw7';
```

Save. That's the only change you make.

## 7. Test it locally
1. Open `index.html` in your browser (double-click, or use a local server).
2. Scroll to **Contact**, fill in the form, and click **Send Message**.
3. You should see the button switch to **Sending…**, then the **Thank you!** success panel, and an email should arrive in your Gmail within a few seconds (check Spam the first time).
4. Try submitting empty fields — you'll see inline validation. Try an invalid email — it's rejected. These confirm the safeguards work.

> Until you paste your keys, the form shows a friendly "not connected yet" note instead of sending — that's expected.

## 8. Verify after deploying to GitHub + Vercel
- **GitHub Pages / Vercel:** just push the project (with your keys saved in `index.html`) and deploy. EmailJS runs entirely in the visitor's browser, so it works the same as local — **no environment variables needed.**
- After deploying, open your live URL, send a test message, and confirm it lands in Gmail.
- **Security note:** the EmailJS Public Key is *designed* to be public and safe to commit. For extra safety, in **EmailJS ▸ Account ▸ Security** you can enable **"Allowed Origins"** and add your live domain (e.g. `https://yourname.vercel.app`) so only your site can use the key.

---

### Built-in protections (already active)
- **Required-field + email-format validation** before anything is sent.
- **Honeypot** hidden field silently blocks bots.
- **Rate limiting** (20s between sends) + **duplicate-submission guard** while sending.
- **Loading state**, **success** panel, and **friendly error** message with your email as a fallback.
- Accessible: labelled fields, `aria-invalid` on errors, `aria-busy` while sending, keyboard-friendly, focus preserved.
