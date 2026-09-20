<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Refresh &amp; Content Opportunity Scoring — FlyRank ML Internship Capstone</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
<style>
  :root{
    --text:#0F172A;
    --bg:#FAFAFA;
    --accent:#2563EB;
    --muted:#5B6472;
    --rule:#E2E5EA;
    --card:#FFFFFF;
  }
  *{box-sizing:border-box;}
  html{scroll-padding-top:24px;}
  body{
    margin:0;
    background:var(--bg);
    color:var(--text);
    font-family:'Inter',-apple-system,BlinkMacSystemFont,sans-serif;
    font-weight:400;
    line-height:1.65;
    -webkit-font-smoothing:antialiased;
  }
  .wrap{max-width:700px;margin:0 auto;padding:0 24px 96px;}
  header.hero{
    max-width:700px;margin:0 auto;padding:72px 24px 40px;
  }
  .kicker{
    color:var(--accent);
    font-weight:600;
    font-size:0.95rem;
    margin:0 0 18px;
  }
  h1{
    font-weight:700;
    font-size:2.15rem;
    line-height:1.25;
    letter-spacing:-0.01em;
    margin:0 0 28px;
  }
  .meta{
    display:flex;
    flex-wrap:wrap;
    gap:8px 22px;
    font-size:0.88rem;
    color:var(--muted);
    padding-top:22px;
    border-top:1px solid var(--rule);
  }
  .meta strong{color:var(--text);font-weight:600;}
  section{padding:40px 0;border-top:1px solid var(--rule);}
  section:first-of-type{border-top:none;padding-top:8px;}
  h2{
    font-weight:700;
    font-size:1.3rem;
    margin:0 0 18px;
    letter-spacing:-0.005em;
  }
  h3{
    font-weight:600;
    font-size:1rem;
    margin:28px 0 10px;
  }
  p{margin:0 0 16px;max-width:62ch;}
  .abstract{
    background:var(--card);
    border:1px solid var(--rule);
    border-radius:6px;
    padding:24px 26px;
  }
  .abstract p{margin:0;max-width:none;}
  ul,ol{margin:0 0 16px;padding-left:22px;}
  li{margin-bottom:8px;max-width:60ch;}
  code{
    font-family:ui-monospace,SFMono-Regular,Menlo,monospace;
    font-size:0.86em;
    background:#EFF2F6;
    padding:0.1em 0.4em;
    border-radius:4px;
  }
  table{
    width:100%;
    border-collapse:collapse;
    margin:8px 0 20px;
    font-size:0.92rem;
  }
  th,td{
    text-align:left;
    padding:10px 12px;
    border-bottom:1px solid var(--rule);
  }
  th{font-weight:600;color:var(--muted);font-size:0.82rem;}
  .figure{margin:24px 0;}
  .figure svg{width:100%;height:auto;display:block;}
  .figure-caption{font-size:0.85rem;color:var(--muted);margin-top:10px;}
  .callout{
    border-left:3px solid var(--accent);
    padding:4px 0 4px 18px;
    color:var(--text);
    font-size:0.96rem;
  }
  .callout p{margin:0;max-width:none;}
  a{color:var(--accent);text-decoration:underline;text-decoration-color:#B7C8F5;text-underline-offset:2px;}
  a:hover{text-decoration-color:var(--accent);}
  .archetype-table td:first-child{font-weight:500;}
  footer{
    max-width:700px;margin:0 auto;padding:40px 24px 60px;
    border-top:1px solid var(--rule);
    color:var(--muted);
    font-size:0.85rem;
  }
  @media (max-width:600px){
    header.hero{padding:48px 20px 32px;}
    h1{font-size:1.7rem;}
  }
</style>
</head>
<body>

<header class="hero">
  <p class="kicker">FlyRank ML Internship — Capstone Research Paper</p>
  <h1>Refresh &amp; Content Opportunity Scoring: a CTR‑gap model for prioritizing content review</h1>
  <div class="meta">
    <span><strong>Track:</strong> Machine Learning</span>
    <span><strong>Lane:</strong> Refresh / Content Opportunity Scoring</span>
    <span><strong>Author:</strong> Sherifdeen Bilal Olamilekan</span>
  </div>
</header>

<div class="wrap">

<section id="abstract">
  <h2>Title &amp; Abstract</h2>
  <div class="abstract">
    <p><strong>Question:</strong> Across thousands of indexed content pages, which ones are underperforming the click‑through rate a page in their position should get, and what should a reviewer check first?
    <strong>Method:</strong> A Random Forest regressor was trained on March 2026 search performance data and benchmarked against a simple position‑band baseline, then validated two independent ways — a client‑grouped split and a time‑aware February‑to‑March split — to rule out memorization and staleness effects.
    <strong>Result:</strong> the model beat the naive baseline by 3.2% (MAE 0.00268 vs. 0.00277) and showed no generalization gap across time (0.00243 in both directions).
    <strong>Application:</strong> applied to 99,197 eligible pages, the model flagged 58,480 (59.0%) for review, each routed to one of three specific actions based on its search position.
    <strong>Framing:</strong> every finding here is decision‑support, not causal — a page being flagged means it's worth a look, not that any single fix will close the gap.</p>
  </div>
</section>

<section id="intro">
  <h2>Introduction &amp; Problem Statement</h2>
  <p>A large content library accumulates pages faster than any team can manually review them. Some pages rank well but aren't clicked; some rank poorly and probably shouldn't be prioritized yet; most pages are performing about as expected and don't need attention at all. Without a systematic way to separate these, review time goes wherever someone happens to look, not where the data says it would help.</p>
  <p>This work builds that triage layer: a model of expected click‑through rate, a score for how far each page's <em>actual</em> CTR falls short of what the model expects, and a ranked queue that routes each flagged page to one specific action. The decision it supports is narrow and deliberate — which pages a human reviews first — not whether any recommended fix will change a ranking.</p>
</section>

<section id="data">
  <h2>Data</h2>
  <p>Built on the <code>FlyRank/internship-warehouse</code> release (Hugging Face, gated access). The core table is <code>fact_content_daily_performance</code>, partitioned by month; this analysis uses the March 2026 partition joined against <code>dim_content</code> for content‑level features.</p>
  <ul>
    <li>9,841,378 total rows in the March partition; 7,700,646 (~78%) survive the join‑integrity filter against GSC and GA4 client data.</li>
    <li>Aggregated to one row per content item, then filtered to items with ≥100 monthly impressions — below that threshold a click‑through‑rate estimate is too noisy to trust. This eligibility gate is what produces the 99,197‑item analysis set.</li>
    <li><code>fact_content_query_90d</code> was intentionally excluded from development — it's the warehouse's sealed, held‑out table and was not touched at any stage of feature building or validation.</li>
  </ul>
  <p>No client names, domains, URLs, private queries, credentials, or raw data exports appear anywhere in this paper, its figures, or its underlying repository.</p>
</section>

<section id="methodology">
  <h2>Methodology</h2>
  <h3>Features &amp; label</h3>
  <p>Numeric features: impression‑weighted average search position, monthly impressions, search volume, and category count. Categorical: main search intent (one‑hot encoded). Label: actual click‑through rate (clicks ÷ impressions).</p>
  <h3>Baseline</h3>
  <p>A position‑band lookup table: expected CTR is read off five position bands (1–3, 4–6, 7–10, 11–20, 21+). The underlying CTR‑vs‑position relationship was confirmed to hold cleanly across all five bands before any model was built, from 0.34% CTR in the top band down to 0.13% in the bottom, with large sample sizes throughout.</p>
  <h3>Model</h3>
  <p>A Random Forest regressor (300 trees, max depth 8) was chosen over a single decision tree for stability, because roughly a third of eligible items are tied at exactly zero clicks — a cluster a shallow single tree tends to overfit around.</p>
  <h3>Validation design — two independent checks</h3>
  <ol>
    <li><strong>Client‑grouped split.</strong> Rows belonging to the same client never appear in both the training and test sets. A naive random split showed a falsely better error (MAE 0.00245) than the grouped split (MAE 0.00268) — confirming that a random split lets the model partly memorize a client's typical CTR level rather than learn the general position‑CTR relationship.</li>
    <li><strong>Time‑aware split.</strong> Trained on February data, tested on March, to check whether the relationship holds across time rather than only across clients.</li>
  </ol>
  <p>The queue score is <code>predicted_ctr − actual_ctr</code>; items scoring above a 0.05‑percentage‑point gap are flagged, then sorted by score (largest gap first) and by impressions (larger audience first) as a tiebreak.</p>
</section>

<section id="results">
  <h2>Results</h2>
  <table>
    <thead><tr><th>Model</th><th>Split</th><th>MAE</th></tr></thead>
    <tbody>
      <tr><td>Position‑band baseline</td><td>—</td><td>0.00277</td></tr>
      <tr><td>Random Forest</td><td>Client‑grouped</td><td>0.00268 <span style="color:var(--muted)">(−3.2%)</span></td></tr>
      <tr><td>Random Forest</td><td>Within‑March (random)</td><td>0.00243</td></tr>
      <tr><td>Random Forest</td><td>Time‑aware (Feb→Mar)</td><td>0.00243 <span style="color:var(--muted)">(0.0% gap)</span></td></tr>
    </tbody>
  </table>

  <div class="figure">
    <svg viewBox="0 0 640 220" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Bar chart comparing model error across baseline and validation splits">
      <text x="0" y="18" font-family="Inter,sans-serif" font-size="13" font-weight="600" fill="#0F172A">Mean absolute error, by model and split</text>
      <g font-family="Inter,sans-serif" font-size="11" fill="#5B6472">
        <!-- bars: max value 0.00277 scaled to 160px -->
        <!-- baseline -->
        <rect x="140" y="46" width="252.6" height="26" fill="#B7C8F5"></rect>
        <text x="132" y="63" text-anchor="end">Baseline</text>
        <text x="400" y="63" fill="#0F172A">0.00277</text>
        <!-- grouped RF -->
        <rect x="140" y="84" width="245.2" height="26" fill="#5A85EE"></rect>
        <text x="132" y="101" text-anchor="end">Grouped split</text>
        <text x="392" y="101" fill="#0F172A">0.00268</text>
        <!-- within march -->
        <rect x="140" y="122" width="222.4" height="26" fill="#2563EB"></rect>
        <text x="132" y="139" text-anchor="end">Within‑March</text>
        <text x="370" y="139" fill="#0F172A">0.00243</text>
        <!-- time-aware -->
        <rect x="140" y="160" width="222.4" height="26" fill="#1D4ED8"></rect>
        <text x="132" y="177" text-anchor="end">Time‑aware</text>
        <text x="370" y="177" fill="#0F172A">0.00243</text>
      </g>
    </svg>
    <p class="figure-caption">Lower is better. The validated model beats the baseline, and the time‑aware split shows no gap versus the within‑March split — the position–CTR relationship generalizes across time.</p>
  </div>

  <div class="figure">
    <svg viewBox="0 0 640 230" xmlns="http://www.w3.org/2000/svg" role="img" aria-label="Bar chart of permutation feature importance">
      <text x="0" y="18" font-family="Inter,sans-serif" font-size="13" font-weight="600" fill="#0F172A">Permutation importance, deployment model</text>
      <g font-family="Inter,sans-serif" font-size="11" fill="#5B6472">
        <rect x="150" y="42" width="303" height="24" fill="#2563EB"></rect>
        <text x="142" y="58" text-anchor="end">avg_position</text>
        <text x="460" y="58" fill="#0F172A">0.1336</text>

        <rect x="150" y="76" width="68.6" height="24" fill="#5A85EE"></rect>
        <text x="142" y="92" text-anchor="end">impressions</text>
        <text x="226" y="92" fill="#0F172A">0.0302</text>

        <rect x="150" y="110" width="65.4" height="24" fill="#5A85EE"></rect>
        <text x="142" y="126" text-anchor="end">search_volume</text>
        <text x="223" y="126" fill="#0F172A">0.0288</text>

        <rect x="150" y="144" width="55.2" height="24" fill="#5A85EE"></rect>
        <text x="142" y="160" text-anchor="end">category_count</text>
        <text x="213" y="160" fill="#0F172A">0.0243</text>

        <rect x="150" y="178" width="10.7" height="24" fill="#B7C8F5"></rect>
        <text x="142" y="194" text-anchor="end">main_intent</text>
        <text x="168" y="194" fill="#0F172A">0.0047</text>
      </g>
    </svg>
    <p class="figure-caption">Average search position dominates the model's signal by roughly a factor of four over the next‑largest feature, consistent across every validation split run in this project.</p>
  </div>

  <p>Applying the validated model to the full 99,197‑item eligible set flags <strong>58,480 items (59.0%)</strong> for review. That share is large, but it's consistent with an earlier observation that a third of the eligible pool sits tied at exactly zero clicks — many flagged items are underperforming by construction, not by surprise.</p>
</section>

<section id="limitations">
  <h2>Limitations &amp; Honest Framing</h2>
  <ul>
    <li><strong>Cross‑sectional, single month.</strong> This is one month's snapshot with no intervention — it supports "these pages look worth reviewing, because their predicted CTR exceeds their actual CTR," not "fixing the title will raise the CTR." No causal claim is made anywhere in this paper.</li>
    <li><strong>Staleness / refresh timing could not be tested.</strong> An earlier check for whether older content underperforms found the eligibility gate itself correlated with the staleness variable, leaving only a handful of older items (69, 32, and 0 across three older bands) against 88,985 in the freshest band — too little data to draw a conclusion either way. This negative result is reported, not hidden.</li>
    <li><strong>Filters are named alongside every finding.</strong> The 100‑impression eligibility gate that produces the 99,197‑item set is stated wherever that count is used, since it silently excludes lower‑traffic content from the analysis.</li>
    <li><strong>Worst errors under‑predict outlier‑high‑CTR items</strong> — the model is more conservative than reality on the small number of pages that dramatically outperform their position.</li>
  </ul>
</section>

<section id="recommendations">
  <h2>Ranked Recommendations</h2>
  <p>Every flagged item is routed to exactly one action, based on where it already ranks — because the right next step depends on whether the page has visibility it isn't converting, or hasn't earned visibility yet:</p>
  <table class="archetype-table">
    <thead><tr><th>Archetype</th><th>Position</th><th>Recommended action</th></tr></thead>
    <tbody>
      <tr><td>High‑rank, ignored</td><td>1–6</td><td>Review snippet &amp; title</td></tr>
      <tr><td>Mid‑rank, ignored</td><td>7–10</td><td>Review snippet/title + content‑relevance check</td></tr>
      <tr><td>Low‑rank, ignored</td><td>11+</td><td>Improve ranking signals</td></tr>
    </tbody>
  </table>
  <div class="callout">
    <p>Nothing here is auto‑published. Every flagged item passes through human review before any title, snippet, or content change goes live — the model's job is to order the queue, not to make the edit.</p>
  </div>
</section>

<section id="reproducibility">
  <h2>Reproducibility</h2>
  <p>All notebooks referenced in this paper are committed to the project repository under <code>work/notebooks/</code>:</p>
  <ul>
    <li><code>w04_baseline_score.ipynb</code> — position‑band baseline &amp; CTR‑vs‑position confirmation</li>
    <li><code>w05_model.ipynb</code> — Random Forest model, client‑grouped validation</li>
    <li><code>w06_validation_audit.ipynb</code> — time‑aware (Feb→March) validation</li>
    <li><code>w07_action_playbook.ipynb</code> — final scoring, archetypes, ranked queue, exports</li>
  </ul>
  <p>Repository: <a href="https://github.com/bilalsherifdeen1-ux/My-FlyRank-AI-project" target="_blank" rel="noopener">github.com/bilalsherifdeen1-ux/My-FlyRank-AI-project</a></p>
</section>

</div>

<footer>
  <p>Built on the FlyRank ML Internship dataset — <a href="https://flyrank.ai" target="_blank" rel="noopener">flyrank.ai</a>.</p>
</footer>

</body>
</html>
