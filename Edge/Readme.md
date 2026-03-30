# 🚀 Deployment Architecture

---

## 🖥️ Workstation

> ⚠️ **Only StatefulSet spawning** — no Deployment option on Hub or Edge. No configurations page on either.

### 🟣 On HUB
**Resources:** `StatefulSet` · `Service`

| # | Detail |
|---|---|
| Spawn type | StatefulSet only — no Deployment option |
| PVC | Defined inside StatefulSet spec |
| PV | ✅ Auto-created via `cstor-csi` storage class |
| Bucket mounting | ✅ Available |
| Configurations page | ❌ Not available |

### 🟠 On Edge
**Resources:** `StatefulSet` · `PersistentVolume` · `Service`

| # | Detail |
|---|---|
| Spawn type | StatefulSet only — no Deployment option |
| PVC | Defined inside StatefulSet spec |
| PV | ⚠️ Must be created explicitly — not auto-provisioned |
| Bucket mounting | ❌ Not available |
| Configurations page | ❌ Not available |

---

## ⚙️ Microservice

### 🟣 On HUB — Deployment
**Resources:** `Deployment` · `PersistentVolumeClaim` · `Service`

| # | Detail |
|---|---|
| Spawn type | ✅ Deployment *(default)* — StatefulSet option also available |
| PVC | Created separately alongside Deployment |
| PV | ✅ Auto-created via `cstor-csi` storage class |
| Bucket mounting | ✅ Available |

---

### 🟣 On HUB — StatefulSet without Storage
**Resources:** `StatefulSet` · `Service`

| # | Detail |
|---|---|
| Spawn type | StatefulSet |
| PVC | ❌ Not included in StatefulSet spec |
| PV | ❌ Not created |
| Storage option in UI | Available but not used |

---

### 🟣 On HUB — StatefulSet with Storage
**Resources:** `StatefulSet` · `Service`

| # | Detail |
|---|---|
| Spawn type | StatefulSet |
| PVC | ✅ Defined inside StatefulSet spec |
| PV | ✅ Auto-created by storage class |
| Storage option in UI | Available and used |

---

### 🟠 On Edge — Deployment without Storage
**Resources:** `Deployment` · `Service`

| # | Detail |
|---|---|
| Spawn type | Deployment *(default)* |
| PVC | ❌ Not created |
| PV | ❌ Not created |

---

### 🟠 On Edge — Deployment with Storage
**Resources:** `Deployment` · `PersistentVolumeClaim` · `PersistentVolume` · `Service`

| # | Detail |
|---|---|
| Spawn type | Deployment |
| PVC | ✅ Created explicitly |
| PV | ⚠️ Created explicitly — not auto-provisioned |

---

## 📊 Quick Reference

| Scenario | Deploy | SS | PVC | PV | Svc | Auto PV |
|---|:---:|:---:|:---:|:---:|:---:|:---:|
| 🟣 Workstation — Hub | | ✅ | ✅ | | ✅ | ✅ |
| 🟠 Workstation — Edge | | ✅ | ✅ | ✅ | ✅ | ❌ |
| 🟣 Microservice — Hub Deployment | ✅ | | ✅ | | ✅ | ✅ |
| 🟣 Microservice — Hub SS no storage | | ✅ | | | ✅ | — |
| 🟣 Microservice — Hub SS with storage | | ✅ | ✅ | | ✅ | ✅ |
| 🟠 Microservice — Edge Deployment no storage | ✅ | | | | ✅ | — |
| 🟠 Microservice — Edge Deployment with storage | ✅ | | ✅ | ✅ | ✅ | ❌ |

> 🟣 **Hub** — PV always auto-provisioned via storage class &nbsp;|&nbsp; 🟠 **Edge** — PV always created manually
