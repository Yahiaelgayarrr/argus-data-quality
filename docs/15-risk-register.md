# Risk Register

| ID | Risk | Likelihood | Impact | Mitigation | Trigger | Owner/Status |
| --- | --- | --- | --- | --- | --- | --- |
| RISK-001 | CadetX does not approve solo participation. | Medium | High | Ask CadetX early; adapt evidence to solo work if approved. | CadetX requires team formation. | User / OPEN |
| RISK-002 | Dataset is unsuitable for required modules. | Medium | High | Compare candidates before choosing; use hybrid corruption strategy. | Dataset lacks semantic fields or quality issues. | User+Agent / OPEN |
| RISK-003 | Evaluation lacks ground truth. | High | High | Inject controlled corruptions with labels. | Metrics cannot be computed credibly. | Agent / OPEN |
| RISK-004 | Project becomes overengineered. | Medium | Medium | Keep requirements traceable; defer optional features. | Complex framework appears before core modules work. | Agent / OPEN |
| RISK-005 | ML models do not outperform simple baselines. | Medium | Medium | Evaluate baselines and report limitations honestly. | ML metrics weak or unstable. | Agent / OPEN |
| RISK-006 | Data leakage affects evaluation. | Medium | High | Separate ground truth labels from model fitting; document methodology. | Config/model uses injected labels improperly. | Agent / OPEN |
| RISK-007 | PII leaks into repo or logs. | Medium | High | Use synthetic PII; redact report/log examples. | Real personal values appear in data/logs. | Agent / OPEN |
| RISK-008 | Module interfaces drift. | Medium | High | Define schemas/contracts before integration. | Downstream module breaks after upstream changes. | Agent / OPEN |
| RISK-009 | Documentation drift. | High | Medium | Update docs in same work unit as behavior changes. | Docs contradict code or status. | Agent / OPEN |
| RISK-010 | Integration fails late. | Medium | High | Add module integration tests early. | Modules work alone but not together. | Agent / OPEN |
| RISK-011 | Docker/environment differences. | Medium | Medium | Add Docker smoke tests and pin dependencies. | Local works but container fails. | Agent / OPEN |
| RISK-012 | Time overrun. | Medium | High | Use 12-week roadmap with buffers and strict scope control. | Major module slips by more than one week. | User+Agent / OPEN |

