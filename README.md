# Icons8 MCP server

Icons8 MCP Server gives AI coding environments instant access to 420,000+ Icons8 [icons](https://icons8.com/icons?utm_source=github) across 132 design styles. Connect your Claude, Cursor, Windsurf, VS Code, or any MCP host, sign in once, and stream high-res PNGs for free or unlock full SVG delivery with a paid plan—ideal for prototyping, production code, and rapid UI experiments.

### Use cases
- Replace entire icon sets in existing projects with consistent, on-trend styles.
- Add icons to bullet lists, feature highlights, process steps, and dashboards without leaving your IDE.
- Prototype quickly with free PNG previews, then upgrade to SVGs for production-ready assets.
- Build AI-assisted workflows that search, preview, and drop icons directly into your code.
- Showcase interactivity with ready-made demos such as falling emojis, sci-fi dashboards, and product catalogs.

---

## How it works

The Icons8 MCP endpoint is hosted at `https://mcp.icons8.com/mcp/` and speaks the MCP Streamable HTTP
transport. Your client connects to it directly—there is no local process to install and nothing to
run alongside your editor.

### Prerequisites
1. An MCP-compatible editor or agent host (Claude Code, Cursor, Windsurf, VS Code, etc.).
2. An Icons8 account. The server authenticates with OAuth, so the first connection opens a browser
   and asks you to sign in.

### Step 1: connect

One configuration covers everything. Point your client at the endpoint:

```json
{
  "mcpServers": {
    "icons8mcp": {
      "type": "http",
      "url": "https://mcp.icons8.com/mcp/"
    }
  }
}
```

Sign in when the browser opens, and search plus high-res PNG work from there. Four tools appear:
`search_icons`, `list_categories`, `list_platforms` and `get_icon_png_url`.

### Step 2: unlock SVG when you need it

SVG is the production format, and it comes with a paid plan. [Subscribe for $15](https://icons8.com/icons/pricing)
and the account you already signed in with carries the plan—there is no second server to add and no
second configuration to swap in. A fifth tool, `get_icon_svg`, joins the other four.

If your agent still hands back PNG, sign in again so it picks up the new plan: `/mcp` in Claude Code,
`codex mcp login icons8mcp` in Codex, **Connect** in Cursor.

#### A client without OAuth

Send an API key instead. The MCP tab of your Icons8 account has the snippet with your key already in
it, or add the header by hand:

```json
{
  "mcpServers": {
    "icons8mcp": {
      "type": "http",
      "url": "https://mcp.icons8.com/mcp/",
      "headers": {
        "Authorization": "Bearer YOUR_API_KEY"
      }
    }
  }
}
```

A key skips the browser entirely, which is also what you want on a build server or in CI, where
nobody is around to sign in. Set this up with a key before? Leave it alone—it still works.

### Configure your client

**Claude Code**
1. Open a terminal and run:
   ```bash
   claude mcp add --transport http icons8mcp https://mcp.icons8.com/mcp/
   ```
2. Verify the server is registered, and sign in with `/mcp`:
   ```bash
   claude mcp list
   ```
   Before you sign in the status reads `Needs authentication`—that means the endpoint answered, not
   that the setup is wrong.

**Codex**
1. Add the server and sign in:
   ```bash
   codex mcp add icons8mcp --url https://mcp.icons8.com/mcp/
   codex mcp login icons8mcp
   ```

**Cursor**
1. Cursor → Settings → Cursor Settings → MCP tab.
2. Click **+ Add new global MCP server**.
3. Paste the JSON from Step 1, then click **Connect** to sign in.

**Windsurf**
1. Windsurf → Settings → Windsurf Settings → MCP section.
2. Manage plugins → **View raw config**.
3. Paste the JSON configuration and save.

**VS Code (agent mode)**
1. Code → Settings → Settings → search for "MCP".
2. Choose **Edit in settings.json**.
3. Add the JSON configuration.
4. Open the chat toolbar (`⌥⌘B`), switch to Agent mode, and look for tools under *MCP Server: Icons8 MCP*. Restart VS Code if tools do not appear immediately.

**Any other MCP host**
Use the same JSON from Step 1. Where the host supports OAuth it will walk you through signing in; if
it does not, add the `Authorization` header shown above. Consult your host documentation for where to
paste MCP server settings.

Hosts label this transport differently—`http`, `streamable-http`, or, in clients that predate the
rename, `SSE`. They all mean the same remote endpoint over HTTP, so pick whichever of those your host
offers and give it the URL above.

### Start building with icons
Ask your AI assistant for the icons you need, e.g. “create a dashboard with analytics SVG icons in color style” or “add MCP server icon SVG in iOS glyph style.” Explore the [live demos](#live-demos) below for inspiration, and review the [FAQ](#faq) if something doesn’t work as expected.

---

## Live demos
- [Falling Emojis Animation](https://goodies.icons8.com/landings/mcp/static/demo/1-falling-emojis.html)
- [Solar System in Tufte Style](https://goodies.icons8.com/landings/mcp/static/demo/2-solar-system-tufte.html)
- [Sci-Fi Control Interface](https://goodies.icons8.com/landings/mcp/static/demo/3-sci-fi-interface.html)
- [Product Catalog Showcase](https://goodies.icons8.com/landings/mcp/static/demo/4-product-catalog.html)

---

## FAQ

**My client says the server needs authentication. Is something broken?**  
No—that is the normal state before you sign in. The endpoint answered; it is waiting for you. Sign in
with `/mcp` in Claude Code, `codex mcp login icons8mcp` in Codex, or **Connect** in Cursor.

**How do I get SVG icons specifically?**  
Include an explicit request for SVG icons in your prompt, and make sure the connection carries a paid
plan—either the account you signed in with, or the API key in the `Authorization` header.

**What formats are available?**  
PNG works on every plan. SVG downloads require a paid plan.

**Can I download PNG icons for free?**  
Yes. High-res PNG is free once you have signed in; no paid plan and no API key needed.

**What are the license terms for the free plan?**  
Free PNG usage requires attribution with a link back to Icons8. Review the [Icons8 license](https://icons8.com/license) for full details.

**Do I need an API key?**  
Only where your client cannot do OAuth, or on a build server where nobody is around to sign in.
Everywhere else, signing in replaces the key—and it is the account, not the key, that carries your
plan.

**I requested SVG but received PNG. What happened?**  
`get_icon_svg` appears only when the connection is on a paid plan, so the agent fell back to PNG. If
you subscribed after connecting, sign in again to pick up the new plan, then ask with an explicit SVG
request.

**Do I still need Node.js or `npx mcp-remote`?**  
No. The server speaks Streamable HTTP, so your client connects to it directly. If you have an older
`npx mcp-remote` configuration it keeps working, but you can replace it with the plain URL above.

**Can I download icons in batches?**  
Batch downloads are in progress—stay tuned.

**Need help?**  
Check the setup steps above or [chat with us](https://icons8.com/contact).

---

## More from Icons8

Icons are just the start — Icons8 also has **[110,000+ illustrations](https://icons8.com/illustrations?utm_source=github)** in 346 styles ([3D Casual Life](https://icons8.com/illustrations/styles/3d-casual-life?utm_source=github), [Cherry](https://icons8.com/illustrations/styles/cherry?utm_source=github), [3D Stickle](https://icons8.com/illustrations/styles/3d-stickle?utm_source=github)) and **[Mega Creator](https://icons8.com/mega-creator?utm_source=github)** for full graphics.

## Credits
- [Product Hunt “Product of the Day” launch recognition](https://www.producthunt.com/products/icons8?launch=icons8-mcp-server).
- © 2025 Icons8 — terms, privacy, and support links are available on [icons8.com](https://icons8.com).


