# TOOLS.md — Navigator

## Skills (3)
`web-browse`, `web-download`, `web-interact`


## Environment-Specific Notes

### Browser Infrastructure
- Browser tool is built into OpenClaw (browser-tool.ts)
- Actions: navigate, snapshot, screenshot, act (click/type/fill), console, pdf, tabs, upload, dialog
- Chromium must be installed in container (runtime dep, vanishes on rebuild)
- Screenshots stored on-site: workspace dir or /home/node/.openclaw/media/
- Browser runs inside the container, not on the host

### Storage Locations
- On-site screenshots: /home/node/.openclaw/media/screenshots/
- On-site downloads: /home/node/.openclaw/media/downloads/
- Extracted text: returned to requesting agent or stored in Chartroom

### Ops Channels
- #ops-dashboard — Infrastructure monitoring
- #ops-alerts — Critical alerts
