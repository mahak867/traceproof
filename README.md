# TraceProof

**Explainable, evidence-verified AI for mule-network investigation and suspicious transaction reporting**

> Entry for the **GIFT FutureLab Grand Challenge 2026-27** (GIFT City · GIFT-IFI · IIT Gandhinagar)
> Track: Cybersecurity, fraud prevention and risk management
> Status: 🟡 **Early-stage concept.** Architecture designed, proof of concept starting.

---

## The problem

India lost an estimated **₹22,495 crore** to cyber fraud in 2025, mostly through investment scams. The stolen money is laundered through networks of **mule accounts**, and banks have reported **24.67 lakh** of them to authorities.

Detection has improved: tools such as RBI's MuleHunter.AI now flag suspicious accounts at scale. The bottleneck has moved to **investigation**. For every alert, an analyst must manually trace a multi-hop network and write a suspicious transaction report (STR) for FIU-IND, within PMLA deadlines. Meanwhile the money keeps moving, often abroad.

## What TraceProof does

```
Alert ──► [1] Detect ──► [2] Explain ──► [3] Draft with proof ──► [4] Govern ──► Filed STR
           Graph NN       Counterfactual    On-site LLM + claim       Analyst sign-off,
           (Multi-GNN)    subgraph          checker (every fact       audit log, kill switch,
                                            must cite evidence)       regulator replay
```

1. **Detect.** Accounts and transfers are modelled as a graph. A graph neural network, built on IBM's open-source [Multi-GNN](https://github.com/IBM/Multi-GNN), scores laundering patterns: fan-in, fan-out, rapid pass-through and cycles.
2. **Explain.** It isolates the transfers that drive each alert and gives a counterfactual, for example *"without these 3 transfers, risk drops from 0.91 to 0.12."*
3. **Draft with proof.** A language model running on the bank's own servers drafts the STR. It may only state facts that carry an evidence ID. A deterministic checker verifies every ID, amount and date, and rejects anything it cannot prove.
4. **Govern.** An analyst must approve every report. All outputs and decisions go to a hash-chained, tamper-evident audit log, and a kill switch can disable the AI instantly.

The design is aligned to RBI's FREE-AI principles and runs fully on-premise, so customer data never leaves the institution.

## What is distinctive

| | Detection tools (MuleHunter.AI, Signzy, Data Sutram) | GenAI report writers (overseas vendors) | **TraceProof** |
|---|---|---|---|
| Flags suspicious accounts | ✅ | ➖ | ✅ (works alongside existing detection) |
| Drafts the STR | ❌ | ✅ | ✅ |
| Every claim verified against evidence | ❌ | ❌ / unpublished | ✅ **measured unsupported-claim rate** |
| Counterfactual explanation | ❌ | ❌ | ✅ |
| Runs fully on-site | varies | mostly cloud | ✅ |
| Indian regulatory fit (FIU-IND, IFSCA, FREE-AI) | partial | ❌ | ✅ |

## How we will measure it

| Metric | Target |
|---|---|
| Time to draft an STR | at least 5x faster than the manual baseline from practitioner interviews |
| Unsupported-claim rate in generated STRs | under 2% with the checker |
| Detection recall at a fixed alert budget | match or beat the published Multi-GNN baseline |
| Robustness to splitting funds into small amounts or adding hops | under 10% recall drop |

These are targets. Results will be published here as they are measured.

## Data

- **IBM AMLworld:** synthetic bank transactions with laundering labels.
- **Elliptic / Elliptic2:** real, labelled Bitcoin transaction data, used for crypto off-ramps.
- **STR-Bench India (planned):** an open benchmark of synthetic investigation cases with gold-standard STRs.

No real customer data is used or stored in this repository.

## Planned stack

Python · PyTorch Geometric · FastAPI · open-weights LLM on-site (Qwen / Llama via Ollama or vLLM) · React and Cytoscape.js · Docker Compose · optional post-quantum signatures via liboqs.

## Roadmap

- [ ] Reproduce the Multi-GNN baseline on AMLworld
- [ ] Add a cross-border exit pattern and Elliptic data
- [ ] Explainer and counterfactual engine
- [ ] Evidence-constrained STR generator and claim checker
- [ ] Analyst dashboard and regulator replay view
- [ ] Hash-chained audit log and kill switch
- [ ] STR-Bench India v1 release
- [ ] Practitioner validation interviews
- [ ] IFSCA FinTech sandbox application and bank pilot

## Team

| Name | Role | Institution |
|---|---|---|
| **Mahak Fahad** (Team Lead) | Backend + AI/ML | Chennai Institute of Technology, B.E. CSE (2029) |
| **Roshini R** | Frontend + Design / UI-UX | Chennai Institute of Technology, B.E. CSE (2029) |

## License

To be decided. Apache-2.0 is planned, to stay compatible with the reused open-source components.
