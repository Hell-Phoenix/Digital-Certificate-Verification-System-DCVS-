# Smart India Hackathon 2025 – Execution Master Plan
Project: Digital Certificate Verification System (DCVS)  
Team: CertGuard Innovators (6 Members)  
Objective: Build a rock-solid MVP + 2–3 polished “WOW” add‑ons to maximize judging impact, with clear roadmap and professional delivery—starting from zero skill baseline.

---

## 0. Philosophy (Read First)
Your win probability = (Core Problem Fit × Demo Stability × Differentiation × Clarity of Story).  
Do LESS but 100% reliable, then sprinkle a few impressive yet low-risk enhancements.

KEEP IN MIND:
- A perfect smaller product > half-broken large product.
- Everything must tie back to “reducing academic fraud quickly & reliably.”
- Judges love: measurable impact, clear plan, scalability, security forethought, responsible AI.

---

## 1. Final Hackathon Deliverables (Target)
| Category | MUST (MVP) | WOW Add‑Ons (Pick 2–3) | Roadmap (Slide Only) |
|----------|------------|------------------------|----------------------|
| Verification | ID lookup + Upload OCR + Risk decision | Highlight mismatched fields overlay | Offline QR signature verification |
| Integrity | Canonical JSON + SHA‑256 hash | Merkle root batch demo (local) | Full blockchain / Hyperledger |
| Forensics | Field mismatch detection | Perceptual hash duplicate detection | Deep seal classifier (CNN) |
| UI | Upload, Verify, Dashboard list | Field difference highlighter + PDF verification report | Mobile companion |
| Risk | Rule weights | Simple anomaly (CGPA outlier) | ML anomaly engine |
| Security | JWT + roles (admin/institution/verifier) | Signed QR token (short-lived) | DID + Verifiable Credentials |
| Analytics | Basic counts (verifications, suspect rate) | Flag frequency mini-chart (ASCII or sparkline) | Full analytics portal |
| Documentation | README + Install + ERD + API stub | Governance & Ethical AI note | Policy automation |
| Pitch | Demo + Slide deck | Side-by-side “Forged vs Legit” diff screen | Cross-state federation model |

SUCCESS FORMULA FOR WOW:
- Must be stable.
- Must be explainable in < 30 seconds.
- Must clearly help fight fraud or build trust.

---

## 2. Team Role Assignment (Novice-Friendly Expansion)

| Role | Person | Primary Focus | Ramp-up (First 3 Days) | “Hero Moment” in Demo |
|------|--------|---------------|------------------------|-----------------------|
| Backend Lead | Member 1 | FastAPI APIs, DB models, canonical hash | Learn: FastAPI fundamentals, SQLAlchemy basics, pydantic | Type a certificate UID → instant VALID result |
| ML/OCR Engineer | Member 2 | OCR pipeline, field extraction, fuzz matching, pHash | Learn: PaddleOCR basics, Python PIL/OpenCV, RapidFuzz | Show tampered CGPA detected |
| Frontend Developer | Member 3 | React UI (Upload, Verify pages, Dashboard) | Learn: React components, fetch API, Tailwind CSS basics | Demo risk flags & highlight mismatches |
| DevOps / Infra | Member 4 | Docker compose, .env, scripts, logs | Learn: Docker build, docker-compose, basic Nginx reverse proxy | Start entire stack in <1 min |
| Security & Integrity | Member 5 | Hashing module, QR, signing stub, rate limiting | Learn: Python hashlib, JWT, simple RSA/Ed25519 with libs | Show hash + Merkle root concept |
| QA & Docs | Member 6 | Test cases, seed data, forged samples, README, pitch slides | Learn: PyTest basics, CSV generation, doc structure | Present test matrix “5/5 forgeries caught” |

PAIRING RULE:
- No one works alone on a blocker > 2 hours.
- Have buddy pairs: (Backend+QA), (ML+Security), (Frontend+DevOps).

---

## 3. Learning Ramp (Day 0–2 Quick Start Resources)

| Topic | Resource | Practice Task |
|-------|----------|---------------|
| FastAPI | fastapi.tiangolo.com | Build /hello endpoint returning JSON |
| SQLAlchemy | docs.sqlalchemy.org | Create certificates table + insert row |
| OCR (PaddleOCR) | GitHub: PaddleOCR README | Extract text from sample certificate PNG |
| RapidFuzz | pypi rapidfuzz docs | Compute similarity between “Amit Kumar” & “Amit Kumarr” |
| React + Tailwind | tailwindcss.com/docs | Make button + layout + fetch test endpoint |
| Docker Compose | docs.docker.com | Compose: API + PostgreSQL + Adminer |
| Hashing | Python hashlib docs | Generate SHA-256 of JSON string |
| Perceptual Hash | imagehash (PIL) | Compare two similar images distance |
| JWT | pyjwt | Create/verify token with role claim |
| PyTest | pytest docs | Write test verifying canonical JSON deterministic |

RULE: Each member completes 3 micro-practice tasks before writing main project code.

---

## 4. High-Level Architecture (MVP)
Simple modular backend (not full microservices yet):

Modules:
- api/ (verify, ingestion, dashboard)
- services/
  - ocr_service.py
  - extraction.py
  - risk_engine.py
  - hashing.py
  - forensics.py (pHash)
  - auth.py
- models/ (SQLAlchemy)
- schemas/ (Pydantic validation)

Single PostgreSQL DB; Redis optional for caching (skip if time short).  
Static file store: local /files/ (replace with MinIO later).  

---

## 5. Core Data Flow (Explain Like You Will to Judges)
1. Institution uploads CSV → Each row normalized → Canonical JSON → Hash → Stored.  
2. Verifier:
   - Option A: enters certificate_uid → quick DB lookup → return stored record + integrity hash.
   - Option B: uploads PDF/scan → OCR → extract fields → fuzzy match vs DB → mismatches as FLAGS → compute risk score.
3. Output decision + flags + hash prefix.  
4. (Add-on) Forensic modules compute duplicate detection / layout variance.  
5. (Add-on) Batch of last N hashes aggregated → Merkle root → displayed (conceptual blockchain anchor).

---

## 6. Detailed Timeline (Assume 4 Weeks Prep + Hackathon Days)

### Week 1 (Foundation)
Goal: Running skeleton + ingestion + verify by ID.

| Day | Tasks | Owners | Output |
|-----|-------|--------|--------|
| 1 | Repo init, .env template, docker-compose (API + DB) | DevOps + Backend | Bootable stack |
| 2 | DB models: institutions, certificates, verification_requests | Backend | Alembic migration |
| 3 | CSV ingestion endpoint + validation + canonical JSON + hash | Backend + QA | Hash shown in DB |
| 4 | Basic React pages (Upload, Verify ID) | Frontend | UI skeleton |
| 5 | Auth stub: static API key or simple JWT login | Security | Authorization header |
| 6 | Seed script + 20 genuine samples | QA | seed_certificates.py |
| 7 | Test: ingestion determinism + ID verify | QA | PyTest green |

MILESTONE: End‑to‑end ID verification works.

### Week 2 (OCR + Matching + Risk)
| Day | Tasks | Owners | Output |
|-----|-------|--------|--------|
| 8 | OCR service integration (PaddleOCR) | ML | Raw text extraction |
| 9 | Field extraction (regex + heuristics) | ML + Backend | JSON extracted |
| 10 | Fuzzy name & roll, CGPA numeric parsing | ML | similarity() utility |
| 11 | Risk engine v1 + flags | Backend + Security | risk_engine.py |
| 12 | Verify Upload API (file → decision) | Backend | Response JSON |
| 13 | Frontend: Upload page w/ result + flags | Frontend | Working OCR verify |
| 14 | QA: create 5 forged docs (name, CGPA, fake ID, duplicate, missing seal placeholder) | QA | /tests/data/forged |

MILESTONE: Upload tampered doc triggers correct flags.

### Week 3 (Forensics + Polish)
| Day | Tasks | Owners | Output |
|-----|-------|--------|--------|
| 15 | Perceptual hash (pHash) + duplicate detection | ML | forensics.py |
| 16 | Layout block capture (bounding boxes summary) | ML | layout_fingerprint.json |
| 17 | Compare layout deviation % (simple threshold) | ML + Backend | Added to risk |
| 18 | Dashboard endpoints (stats + recent verifications) | Backend | /dashboard/summary |
| 19 | UI: Dashboard list + suspect rate | Frontend | Visible metrics |
| 20 | Hash batch Merkle root demo (in-memory or file) | Security | merkle_root.json |
| 21 | PDF/JSON verification report (export) | Backend + Frontend | Download button |

MILESTONE: WOW features functional (choose which to finalize).

### Week 4 (Stabilization + Demo Prep)
| Day | Tasks | Owners | Output |
|-----|-------|--------|--------|
| 22 | Signed QR code stub (JWT with certificate_uid + hash_prefix) | Security | scan_qr_demo.png |
| 23 | Mismatch highlight overlay (approx region rectangles) | Frontend + ML | UI difference highlight |
| 24 | Finalize README, architecture diagram, ER diagram | QA + Backend | docs/ outputs |
| 25 | Slide deck (Problem → Solution → Demo → Roadmap) | QA + TL | pitch.pptx |
| 26 | Load test light (10 sequential OCR calls) | DevOps | Timing metrics |
| 27 | Dry run full demo + timed script | ALL | Rehearsal 1 |
| 28 | Buffer / bug fixing / backup video recording | ALL | fallback_demo.mp4 |

HACKATHON DAYS:
- Keep Core Flow stable.
- Freeze code except critical bugs 24h before final demo.
- Assign roles: Driver (keyboard), Narrator, Support (monitor logs).

---

## 7. Backlog (Prioritized User Stories)

| Priority | Story | Acceptance |
|----------|-------|------------|
| P0 | As institution, I upload CSV to register certificates | Returns count imported; errors listed |
| P0 | As verifier, I verify by certificate UID | Returns certificate fields + decision |
| P0 | As verifier, I upload PDF → system returns decision + flags | Risk & flags present |
| P0 | As system, I compute canonical hash | Reproducible & stable |
| P1 | As system, I detect CGPA mismatch >0.05 | Flag CGPA_DIFF |
| P1 | As user, I see recent verification list | Sorted by time |
| P1 | As user, I see risk score & flags explained | Free-text or codes |
| P2 | As system, I detect duplicate uploads | DUPLICATE_DOC flag |
| P2 | As verifier, I download a verification report | PDF/JSON |
| P2 | As institution, I scan QR → certificate validity pops | QR includes UID JSON |
| P3 | As admin, I see simple Merkle root demo | root hash display |
| P3 | As user, I view layout deviation % | Additional flag LAYOUT_DEVIATION |

---

## 8. Risk Matrix (Novice Focus)

| Risk | Likelihood | Impact | Mitigation | Owner |
|------|------------|--------|-----------|-------|
| OCR poor output | Medium | High | Preprocess: deskew, convert to grayscale; pick clear fonts | ML |
| JSON hash mismatch due to order | Low | High | Deterministic sorted keys function; test | Backend |
| Scope creep kills stability | High | High | Feature freeze after Day 21 | Team Lead |
| Demo Wi-Fi failure | Medium | High | Local fallback & video & offline dataset | DevOps |
| Forgeries too easy (not detected) | Medium | Medium | Tune risk weights, ensure thresholds tested | ML + QA |
| Time overrun in pitch | Medium | Medium | Rehearse with timer; script segments | Narrator |
| Fail to explain blockchain story | Low | Medium | Prepare 30-second Merkle narrative | Security |
| Broken PDF causing crash | Medium | Medium | Try/except wrap OCR; return graceful error | Backend |
| Duplicate detection false positives | Low | Low | Set safe phash distance threshold (e.g., <5) | ML |
| Team fatigue last 48h | Medium | High | Enforce break schedule & freeze | Team Lead |

---

## 9. Code Quality Baseline
- Pre-commit hooks: black / isort / flake8 (Python) OR ruff.
- Directory tests/data/ for sample docs (never commit real personal data).
- Each core service function (ocr_extract, compute_risk) unit tested with 2+ cases.
- Logging: Use structured logs (level, module, correlation_id).

LOG SAMPLE (JSON):
```
{"time":"2025-01-12T10:14:22Z","level":"INFO","event":"verification_completed","cert_uid":"JH-UNI-2024-ENG-000123","risk":12,"decision":"valid"}
```

---

## 10. Risk Engine v1 (Pseudo)
```
risk = 0
if not_found: risk += 70
if roll_mismatch: risk += 40
if name_similarity < 0.85: risk += 20
if abs(cgpa_diff) > 0.05: risk += 15
if layout_deviation > 0.25: risk += 15
if seal_confidence < 0.6: risk += 15
if phash_distance < 5: risk += 25

decision = valid (<25) / review (25–54) / suspect (>=55)
```

---

## 11. Data & Forged Sample Generation Guide
| Type | How to Forge | Expected Flag |
|------|--------------|---------------|
| CGPA Tamper | Edit number in PDF text layer or overlay | CGPA_DIFF |
| Name Variation | Change “Amit Kumar” → “Amit Kumarr” | NAME_MISMATCH (if distance high) |
| Fake ID | Random ID not in DB | RECORD_NOT_FOUND |
| Roll Swap | Swap two roll numbers between docs | ROLL_MISMATCH |
| Duplicate Scan | Re-upload same file | DUPLICATE_DOC |
| Layout Shift (Optional) | Move block positions manually in editor | LAYOUT_DEVIATION |
| Seal Removed (Optional) | Erase official seal region | SEAL_CONFIDENCE_LOW |

Rule: Put these in /tests/data/forged/ with naming pattern: forged_<type>.pdf.

---

## 12. WOW Feature Implementation Tips (Low Effort / High Impact)

1. Merkle Root Demo
   - Collect last 10 certificate hashes → sort → pairwise hash up tree → display root.
   - Show JSON:
     ```
     {
       "batch_id": "2025-01-12-01",
       "hashes": [...],
       "merkle_root": "abc123..."
     }
     ```
   - Narrate: “Anchoring this root publicly makes all included certificates tamper evident.”

2. Highlight Mismatched Fields
   - OCR returns bounding boxes.
   - For each mismatch: highlight rectangle in canvas overlay (semi-transparent red).
   - Judges visually see tampered CGPA zone.

3. Signed QR Stub
   - Data: {"uid":"JH-UNI-2024-ENG-000123","h":"abcd1234"} signed with JWT secret.
   - Scanner page decodes & calls /verify?certificate_uid to confirm.

4. Simple Outlier (Anomaly) CGPA Check
   - Compute mean & std for program session.
   - If cgpa > mean + 3*std → add flag CGPA_OUTLIER (small risk weight 10).
   - Show “This score is statistically unusual.”

---

## 13. Demo Script Roles (Live)
| Segment | Speaker | Time |
|---------|---------|------|
| Problem & Context | Team Lead | 0:00–0:40 |
| Architecture & Approach | Backend Lead | 0:40–1:20 |
| Live Ingestion (if shown) | Backend Lead | 1:20–1:50 |
| Verification (ID + Genuine Upload) | Frontend Dev | 1:50–2:40 |
| Tampered Demo + Flags | ML Engineer | 2:40–3:30 |
| WOW Feature (e.g., Merkle root) | Security | 3:30–4:00 |
| Dashboard & Stats | QA | 4:00–4:30 |
| Roadmap & Impact | Team Lead | 4:30–5:10 |
| Q&A | All | 5:10–End |

Record a backup dry-run video.

---

## 14. Slide Deck Outline (10–12 Slides)
1. Title + Team
2. Problem & Impact (stats + pain)
3. Current Challenges vs Our Solution
4. Architecture Overview
5. Core Features (MVP list)
6. Live Verification Flow Diagram
7. Demo (placeholder screenshot)
8. Risk Engine & Flags Table
9. WOW Enhancements (visual)
10. Roadmap (Phases 2–4)
11. Impact Metrics & Scalability
12. Thank You + Q&A

---

## 15. Judges’ Favorite Talking Points (Integrate Verbally)
- “We prioritized reliability over premature blockchain complexity.”
- “Hash + Merkle root today → instant path to public or permissioned anchoring.”
- “Transparent flags build trust—no black box.”
- “Architecture allows pluggable future ML modules.”
- “DPDP-aligned: minimal PII, purpose-limited fields.”

---

## 16. Testing Strategy (Novice Adaptation)
| Layer | Tests | Tools |
|-------|-------|-------|
| Unit | hash determinism, risk calculation logic | PyTest |
| Integration | OCR pipeline end-to-end with sample images | PyTest + fixtures |
| API | /verify (ID & upload), /bulk ingestion | HTTPX / requests |
| Regression | All forged docs still flagged after changes | PyTest |
| Performance | 10 sequential verifications < threshold | Simple loop timer |
| Security (basic) | Reject unsupported file types, large size | Manual + tests |

Test Command Example:
```
pytest -q --maxfail=1
```

---

## 17. Minimal File Naming Standards
| Area | Convention |
|------|------------|
| API Routers | api/verify.py, api/ingestion.py |
| Services | services/risk_engine.py |
| Tests | tests/test_<module>.py |
| Data Seeds | scripts/seed_certificates.py |
| Forged Data | tests/data/forged/forged_<type>.pdf |

---

## 18. Daily Stand-Up Template (10 Minutes)
Each member answers:
1. Yesterday I completed …
2. Today I will …
3. I’m blocked by …
4. Risks spotted …
QA logs blockers & escalates immediately.

---

## 19. Pre-Demo Checklist (Final 24h)
- [ ] All forged docs correctly flagged
- [ ] No unhandled exceptions in logs
- [ ] Offline mode: local dataset + script works
- [ ] QR scanning page tested
- [ ] Hash / Merkle root page loads fast
- [ ] Slide deck final & fonts embedded
- [ ] Backup video zipped
- [ ] Team roles rehearsed twice
- [ ] Network fallback (phone hotspot verified)

---

## 20. “If Something Breaks” Fallback Script
Scenario: OCR failing
→ Switch to verification by ID + show pre-run flagged result JSON.

Scenario: DB crash
→ Use backup JSON static dataset + mock verify endpoint (explain incident contingency in production plan).

Scenario: Time running out
→ Skip Merkle/QR section; focus on forged detection success.

---

## 21. Roadmap Summary (You Say This)
“Next, we’ll enhance structural forensics (layout embeddings), add Merkle-root chain anchoring, roll out anomaly detection on grade distributions, then issue verifiable credentials with selective disclosure—deployable across universities in a phased onboarding.”

---

## 22. Ethical & Responsible Use (Judge Bonus)
- Avoid unnecessary personal identifiers.
- Hash-based integrity prevents silent data tampering.
- Transparent explainability: Flags + Reason → no opaque AI decisions.
- Future selective disclosure to protect students’ private grade details.

---

## 23. Stretch (Only if Time Miracle)
- Simple face area detection (NOT recognition; just bounding box detection).
- Export CSV of all suspect verifications.
- Add “Confidence Slider” to adjust fuzzy threshold live.

---

## 24. Motivation / Team Culture Tips
- Celebrate each milestone (MVP ingestion, first flag, first duplicate detection).
- Keep a whiteboard of “Bugs Killed” for morale.
- Practice explaining system to a non-tech friend (ensures clarity for judges).

---

## 25. Simple Kanban Columns
To Do | In Progress | Review | Done  
Use GitHub Projects or a local board. Limit WIP ≤ 2 items per dev.

---

## 26. Final Words to Judges (Memorize)
“We focused on an integrity-first MVP: structured ingestion, OCR-based verification, deterministic hashing, transparent risk scoring—and demonstrated real forged detection. The system is production-extendable toward blockchain anchoring and verifiable credentials, aligning with a national trust mission in education.”

---

## 27. Quick FAQ Snippets You Can Reuse
Q: Why will institutions adopt this?
A: Low friction CSV ingestion + immediate fraud reduction + future interoperability (VCs).

Q: What about scaling to millions?
A: Read-heavy paths are indexed, OCR async capable, and modules decouple for microservice evolution past MVP.

Q: How do you ensure no silent tampering post-ingestion?
A: Hash recorded; mismatch between recomputed canonical JSON & stored hash = integrity alert.

---

## 28. Keep This Mindset
You aren’t just “building features”—you’re delivering a TRUST LAYER for academic credentials.

---

## 29. Immediate Next Steps (Actionable)
1. Create repo + push README & this plan.
2. Member pairing & micro-learning tasks (complete in 2 days).
3. Implement ingestion → ID verify pipeline.
4. Add upload + OCR + risk logic.
5. Forge docs & tune thresholds.
6. Layer 1–2 WOW features.
7. Rehearse + refine storytelling.
8. Freeze & polish.

---

## 30. You Are Ready
Save this file in docs/ as execution_master_plan.md. Use it daily. Check off sections as you complete them.

Go build. You’ve got a roadmap; execution + polish will differentiate you.

---