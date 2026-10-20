## SECTION 1 : PROJECT TITLE
## DC-GUARDIAN: Multi-Domain Reasoning for Data-Center Security, Safety and Operations

Practice Module Group 22 · Graduate Certificate in Intelligent Reasoning Systems, NUS-ISS

<img src="SystemCode/logo/dc-guardian-mark-256.png" alt="DC-GUARDIAN" width="160" />

---

## SECTION 2 : EXECUTIVE SUMMARY / PAPER ABSTRACT

Data-centre operations teams watch separate tools for cyber security, physical access, worker safety, equipment health and room conditions. Each tool raises its own alerts, so an incident that spans several domains reaches the operator as unrelated fragments, and real incidents are buried among false alarms.

DC-GUARDIAN is an intelligent reasoning system that combines these domains. Five detectors produce Evidence:
- SSH log anomalies (rules plus Isolation Forest and an autoencoder);
- PPE compliance in camera images (YOLOv8);
- face recognition with zone authorisation (ArcFace);
- 7-day drive-failure risk (a temporal Random Forest);
- room temperature and humidity thresholds.

The Evidence is mapped onto a Neo4j knowledge graph of a synthetic data centre (zones, racks, servers, sensors, cameras, staff and their authorised zones). Deterministic rules then correlate results that share place and time, and a Bayesian network estimates how likely two results are to be related. Severity comes only from fixed, versioned Decision rules: a group's severity is its most severe member, raised one level only when the members show a known mechanism.

Specialist agents and an AI Investigator explain the Evidence using retrieval over approved public guidance (NIST, CISA, OSHA, U.S. DOE). They are grounded and citation-checked, with a post-generation check for unsupported claims, and they never assign severity or act. Every result goes to human review, with audited verdicts and an investigation workflow.

**Experiments** (24 held-out labelled scenarios; detectors on locked public test sets):
- **Against isolated single-domain monitoring:** correlation cuts the alerts an operator sees from 65 to 44 (−21, 95% CI −29 to −14) and shows 14 of 18 incidents whole in one alert (0 of 18 when isolated). Every HIGH alert is a real incident, and the review queue's average precision rises from 0.73 to 0.81 (+0.08, 95% CI +0.02 to +0.19).
- **Bayesian correlation:** F1 0.86 against 0.78 for the crisp rule. The interval includes zero, so the Bayesian score is shown for information only and does not drive decisions.
- **Detectors and grounding:** PPE detection was improved and tested on two further datasets. Retrieval Recall@3 is 96.1%. A post-generation judge caught 4 of 5 overclaims.
- **Speed:** persistent inference workers cut uploads from 7–18 s to 0.6–3.1 s.
- **Usability:** a study with participants outside the team. _(results to add)_
- **Limitations:** the scenarios and topology are synthetic, and the labels come from the project team; a ChatGPT labelling of the same blind pack was used only as a sensitivity check. The system has not been deployed on real facility data.

The system runs on one workstation with a React dashboard and a FastAPI backend, and does not need a GPU.

---

## SECTION 3 : CREDITS / PROJECT CONTRIBUTION

| Official Full Name | Student ID (Matriculation) | Work Items (Who Did What) | Email (Optional) |
| :------------ |:---------------:| :-----| :-----|
| AL AJMI ALJAWHARAH MOHAMMAD H | A0356913X | _(contributions)_ | |
| ALOTAIBI MARAM MUGAAD M | A0355452A | _(contributions)_ | |
| ALSHAKHS MAI ZAID M | A0354315J | _(contributions)_ | |
| ALSHAMMARY NORAH MOHAMMED H | A0354356Y | _(contributions)_ | |

---

## SECTION 4 : VIDEO OF SYSTEM MODELLING & USE CASE DEMO

- Business video (promotion, business value, use case, pricing): _(link to `Video/` file or YouTube)_
- Technical video (system design, technical explanation of use cases): _(link)_

---

## SECTION 5 : USER GUIDE

Full guide: [SystemCode/docs/INSTALLATION_AND_USER_GUIDE.md](SystemCode/docs/INSTALLATION_AND_USER_GUIDE.md), also the report appendix.

**Requirements:**
- Windows 10/11 (tested on Windows 11), 16 GB RAM, 15 GB free disk;
- Git with **Git LFS**, Python 3.12, Node.js 20+;
- Neo4j (tested with 2026.08, for example in Neo4j Desktop);
- optional: an NVIDIA GPU (faster PPE checks), an OpenAI API key (AI Investigator and specialists). No GPU is needed.

**Install and run.** Use `git clone`, not "Download ZIP": the ZIP leaves out the 318 MB drive-failure model stored in Git LFS.

```powershell
git lfs install
git clone https://github.com/Jowharah/IRS-PM-2026-10-25-AIS-GRP22-DC-Guardian-System.git
cd IRS-PM-2026-10-25-AIS-GRP22-DC-Guardian-System\SystemCode
git lfs pull
powershell -ExecutionPolicy Bypass -File setup.ps1     # asks for your Neo4j password and a dashboard password

# then, in two terminals (inside SystemCode):
.venv-dc-guardian\Scripts\python.exe -m uvicorn presentation.backend.app.main:app --host 127.0.0.1 --port 8000
cd presentation\frontend; npm run dev                    # open http://localhost:5173, user: operator
```

Setup loads demo data, so the dashboard shows results straight away. Use **Scenario Lab** to add your own camera images, SSH logs, drive histories or sensor readings.

---

## SECTION 6 : PROJECT REPORT / PAPER

[ProjectReport/](ProjectReport/): group report (business case and market research, system design, implementation, experiments, findings and discussion). Its appendices are the project proposal, the mapping of system functions to MR / RS / CGS course techniques, and the installation and user guide.

**Technique groups demonstrated (three or more required):**

| Technique group | In DC-GUARDIAN |
|---|---|
| Decision automation: business rules and knowledge-based reasoning | Versioned deterministic Decision rules (DCG-DECISION-v1/v2); investigation workflow; correlation rules over the knowledge graph |
| Knowledge discovery and data mining | Five ML detectors (YOLOv8 PPE, ArcFace, Isolation Forest + autoencoder SSH, temporal Random Forest drives); Bayesian correlation network; RAG retrieval |
| Cognitive systems | Neo4j knowledge graph of the data centre; computer vision (PPE, face); AI Investigator and specialist agents answering in natural language with citations |

---

## SECTION 7 : MISCELLANEOUS

- [Miscellaneous/](Miscellaneous/): pointers to the usability study kit, datasets and citations, the Bayesian study and the evaluation.
- Full development history (pull requests, CI, work log): <https://github.com/Jowharah/DC-Guardian>. `SystemCode/` is a snapshot of that repository's `main` branch at commit `97f9f25`.
- Datasets and licences: [SystemCode/docs/DATASETS_AND_CITATIONS.md](SystemCode/docs/DATASETS_AND_CITATIONS.md). Academic, non-commercial use; PPE models trained on SH17 (CC BY-NC-SA 4.0).

---

**This [Machine Reasoning (MR)](https://www.iss.nus.edu.sg/executive-education/course/detail/machine-reasoning "Machine Reasoning") course is part of the Analytics and Intelligent Systems and Graduate Certificate in [Intelligent Reasoning Systems (IRS)](https://www.iss.nus.edu.sg/stackable-certificate-programmes/intelligent-systems "Intelligent Reasoning Systems") series offered by [NUS-ISS](https://www.iss.nus.edu.sg "Institute of Systems Science, National University of Singapore").**

**Lecturer: [GU Zhan (Sam)](https://www.iss.nus.edu.sg/about-us/staff/detail/201/GU%20Zhan "GU Zhan (Sam)")**

[![alt text](https://www.iss.nus.edu.sg/images/default-source/About-Us/7.6.1-teaching-staff/sam-website.tmb-.png "Let's check Sam' profile page")](https://www.iss.nus.edu.sg/about-us/staff/detail/201/GU%20Zhan)

**zhan.gu@nus.edu.sg**
