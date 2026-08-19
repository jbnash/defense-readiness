# Defense Readiness Check

A planning aid for UK doctoral students (EdD and PhD) working out whether a target defense term is realistic. It answers procedural questions about NOTIF timing, 767 enrollment, committee distribution, the Request for Final Examination, summer off-contract conflicts, and the cost of defending right at the term-end deadline. Built by John B. Nash, DGS, Educational Leadership Studies, University of Kentucky.

## Not a substitute for the authoritative sources

Not an official Graduate School product. Not a replacement for your chair, committee, or DGS on substantive work — feedback on chapters, methodology, revisions, scholarly defense readiness. For procedural answers the canonical sources are the DGS office (859-257-7845), the UK Graduate School DGS Manual, the EDL Doctoral Students SharePoint, and the UK Registrar's Academic Calendar. All are linked from the tool.

Rules and dates may be updated to reflect current university and accreditation policy, and updates are enforced as of the date they take effect. For course requirements, refer to the Graduate Catalog for the year you started your program.

## Terms covered

Fall 2026 through Summer 2028. Dates for Fall 2026 through Summer 2027 are taken from the Registrar's published academic calendar. Dates for Fall 2027 through Summer 2028 are marked provisional in the interface: term anchors come from the official Five-Year Calendar, and the Graduate School deadlines are projected from offsets the Registrar has used consistently in every published year.

## Maintaining it

Single self-contained HTML file. No build step, no framework, no dependencies — open `index.html` in any browser.

- To add a term, edit the `TERMS` object near the top of the inline `<script>` and add the key to `TERM_ORDER`.
- To change planning assumptions, edit the constants beneath it: `REVISION_BUFFER_DAYS` (60), `NOTIF_LEAD_DAYS` (56), `CHAIR_LEAD_DAYS` (70), `DIST_LEAD_DAYS` (56), `REQUEST_EXAM_DAYS` (14), `OUTSIDE_COPY_DAYS` (14).
- When the Registrar publishes the AY 2027–28 calendar, replace the projected dates and remove `provisional: true` from those three terms.
