# Authenticity Validator for Academia (MVP)
Problem Statement ID: SIH25029  
Theme: Smart Education (Government of Jharkhand)  

This repository contains the Minimum Viable Product (MVP) implementation plan for a system that verifies the authenticity of higher‑education certificates and flags tampered or fake documents.

---

## 1. MVP Mission Statement
Deliver a reliable, fast, and explainable end‑to‑end verification flow where:
1. Institutions (pilot) upload structured certificate records.
2. Verifiers check a certificate by ID or by uploading a scanned/PDF document.
3. System extracts key fields (OCR), matches against registry, calculates risk, and outputs VALID / REVIEW / SUSPECT with explicit flags.
4. Each certificate has a canonical JSON + SHA‑256 hash (foundation for later blockchain anchoring).

If we remove any feature below, the product stops being useful. Everything else is roadmap.

---

## 2. Core MVP Features (IN SCOPE)
| Area | Included |
|------|----------|
| Data Ingestion | Bulk CSV upload (institution records) |
| Canonical Hash | Deterministic JSON → SHA‑256 stored |
| Verification by ID | Fast path (no OCR) |
| Verification by Upload | PDF / Image → OCR → field parsing |
| Field Matching | name, roll_number, certificate_uid, cgpa |
| Risk Engine v1 | Rule-based numeric score + thresholds |
| Decision States | valid / review / suspect |
| Flags | NAME_MISMATCH, ROLL_MISMATCH, CGPA_DIFF, RECORD_NOT_FOUND |
| Basic Forensics (optional if time) | Perceptual hash → duplicate flag |
| UI | Bulk upload page, verify-by-ID, verify-by-upload, dashboard list |
| Hash Transparency | Show hash prefix in result |
| Auditability | Store verification_requests row for every attempt |
| Basic Metrics | Count of verifications, suspect rate (summary endpoint) |

---

## 3. Explicitly OUT of MVP (Roadmap Only)
- Full blockchain / Hyperledger Fabric
- W3C Verifiable Credentials / DIDs
- Advanced tamper ML (layout embeddings, deep CNN seal classifier)
- Case management workflow & alert triage UI
- Photo/face comparison
- Multi-language UI & accessibility enhancements
- Fine-grained RBAC / SSO
- Public API key issuance & quota dashboards
- Mobile app
- Real-time notifications (email/SMS)
- Complex anomaly detection (grade distribution outliers)

---

## 4. Directory Structure (Proposed)
```
/backend
  /app
    api/ (FastAPI routers)
    services/ (ocr, extraction, risk_engine, hashing)
    models/ (SQLAlchemy)
    schemas/ (Pydantic)
    db/
  tests/
/frontend
  src/
    pages/
    components/
    services/api.ts
/ml
  scripts/
  notebook/
/docs
  README.md (this file)
  demo_pitch_script.md
  roadmap.md
  judge_qa_cheatsheet.md
/infra
  docker-compose.yml
```

---

## 5. Data Entities (MVP Subset)
- institutions
- certificates
- certificate_artifacts (optional: store upload)
- verification_requests
- audit_log (lightweight)
(See docs/er-diagram.md for full detail.)

---

## 6. Canonical JSON & Hash (Example)
```json
{
  "certificate_uid": "JH-UNI-2024-ENG-000123",
  "institution_code": "JHENG001",
  "student_name": "Amit Kumar",
  "roll_number": "19ENG0234",
  "program_name": "B.Tech Mechanical",
  "session": "2019-2023",
  "issue_date": "2023-08-21",
  "cgpa": "8.14",
  "class_division": "First Division"
}
```
Hash = SHA-256 of UTF-8 serialized JSON (sorted keys, compact separators).

---

## 7. Risk Scoring (v1 Sample)
| Condition | Score Add |
|-----------|-----------|
| RECORD_NOT_FOUND | +70 |
| ROLL_MISMATCH | +40 |
| NAME_MISMATCH | +20 |
| CGPA_DIFF (>0.05) | +15 |
| DUPLICATE_DOC (phash distance < 5) | +25 |

Thresholds:
- < 25 → valid
- 25–54 → review
- ≥ 55 → suspect

---

## 8. API (Initial)
| Method | Endpoint | Purpose |
|--------|----------|---------|
| POST | /institutions/{id}/certificates/bulk | CSV ingestion |
| GET | /verify?certificate_uid=... | Quick verify by ID |
| POST | /verify/upload | File upload verify |
| GET | /dashboard/summary | Stats |
| GET | /dashboard/recent | Recent verifications |

---

## 9. Run Instructions (Dev)
Prereqs: Docker, Docker Compose, Node 18+, Python 3.11

1. Clone repo.
2. Copy .env.example → .env (fill DB creds).
3. Start stack:
   ```
   docker compose up --build
   ```
4. Seed sample data:
   ```
   python scripts/seed_institution.py
   python scripts/seed_certificates.py
   ```
5. Frontend:
   ```
   cd frontend
   npm install
   npm run dev
   ```
6. Open http://localhost:5173

---

## 10. Demo Checklist
- Bulk upload 20 sample certs (or already seeded).
- Verify genuine certificate by ID → valid.
- Upload genuine PDF → valid (risk < threshold).
- Upload tampered CGPA PDF → cgpa_diff flag + review/suspect.
- Upload unknown ID → record_not_found suspect.
- Show hash prefix & explain future blockchain anchor.
- Show recent list and suspect count.

---

## 11. Metrics (Log or Endpoint)
- totalCertificates
- totalVerifications
- suspectRate
- averageVerificationMs (optional)
- flag frequency (optional daily aggregate)

---

## 12. Quality Gates
- Unit tests: ingestion, canonical hash stability, risk scoring
- Lint: ruff / eslint
- Basic OCR failure fallback (return readable error)
- Deterministic output for same input (idempotent verification by ID)

---

## 13. Security (MVP Light)
- Simple API token or hard-coded admin token (upgrade later)
- Validate file types & size
- Sanitize all OCR text before logging
- No PII exposure beyond verification context

---

## 14. Roadmap Preview (High-Level)
(Full details in docs/roadmap.md)
Phase 2: Layout forensics, seal/logo feature matching, duplicate detection scaling.  
Phase 3: Anchoring (Merkle root + public chain), anomaly detection, alert workflow.  
Phase 4: Verifiable Credentials, multi-institution federation, advanced analytics.

---

## 15. Contributing (Short)
1. Branch naming: feature/<name>, fix/<name>
2. PR includes: description, test evidence
3. Run test + lint before pushing

---

## 16. Licensing & Compliance
(Placeholder) Decide between Apache-2.0 / MIT if open-sourcing.  
Ensure alignment with DPDP (Data minimization, lawful purpose).

---

## 17. Quick Pitch (One-Liner)
“Upload a certificate or enter its ID and in seconds get an integrity‑scored authenticity decision backed by cryptographic hashing and structured institutional data.”

---

## 18. Contact / Team Roles (Example Template)
| Role | Owner |
|------|-------|
| Backend / APIs | Person A |
| OCR & Extraction | Person B |
| Risk & Forensics | Person C |
| Frontend UI/UX | Person D |
| DevOps & Infra | Person E |
| Data Preparation | Person F |

---

## 19. Fast Rehearsal Script (30‑Second)
“We imported 20 real sample certificates. This certificate’s JSON hash is anchored locally. Now we’ll verify three cases: valid ID, tampered CGPA, and fake ID. The system extracts fields, compares with registry, assigns a risk score, and flags suspicious mismatches. This core engine is production‑ready for expansion into blockchain anchoring and AI-based tamper detection.”

---

## 20. Status Badges (Add later if CI)
(Placeholder for GitHub Actions: build, test coverage.)

---

## 21. FAQ (Short)
Q: Why no blockchain yet?  
A: We start with reproducible hashes; anchoring is trivial later—focus is adoption.

Q: Can marks be hidden?  
A: Future feature: selective disclosure / zero-knowledge style proofs.

Q: How to scale to multiple institutions?  
A: Institution_code isolation + future multi-tenant auth.

---

## 22. Glossary
- Canonical JSON: Deterministic serialized representation used for hashing.
- Perceptual Hash: Looks-like similarity hash for detecting duplicate scans.
- Risk Score: Weighted rule-based fraud likelihood indicator (not a legal decision).

---

## 23. Acknowledgements
Problem statement by Government of Jharkhand – Higher & Technical Education.

---

## 24. Next Action
Team: Implement ingestion → verify-by-ID → file upload OCR → risk engine → UI result panes → finalize demo.

---
