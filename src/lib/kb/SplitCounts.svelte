<script lang="ts">
  import reports from '#lib/data/reports-4471.json';
  import notes from '#lib/data/desk-notes.json';
  import { causeOf, uniqueById, type Report } from '#lib/kb/causes';
  import { tenantOf } from '#lib/kb/locales';

  const merged = uniqueById(reports as Report[]).filter((r) => r.mergedInto === 4471);
  const oneDay = new Set(notes.oneDayMove);
  const groups = { parser: 0, hint: 0, unplaced: 0, oneDay: 0 };
  for (const r of merged) {
    const c = causeOf(r, tenantOf(r)) ?? 'unplaced';
    groups[c]++;
    if (oneDay.has(r.id)) groups.oneDay++;
  }
</script>

<table>
  <thead>
    <tr><th>problem</th><th>reports</th><th>own ticket</th></tr>
  </thead>
  <tbody>
    <tr>
      <td><code>parser</code> — month-first parse on day-first tenants</td>
      <td>{groups.parser}</td>
      <td>yes — see GitHub</td>
    </tr>
    <tr>
      <td><code>hint</code> — booking-v3 MM/DD hint vs tenant locale</td>
      <td>{groups.hint}</td>
      <td>yes — see GitHub</td>
    </tr>
    <tr>
      <td>one-day shift — not a month swap (desk notes)</td>
      <td>{groups.oneDay}</td>
      <td>yes — see GitHub</td>
    </tr>
  </tbody>
</table>

<p class="sum"><strong>{merged.length}</strong> unique reports were merged into #4471. They are three problems, not one.</p>

<style>
  .sum { font-size: 1.1rem; }
</style>
