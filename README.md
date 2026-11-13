# TallyBridge – How to use

TallyBridge keeps your monday.com boards in step with your TallyPrime cost categories and cost centres. You get month-wise figures, budget and grant balances, and ready-made MIS dashboards, updated on your schedule.

## What you need

- A monday.com **account admin** to install and set up the app. Other users see a read-only screen.
- **TallyPrime** with the companies you want to sync, and their Cost Categories and Cost Centres in use.
- The **TallyBridge TDL** file `CC_FY_Monthly_Dynamic.tdl`. Email support@thinqloud.com if you don't have it.
- A Windows or Linux computer next to Tally with **Node.js 18 or later**, to run the Tally Secure Connector.
- About **20 minutes**, once.

## 1. Install the app

1. Install TallyBridge from the monday.com marketplace and choose the workspaces that can use it.
2. Open TallyBridge. Your **14-day free trial** starts automatically.
3. On the welcome screen, click **Get started**.
4. In the **monday.com** tab, click **Connect monday.com** and approve. This lets scheduled syncs run when nobody is logged in.

## 2. Load the TDL in TallyPrime

1. Copy `CC_FY_Monthly_Dynamic.tdl` to the Tally computer, for example `C:\TallyTDL\`.
2. In TallyPrime press **F1 → TDLs & Add-ons → Manage Local TDLs**. Set *Load TDLs on startup* to **Yes** and add the file's path.
3. Press **F1 → Settings → Connectivity**. Set *TallyPrime acts as* to **Server**, port **9000**. Restart TallyPrime.
4. Open the companies you want to sync. Tally only shares open companies.

**Important:** never open port 9000 to the internet. Tally's gateway has no password and no encryption.

## 3. Install the Tally Secure Connector

1. Install Node.js 20 LTS from nodejs.org.
2. Unzip the `tally-secure-connector` folder to `C:\TallySecureConnector`.
3. Double-click **start-connector.bat**. Copy the **connector key** it shows (it starts with `tsc1_`).
4. To keep it running after restarts, install it as a Windows service with NSSM:
   `nssm install TallySecureConnector "C:\Program Files\nodejs\node.exe" "C:\TallySecureConnector\connector.js"`

The connector encrypts every request and response with your key (AES-256-GCM) and allows read-only report exports only. TallyBridge can never change your books.

## 4. Make the connector reachable

Choose one option:

- **Cloudflare Tunnel (recommended, no open ports):** create a tunnel in Cloudflare Zero Trust, install it on the Tally computer, and point a hostname such as `tally.yourcompany.com` to `http://localhost:9443`.
- **Forward one port:** forward TCP **9443** to the Tally computer and add an HTTPS certificate in `connector.config.json`. Never forward 9000.

## 5. Connect Tally in the app

1. Open the **Tally** tab and choose **Tally Secure Connector**.
2. Enter the connector URL and paste the connector key.
3. Your open companies load automatically. Tick the ones to sync and give each a friendly name.
4. Click **Check report** to confirm the TDL works.

## 6. Choose the workspace and schedule

1. In the **monday.com** tab, choose a workspace (or create one). Keep **One folder per Tally company** on.
2. In the **Schedule** tab, choose your time zone, working days and sync times.
3. Click **Save changes**, then go to **Overview** and click **Sync now**.

## What you get in monday

- One folder per company, and one board per **Cost Category** with one item per **Cost Centre**.
- Month-wise **Dr**, **Cr** and **Cl** columns for April to March.
- Your own columns, **Budget, Grant Received, Total Unit, Unit Cost and Notes**, are never overwritten.
- **Total Utilization, Balance, Percentage Utilized and Unutilized as per Grant** are calculated by monday formulas.
- An **MIS Sheet** view on every board, and an **MIS Dashboard** view on the Professional and Business plans.
- A **Logs** folder with a Sync Log board, and the last 15 runs in the **History** tab.

## Plans

| Plan | Companies | Syncs per day | MIS Dashboard |
| --- | --- | --- | --- |
| Starter | 1 | 2 | No |
| Professional | 5 | 8 | Yes |
| Business | 25 | 8 | Yes |

When a trial or subscription ends, syncing pauses. Your boards and settings are kept.

## Troubleshooting

| Message | What to do |
| --- | --- |
| The connector rejected the key | Run `node connector.js --show-key`, paste the key in the Tally tab and save |
| Its clock is wrong | Turn on automatic time sync on the connector computer |
| Cannot reach TallyPrime | Open TallyPrime and the company; check `http://localhost:9000` on that computer |
| Timeout | The connector or tunnel is not running, or the address changed |
| That address is on a private network | Use the public tunnel address, not a 192.168.x / 10.x / 172.16.x address |
| Your free trial or subscription has ended | An account admin chooses a plan, then clicks Refresh |

## Uninstalling

Uninstalling TallyBridge deletes everything it stored for your account immediately. Boards it created stay in your monday account.

## Help

Email **support@thinqloud.com**. We reply within one business day.
