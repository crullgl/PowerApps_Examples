# 🧭 Problem Decision Tool

![Decision Tool UI](images/Screenshot%202025-05-08%20155534.png)

**The Problem Decision Tool** is a Power Platform-based triage app that helps users categorize problems and determine the appropriate level of intervention. Whether an issue is best suited for a `Just Do It`, `RPIW`, `Yellow Belt`, or `Green Belt` project, this app guides users through a consistent, structured decision-making process.

---

## 🔍 Purpose

- Quickly assess the scale and complexity of a problem  
- Reduce ambiguity when deciding whether to escalate an issue  
- Align problems with appropriate Lean or PI tools
- Does not require a datasource but can add one for tracking. 

---

## ⚙️ Features

- Dynamic questions based on selected issue type  
- Built-in logic that evaluates scope, risk, urgency, and control  
- Recommends a decision path (e.g., Yellow Belt or Green Belt)  
- Allows saving/exporting of decisions  
- Works on both mobile and desktop  

---

## 📊 Decision Logic

- Is the root cause known?  
- Is it isolated to one area or cross-functional?  
- Does it involve safety, compliance, or major impact?  
- How much effort would it take to address?  

**Possible outcomes:**
- 🛠️ **Just Do It**
- 🟡 **Yellow Belt**
- 🚀 **RPIW**
- 🟢 **Green Belt**

---

## 📈 ROI & Impact

- 5 unnecessary RPIW requests avoided per year  
- 10 employees × 40 hours = **2,000 labor hours saved**  
- Estimated savings per facility: **$480,000/year**  
- App cost to build: **$2,040** (40 hours × 2 developers)  
- **ROI: 23,429%**  
- Org-wide potential savings: **$22M+ in unrealized value**

![ROI Summary](images/ROI-Screenshot.png)

---

## 🧠 Tech Stack

- Power Apps Canvas App  
- SharePoint or Dataverse backend  
- Logic via `Switch()`, `If()`, `Patch()`, and collections  
---
## ✅Other suggested addons for the app
- add a list that dynamically tracks ROI based on final selections
  
---



