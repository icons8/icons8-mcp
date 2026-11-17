# Icons8 MCP server

Icons8 MCP Server gives AI coding environments access to the [Icons8](https://icons8.com/icons?utm_source=github&utm_campaign=mcp) icon catalog (420,000+ icons in 130+ design styles) and to [Ouch illustrations](https://icons8.com/illustrations?utm_source=github&utm_campaign=mcp) (110,000+ illustrations in 340+ styles). Connect Claude, Cursor, Windsurf, VS Code, Codex or any MCP host, sign in once, and search the catalogs, preview artwork for free, or download SVG, Lottie and animated formats with a paid plan.

The server is listed in the official MCP Registry as `com.icons8/icons8`.

### Use cases
- Replace an icon set in an existing project with one consistent style.
- Add icons to bullet lists, feature highlights, process steps and dashboards without leaving your IDE.
- Add illustrations to hero sections, empty states and onboarding screens.
- Embed animated icons (GIF, APNG, Lottie) and animated illustrations (WebM, HEVC MP4, Lottie).
- Prototype with free PNG previews, then switch to SVG for production assets.

---

## How it works

The endpoint is hosted at `https://mcp.icons8.com/mcp/` and speaks the MCP Streamable HTTP
transport. Your client connects to it directly. There is no local process to install.

### Prerequisites
1. An MCP-compatible editor or agent host (Claude Code, Claude Desktop, Cursor, Windsurf, VS Code, Codex and others).
2. An Icons8 account. The first connection opens a browser and asks you to sign in. You can sign in with email and password, or with Google, Apple, Microsoft or GitHub.

### Step 1: connect

Point your client at the endpoint:

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

Sign in when the browser opens. Search, previews and the free formats work from there.

### Step 2: unlock the paid formats when you need them

Icon SVG, icon Lottie and the full-size illustration formats need an Icons8 plan with MCP access.
See [the plans](https://icons8.com/pricing?utm_source=github&utm_campaign=mcp). The account you signed in with carries
the plan. There is no second server to add and no second configuration to swap in.

The `get_icon_svg` tool appears in the tool list once the connection carries a credential. If your
agent does not see a paid format after you subscribe, ask it to call the tool again. If that does not
help, sign in again: `/mcp` in Claude Code, `codex mcp login icons8mcp` in Codex, **Connect** in Cursor.

#### A client without OAuth

Send a personal API key instead. The MCP tab of your Icons8 account has the snippet with your key
already in it. You can also add the header by hand:

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

A key skips the browser, which suits a build server or CI. A key reaches the icon tools. The paid
illustration formats and `account_info` need the sign-in (see [Tools](#tools)).

---

## Tools

The server has 13 tools in three groups. Icons are small symbols for buttons, menus and tables.
Illustrations are full scenes for hero images, empty states and onboarding.

### Icons

| Tool | What it does |
|---|---|
| `search_icons` | Search the catalog by keyword. `animated=True` narrows the results to animated icons. |
| `list_categories` | List icon categories, paginated. |
| `list_platforms` | List icon platforms (styles) such as iOS, Fluent and Color, paginated. |
| `get_icon_svg` | Fetch an icon as SVG. Needs a plan with MCP access. |
| `get_icon_png_url` | Build a PNG preview URL for an icon (24x24 by default). Free. |
| `get_icon_animation` | Get the motion of an animated icon. `gif` and `apng` return a permanent URL and are free. `lottie` returns the animation as JSON and needs a plan with MCP access. |

### Illustrations

| Tool | What it does |
|---|---|
| `search_illustrations` | Search the Ouch catalog by keyword. Every result carries a permanent preview link. |
| `list_illustrations_categories` | List illustration categories, paginated. |
| `list_illustrations_styles` | List illustration styles. Keep one style across a project. |
| `get_illustration_svg` | Build a presigned SVG download URL. |
| `get_illustration_png_url` | Build a presigned PNG download URL in three sizes. |
| `get_illustration_animation` | Build a presigned download URL for an animated illustration: WebM, HEVC MP4, ProRes, GIF, Lottie or After Effects project. |

Presigned illustration URLs expire after one hour. The permanent link to an illustration is the
preview in its search result. Most animated illustrations already carry permanent WebM and HEVC
preview links in the search result, so embedding one needs no second call.

### Your account

| Tool | What it does |
|---|---|
| `account_info` | Show your plan, the icon SVG and Lottie downloads left, the renewal date and the server's own limit on paid downloads. It takes no download and changes no plan. |

---

## Free and paid

| Free (after you sign in or send a key) | Needs a plan with MCP access |
|---|---|
| Search, categories, platforms and styles | `get_icon_svg` |
| PNG icon links | `get_icon_animation` with `format="lottie"` |
| GIF and APNG for animated icons | `get_illustration_svg` |
| Illustration previews and `gif-low` | `get_illustration_png_url` with `size="hd"` |
| Anything an illustration marks `free_distribution: true` | `get_illustration_animation` in any format except `gif-low` |

The paid illustration formats need an account whose plan covers both illustrations and MCP access.
The Illustrations plan alone has no MCP access.

Free PNG use requires attribution with a link back to Icons8. See the [Icons8 license](https://icons8.com/license?utm_source=github&utm_campaign=mcp).

### Download limit

Paid downloads are limited to 100 per hour and 1000 per day for each account, icons and
illustrations together. A refused call returns `retry_after_seconds` and names the free format that
still works. Each plan also has its own download allowance for icon SVG and Lottie. When it is spent,
the tool returns `limit_exhausted` with a free alternative. `account_info` shows what is left.

---

## Configure your client

**Claude Code**
1. Run:
   ```bash
   claude mcp add --transport http icons8mcp https://mcp.icons8.com/mcp/
   ```
2. Check that the server is registered, then sign in with `/mcp`:
   ```bash
   claude mcp list
   ```
   Before you sign in the status reads `Needs authentication`. That means the endpoint answered.

**Claude Desktop and claude.ai**
1. Open Settings → Connectors → **Add custom connector**.
2. Enter the URL `https://mcp.icons8.com/mcp/` and save.
3. Click **Connect** and sign in to Icons8 in the browser.

**Codex**
```bash
codex mcp add icons8mcp --url https://mcp.icons8.com/mcp/
codex mcp login icons8mcp
```

**Cursor**
1. Open Cursor → Settings → Cursor Settings → MCP.
2. Click **+ Add new global MCP server**.
3. Paste the JSON from Step 1, then click **Connect** to sign in.

**Windsurf**
1. Open Windsurf → Settings → Windsurf Settings → MCP.
2. Choose Manage plugins → **View raw config**.
3. Paste the JSON configuration and save.

**VS Code (agent mode)**
1. Open Settings and search for "MCP".
2. Choose **Edit in settings.json** and add the JSON configuration.
3. Open the chat toolbar, switch to Agent mode and look for the tools under *MCP Server: Icons8 MCP*. Restart VS Code if the tools do not appear.

**Any other MCP host**
Use the JSON from Step 1. If the host supports OAuth, it walks you through the sign-in. If not,
add the `Authorization` header shown above. Hosts name this transport differently: `http`,
`streamable-http` or, in older clients, `SSE`. All of them mean this remote endpoint over HTTP.

---

## For agents and automation

The server publishes documents for agents that read URLs:

- [`/llms.txt`](https://mcp.icons8.com/llms.txt) describes the server and every tool.
- [`/auth.md`](https://mcp.icons8.com/auth.md) describes how to authenticate: OAuth with Client ID Metadata Documents or Dynamic Client Registration, agent registration by email, and a personal key.
- [`/.well-known/agent-skills/index.json`](https://mcp.icons8.com/.well-known/agent-skills/index.json) lists the Icons8 agent skills, which live in [icons8/agent-skills](https://github.com/icons8/agent-skills).

A request with no credential gets a `401` with a `WWW-Authenticate` header. That is where the
sign-in starts for an OAuth client.

---

## Live demos
- [Falling Emojis Animation](https://goodies.icons8.com/landings/mcp/static/demo/1-falling-emojis.html)
- [Solar System in Tufte Style](https://goodies.icons8.com/landings/mcp/static/demo/2-solar-system-tufte.html)
- [Sci-Fi Control Interface](https://goodies.icons8.com/landings/mcp/static/demo/3-sci-fi-interface.html)
- [Product Catalog Showcase](https://goodies.icons8.com/landings/mcp/static/demo/4-product-catalog.html)

---

## FAQ

**My client says the server needs authentication. Is something broken?**  
No. That is the normal state before you sign in. The endpoint answered and waits for you. Sign in
with `/mcp` in Claude Code, `codex mcp login icons8mcp` in Codex, or **Connect** in Cursor.

**Do I need an API key?**  
Only where your client cannot do OAuth, or on a build server where nobody can sign in. Elsewhere,
signing in replaces the key. The account, not the key, carries your plan.

**I asked for SVG and received PNG. Why?**  
`get_icon_svg` appears only when the connection carries a credential. A plan with MCP access
unlocks it. If you subscribed after connecting, sign in again, then ask for SVG explicitly.

**A paid call was refused. What now?**  
Read `account_info`. The refusal names a free format that still works, and `retry_after_seconds`
tells you when the hourly or daily limit lifts.

**Do I need Node.js or `npx mcp-remote`?**  
No. The server speaks Streamable HTTP, so your client connects to it directly. An older
`npx mcp-remote` configuration keeps working.

**Where is my data processed?**  
The server runs at `mcp.icons8.com`. Icons8 [terms and privacy](https://icons8.com/?utm_source=github&utm_campaign=mcp) apply.

**Need help?**  
[Chat with us](https://icons8.com/contact?utm_source=github&utm_campaign=mcp).

---

## More from Icons8

[Mega Creator](https://icons8.com/mega-creator?utm_source=github&utm_campaign=mcp) makes full graphics. The illustration
catalog has styles such as [3D Casual Life](https://icons8.com/illustrations/styles/3d-casual-life?utm_source=github&utm_campaign=mcp),
[Cherry](https://icons8.com/illustrations/styles/cherry?utm_source=github&utm_campaign=mcp) and
[3D Stickle](https://icons8.com/illustrations/styles/3d-stickle?utm_source=github&utm_campaign=mcp).

## Credits
- [Product Hunt “Product of the Day” launch recognition](https://www.producthunt.com/products/icons8?launch=icons8-mcp-server).
- © Icons8 — terms, privacy and support links are on [icons8.com](https://icons8.com/?utm_source=github&utm_campaign=mcp).
