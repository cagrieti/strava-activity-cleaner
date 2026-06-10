(async () => {
  const REALLY_DELETE = false; // first run false, review, then change to true

  const authenticityToken =
    document.querySelector('meta[name="csrf-token"]')?.content;

  if (!authenticityToken) {
    console.error("Could not find authenticity/CSRF token.");
    return;
  }

  const rows = [...document.querySelectorAll('tbody tr, .training-activity-row')];
  const candidates = [];

  for (const row of rows) {
    const link = row.querySelector('a[href*="/activities/"]');
    if (!link) continue;

    const id = link.getAttribute("href")?.match(/\/activities\/(\d+)/)?.[1];
    if (!id) continue;

    const text = row.innerText.toLowerCase();

    const isStrength =
      text.includes("weight training") ||
      text.includes("strength") ||
      text.includes("gym") ||
      text.includes("krafttraining");

    if (!isStrength) continue;

    candidates.push({
      id,
      row,
      preview: row.innerText.slice(0, 180)
    });

    row.style.outline = "3px solid red";
    row.style.backgroundColor = "#ffe0e0";
  }

  console.log(`Candidate strength activities: ${candidates.length}`);
  console.table(candidates.map(c => ({
    id: c.id,
    preview: c.preview,
    url: `https://www.strava.com/activities/${c.id}`
  })));

  if (!REALLY_DELETE) {
    console.warn("Dry run only. Review highlighted rows first.");
    console.warn("Then set REALLY_DELETE = true and run again.");
    return;
  }

  const ok = confirm(`Delete ${candidates.length} highlighted activities?`);
  if (!ok) {
    console.log("Cancelled. Nothing deleted.");
    return;
  }

  let success = 0;
  let failed = 0;

  for (const { id, row } of candidates) {
    const body = new URLSearchParams();
    body.set("_method", "delete");
    body.set("authenticity_token", authenticityToken);

    console.log(`Deleting ${id}...`);

    const response = await fetch(`https://www.strava.com/activities/${id}`, {
      method: "POST",
      credentials: "include",
      headers: {
        "accept": "text/html,application/xhtml+xml,application/xml;q=0.9,*/*;q=0.8",
        "content-type": "application/x-www-form-urlencoded"
      },
      referrer: `https://www.strava.com/activities/${id}`,
      body: body.toString()
    });

    console.log(`${id}: ${response.status} ${response.statusText}`);

    if (response.ok) {
      success++;
      row.style.opacity = "0.25";
      row.style.backgroundColor = "#ffcccc";
    } else {
      failed++;
      row.style.backgroundColor = "#fff3cd";
      console.error(`Failed ${id}:`, (await response.text()).slice(0, 500));
    }

    await new Promise(resolve => setTimeout(resolve, 1500));
  }

  console.log(`Done. Deleted: ${success}, failed: ${failed}.`);
  console.log("Refresh the page to verify, then go to the next page manually.");
})();
