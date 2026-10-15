# NRC Social — Switchable AI (Vercel)

This edition keeps all previous feeds, reply threads, group chats, DMs, character profiles, images and scripted NPCs. The **Customize → AI & API Settings** panel now lets you select an AI provider, edit a model ID, and change which API key is used.

## Deploy or upgrade on Vercel using iPhone

1. Unzip this folder and upload `index.html`, `package.json`, and the complete `api/reply.js` to the **root** of your GitHub repository. Do not upload only the HTML file; preserve the `api` folder.
2. In [Vercel](https://vercel.com/dashboard), make sure the GitHub repository is imported as a project, with **Root Directory** at the repository root. Redeploy from the latest commit. The serverless function is available at `https://YOUR-PROJECT.vercel.app/api/reply`.
3. Under **Project → Settings → Environment Variables**, set `NRC_ACCESS_CODE` to a long private passcode. Set at least one provider secret:
   - `OPENROUTER_API_KEY` for OpenRouter
   - `GEMINI_API_KEY` for Google Gemini
   - `CUSTOM_API_KEY` and `CUSTOM_AI_URL` for a custom OpenAI-compatible **HTTPS chat/completions endpoint** (e.g. a third-party service). Do not put your private key inside the URL.
   - Optional `NRC_SITE_URL` = your Vercel website URL (for OpenRouter attribution).
4. Redeploy **after changing Vercel environment variables**. Never commit `.env` files or keys to GitHub.
5. Open your **Vercel website**, choose **Customize → AI & API Settings**, select the provider and type its model ID, enter the NRC access code, and tap **Save & Connect AI**. Then tap **Test AI reply** to test the actual model.

### Models you can choose

- OpenRouter: default `nvidia/nemotron-3-ultra-550b-a55b:free`; substitute **any current OpenRouter model slug**, e.g. `google/gemma-3-27b-it:free` (availability and quotas change; verify slug at https://openrouter.ai/models).
- Gemini: default `gemini-2.5-flash`; enter another valid Gemini model ID.
- Custom: enter the model ID accepted by the API configured in `CUSTOM_AI_URL`. Uses an OpenAI-compatible message format, even when the AI service is not OpenAI.

**Custom endpoints are configured on Vercel, not from a public form**, to prevent this backend from being used to fetch arbitrary URLs.

### Change API keys *without a redeploy* (optional)

Choose **Enter an API key for this tab only** under **API key source**, paste the key, enter your NRC access code, and press **Save & Connect AI**. The temporary key remains only in JavaScript memory (not in localStorage, a cookie, or a GitHub file). It is transmitted over HTTPS to your own Vercel `/api/reply` function and sent on to the selected provider. It is lost on refresh/tab close, so you must enter it again. This is less secure than a Vercel environment variable: use this only on your own device and trusted personal deployment, not a public shared web app.

You can select **Use scripted NPCs** to stop AI and wipe the in-memory temporary key.

### If the AI fails

- **“No ... API key configured”**: add the correct key to Vercel settings, redeploy, or choose the temporary key mode.
- **“Wrong NRC access code”**: re-enter `NRC_ACCESS_CODE` (not the provider key).
- **HTTP 404 or HTML returned from `/api/reply`**: upload `api/reply.js` and redeploy; verify the project Root Directory. The URL `.../api/reply` must show a JSON response (unauthorized if no code is provided), not your social website HTML.
- **Rate limit / model unavailable**: use a different model ID; free models have quotas or outages.
- **Wrong string pattern on Safari**: access the HTTPS Vercel website rather than a local HTML file; this version validates the backend endpoint and shows clearer errors.

**Note:** Profile images/posts are displayed in the app, but this edition does not send image pixels to AI providers for interpretation. Automatic NPC posts and proactive DMs remain scripted. Your saved posts and messages stay in your browser's storage, not an online database. API usage costs/limits depend on the provider and model. Fictional school-age characters are not configured for explicit sexual content.

## Optional terminal syntax check

```bash
node --check api/reply.js
```
