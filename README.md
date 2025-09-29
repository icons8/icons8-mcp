# Icons8 MCP server

Icons8 MCP Server gives AI coding environments instant access to 368,865+ Icons8 icons across 116 design styles. Connect your Claude, Cursor, Windsurf, VS Code, or any SSE-capable MCP host to stream high-res PNGs for free or unlock full SVG delivery with an API key—ideal for prototyping, production code, and rapid UI experiments.

### Use cases
- Replace entire icon sets in existing projects with consistent, on-trend styles.
- Add icons to bullet lists, feature highlights, process steps, and dashboards without leaving your IDE.
- Prototype quickly with free PNG previews, then upgrade to SVGs for production-ready assets.
- Build AI-assisted workflows that search, preview, and drop icons directly into your code.
- Showcase interactivity with ready-made demos such as falling emojis, sci-fi dashboards, and product catalogs.

---

## How it works

The Icons8 MCP endpoint is hosted at `https://mcp.icons8.com/mcp/` and works with any MCP client that supports Server-Sent Events.

### Prerequisites
1. [Node.js](https://nodejs.org/) installed locally.
2. An MCP-compatible editor or agent host (Claude Code, Cursor, Windsurf, VS Code, etc.).

### Pick your plan

#### Free high-res PNG icons
Use this configuration to stream PNG previews in high resolution without authentication:

```json
{
  "mcpServers": {
    "icons8mcp": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "https://mcp.icons8.com/mcp/"
      ]
    }
  }
}
```

#### Full SVG access (API key required)
Subscribe to the [SVG plan for $15](https://icons8.com/icons/pricing) and pass your API key as a Bearer token:

```json
{
  "mcpServers": {
    "icons8mcp": {
      "command": "npx",
      "args": [
        "mcp-remote",
        "https://mcp.icons8.com/mcp/",
        "--header",
        "Authorization:${AUTH_HEADER}"
      ],
      "env": {
        "AUTH_HEADER": "Bearer YOUR_API_KEY"
      }
    }
  }
}
```

> Tip: swap configs as needed—PNG setup for free previews, SVG setup when you are ready to ship.

### Configure your client

**Claude Code**
1. Open a terminal and run:
   ```bash
   claude mcp add icons8mcp -- \
     npx mcp-remote \
     "https://mcp.icons8.com/mcp/" \
     --header "Authorization: Bearer YOUR_API_KEY"
   ```
2. Verify the server is registered:
   ```bash
   claude mcp list
   ```

**Cursor**
1. Cursor → Settings → Cursor Settings → MCP tab.
2. Click **+ Add new global MCP server**.
3. Paste the JSON from the plan you chose (omit the header for the free PNG tier).

**Windsurf**
1. Windsurf → Settings → Windsurf Settings → MCP section.
2. Manage plugins → **View raw config**.
3. Paste the JSON configuration and save.

**VS Code (agent mode)**
1. Code → Settings → Settings → search for "MCP".
2. Choose **Edit in settings.json**.
3. Add the JSON configuration.
4. Open the chat toolbar (`⌥⌘B`), switch to Agent mode, and look for tools under *MCP Server: Icons8 MCP*. Restart VS Code if tools do not appear immediately.

**Other SSE-compatible tools**
Use the same JSON template with `npx mcp-remote` and provide the optional `Authorization` header for SVG access. Consult your host documentation for where to paste MCP server settings.

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

**What if it says “No API key”?**  
[Subscribe for $15](https://icons8.com/icons/pricing) to receive an API key.

**How do I get SVG icons specifically?**  
Include an explicit request for SVG icons in your prompt and make sure your configuration passes the Bearer token.

**What formats are available?**  
PNG previews work for all users. SVG downloads require an active API key.

**Can I download PNG icons for free?**  
Yes. PNG icon previews are available without authentication when you connect to the MCP server.

**What are the license terms for the free plan?**  
Free PNG usage requires attribution with a link back to Icons8. Review the [Icons8 license](https://icons8.com/license) for full details.

**Do I need an API key?**  
Only for SVG delivery. PNG previews remain keyless.

**I requested SVG but received PNG. What happened?**  
Ask again with an explicit SVG request and confirm your API key is present.

**Can I download icons in batches?**  
Batch downloads are in progress—stay tuned.

**Need help?**  
Check the setup steps above or [chat with us](https://icons8.com/contact).

---

## Credits
- [Product Hunt “Product of the Day” launch recognition](https://www.producthunt.com/products/icons8?launch=icons8-mcp-server).
- © 2025 Icons8 — terms, privacy, and support links are available on [icons8.com](https://icons8.com).

