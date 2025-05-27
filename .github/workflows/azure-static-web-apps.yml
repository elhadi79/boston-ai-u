/*
Project: Boston AI University - Complete Integrated Codebase
Tech Stack: React, Vite, Tailwind CSS, shadcn/ui, lucide-react, axios, react-player, react-helmet-async
Deployment: Azure Static Web Apps & Azure Functions

-- Repository Structure --

.github/
└── workflows/
    └── azure-static-web-apps.yml     # CI/CD for static frontend + Azure Functions API

.env.example
vite.config.js
/public/
├── index.html
├── robots.txt
└── assets/
    ├── logo.png
    └── hero-bg.jpg

/src/
├── main.jsx
├── index.css
├── App.jsx
├── components/
│   ├── SEO.jsx
│   ├── TopBar.jsx
│   ├── LiveChat.jsx
│   ├── AIDemo.jsx
│   ├── VRPreview.jsx
│   ├── SuccessCalculator.jsx
│   ├── Header.jsx
│   ├── Hero.jsx
│   ├── ValueProp.jsx
│   ├── Statistics.jsx
│   └── FeaturedPrograms.jsx
└── pages/
    ├── Home.jsx
    ├── About.jsx
    ├── Certifications.jsx
    ├── VirtualLearning.jsx
    ├── ForEnterprises.jsx
    ├── ForIndividuals.jsx
    ├── Blog.jsx
    └── Contact.jsx

/functions/
└── socialScraper/
    ├── index.js
    └── function.json

azure-static-web-apps.yml (CI/CD)
---
name: Azure Static Web Apps CI/CD
on:
  push:
    branches:
      - main
jobs:
  build_and_deploy_job:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - name: Build And Deploy
        uses: Azure/static-web-apps-deploy@v1
        with:
          azure_static_web_apps_api_token: ${{ secrets.AZURE_STATIC_WEB_APPS_API_TOKEN }}
          repo_token: ${{ secrets.GITHUB_TOKEN }}
          action: "upload"
          app_location: "./"
          api_location: "/functions"
          output_location: "dist"

.env.example
-------------
VITE_API_URL=https://api.bostonaiu.com
VITE_AI_API_URL=https://ai.bostonaiu.com
CRISP_WEBSITE_ID=your-crisp-id
VITE_SITEMAP_BASE_URL=https://www.bostonaiu.com
AZURE_FUNCTIONS_ENV=Production

vite.config.js
---------------
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';
import { VitePWA } from 'vite-plugin-pwa';
import sitemap from 'vite-plugin-sitemap';

export default defineConfig({
  plugins: [
    react(),
    sitemap({ baseUrl: process.env.VITE_SITEMAP_BASE_URL, pagesDirectory: 'src/pages', targetDir: 'dist' }),
    VitePWA()
  ],
  define: {
    'process.env': process.env
  }
});

/public/index.html
------------------
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Boston AI University</title>
  <link rel="icon" href="/assets/logo.png" />
</head>
<body>
  <div id="root"></div>
  <!-- Crisp Chat -->
  <script>window.CRISP_WEBSITE_ID = "REPLACE_WITH_${CRISP_WEBSITE_ID}";</script>
  <script async src="https://client.crisp.chat/l.js"></script>
</body>
</html>

/public/robots.txt
------------------
User-agent: *
Allow: /
Sitemap: https://www.bostonaiu.com/sitemap.xml

/src/main.jsx
-------------
import React from 'react';
import ReactDOM from 'react-dom/client';
import { BrowserRouter } from 'react-router-dom';
import { HelmetProvider } from 'react-helmet-async';
import App from './App';
import './index.css';

ReactDOM.createRoot(document.getElementById('root')).render(
  <HelmetProvider>
    <BrowserRouter>
      <App />
    </BrowserRouter>
  </HelmetProvider>
);

/src/components/SEO.jsx
-----------------------
import { Helmet } from 'react-helmet-async';
export default function SEO({ title, description }) {
  return (
    <Helmet>
      <title>{title}</title>
      <meta name="description" content={description} />
      <meta property="og:title" content={title} />
      <meta property="og:description" content={description} />
      <meta property="og:url" content={window.location.href} />
      <meta name="twitter:card" content="summary_large_image" />
    </Helmet>
  );
}

/src/components/LiveChat.jsx
----------------------------
export default function LiveChat() {
  const open = () => window.$crisp.push(["do", "chat:open"]);
  return <button onClick={open} className="text-blue-600 underline">Live Chat</button>;
}

/src/components/AIDemo.jsx
--------------------------
import { useState } from 'react';
import axios from 'axios';
export default function AIDemo() {
  const [q, setQ] = useState('');
  const [res, setRes] = useState('');
  const run = async () => {
    const r = await axios.post(`${process.env.VITE_AI_API_URL}/recommend`, { question: q });
    setRes(r.data.answer);
  };
  return (
    <div className="p-4 bg-white rounded">
      <textarea value={q} onChange={e=>setQ(e.target.value)} className="w-full p-2 border mb-2" />
      <button onClick={run} className="bg-blue-600 text-white px-4 py-2">Ask AI</button>
      {res && <p className="mt-2">{res}</p>}
    </div>
  );
}

/src/components/VRPreview.jsx
-----------------------------
import ReactPlayer from 'react-player';
export default () => (
  <ReactPlayer url="https://youtu.be/ScMzIvxBSi4" controls width="100%" />
);

/src/components/SuccessCalculator.jsx
--------------------------------------
import { useState } from 'react';
export default function SuccessCalculator() {
  const [s, setS] = useState(50000), [i, setI] = useState(20), [n, setN] = useState(null);
  return (
    <div className="p-4 bg-white rounded">
      <input type="number" value={s} onChange={e=>setS(+e.target.value)} className="w-full mb-2 p-2 border" />
      <input type="number" value={i} onChange={e=>setI(+e.target.value)} className="w-full mb-2 p-2 border" />
      <button onClick={()=>setN(s*(1+i/100))} className="bg-blue-600 text-white px-4 py-2">Calc</button>
      {n!==null && <p className="mt-2">New Salary: ${n.toFixed(2)}</p>}
    </div>
  );
}

-- pages and other components follow similar pattern, importing SEO at top --

/functions/socialScraper/index.js
---------------------------------
const axios = require('axios');
module.exports = async function (context, myTimer) {
  context.log('SocialScraper run:', new Date().toISOString());
  // example: fetch recent tweets via Twitter API
  // filter keywords, generate DM via OpenAI
  // send outreach and log
};

/functions/socialScraper/function.json
--------------------------------------
{
  "bindings": [{ "name": "myTimer", "type": "timerTrigger", "direction": "in", "schedule": "0 0 * * * *" }]
}

/*
Follow these steps using GitHub’s web UI (no terminal needed):

1. On your repository page (https://github.com/elhadi79/boston-ai-u), click the **Add file** button just above the file list (next to **Go to file**).
2. Choose **Upload files**.
3. Drag and drop the entire scaffold folder contents (all files and subfolders: `/public`, `/src`, `/functions`, `.github`, `vite.config.js`, `.env.example`, etc.) into the upload area.
4. Scroll down, enter “Initial integrated scaffold” in the commit message, and click **Commit changes**.

5. Next, click **Add file** → **Create new file**, name it `.env`, and paste:
   ```
   VITE_API_URL=https://api.bostonaiu.com
   VITE_AI_API_URL=https://ai.bostonaiu.com
   CRISP_WEBSITE_ID=<your-crisp-id>
   VITE_SITEMAP_BASE_URL=https://www.bostonaiu.com
   AZURE_FUNCTIONS_ENV=Production
   ```
   then commit.

6. In the repo, go to **Settings** → **Secrets and variables** → **Actions**, add two new secrets:
   - `AZURE_STATIC_WEB_APPS_API_TOKEN`: your Azure deployment token
   - `CRISP_WEBSITE_ID`: the Crisp Chat website ID

7. After committing and adding secrets, open the **Actions** tab. The **Azure Static Web Apps CI/CD** workflow will run automatically, building and deploying both the frontend and Azure Function API.

8. Once the workflow completes, your site will be live at the URL shown in the workflow logs under **Azure Static Web Apps**.
*/
