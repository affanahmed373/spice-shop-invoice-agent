# Roadmap

Planned improvements. Each item lists the skill it proves and what "done" means.
Add the skill to the CV only after the item is pushed.

## 1. Live demo with synthetic bills (priority: highest)
Proves: a deployed RAG agent a recruiter can try, without exposing the shop's real bills.
- [ ] Script that generates 8-10 fake supplier bills (made-up vendors, products like atta, basmati, haldi, PG Tips, prices in EUR) into `demo_invoices/`, committed to the repo.
- [ ] Gradio Space on Hugging Face: index `demo_invoices/` at startup (in-memory Qdrant), Gemini key as a Space secret.
- [ ] Banner in the app: "Demo data, not the shop's bills".
- [ ] Example question buttons: cheapest vendor, price comparison, invoice total.
- [ ] Rename the Gradio title from "Invoice Agent" to "Spice Shop Invoice Agent".
- [ ] "Try it" link at the top of the README.
Done when: the Space link works and is in the README, the profile README and the CV.

## 2. Structured line items + SQL tool (priority: high)
Proves: structured outputs, text-to-SQL, agents that combine tools.
- [ ] Pydantic `InvoiceLine` (vendor, invoice number, date, product, quantity, unit, unit price, line total), filled from the pdfplumber tables.
- [ ] Store the lines in SQLite (Postgres later) next to the Qdrant chunks.
- [ ] Agent tools: read-only SQL over the line-item table for totals and comparisons; vector search to match product names ("chakki atta" vs "atta 10kg").
- [ ] Remove the "Current limits" note in the README once totals come from SQL.
Done when: "How much did we spend on atta in March?" matches a hand calculation over the demo bills.

## 3. Tests and CI (priority: high)
Proves: tested LLM pipelines.
- [ ] pytest for vendor and line-item extraction on `demo_invoices/`.
- [ ] Golden questions with exact expected answers, run from `rag-agent-evals`.
- [ ] GitHub Actions: lint + pytest on every push.

## 4. Photographed bills (priority: medium)
Proves: vision-LLM OCR, multimodal extraction.
- [ ] Extract the same `InvoiceLine` schema from phone photos of paper bills with a vision model.
- [ ] Compare against pdfplumber on bills that exist as both PDF and photo; report the field accuracy.

## 5. Shop workflows (priority: medium)
Proves: agents that do useful work, not only answer.
- [ ] Price change alert when a supplier raises a product price compared with its last bill.
- [ ] Monthly spend export (CSV) per supplier for the tax advisor.

## 6. Halal certificates and best-before dates (priority: low)
Proves: the same document pipeline on a second document type, with date-based alerts.
- [ ] Ingest supplier halal certificates (issuer, products, expiry date) into the same SQL store.
- [ ] Record best-before dates (Mindesthaltbarkeitsdatum) per delivery from the bills or by hand.
- [ ] Weekly list: certificates expiring within 30 days and stock past or near its best-before date.

## 7. German product labels (priority: low)
Proves: human-in-the-loop generation where mistakes matter.
- [ ] Draft German product names and ingredient and allergen lists (EU food information rules, LMIV) from the supplier's English label.
- [ ] Every draft needs the owner's approval before printing; the app says so on screen.

## 8. Repo hygiene (priority: medium)
- [ ] Move `invoice_agent.ipynb` and `vector_storage.ipynb` into `notebooks/`.
- [ ] Optional: rebuild the agent loop in Pydantic AI or the OpenAI Agents SDK to show a second agent framework.
