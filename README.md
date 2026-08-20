# 🚗 Company EV Home Charging Reimbursement

<p align="center">
  <img src="images/hero.png" width="900" alt="Company EV Home Charging Reimbursement">
</p>

<p align="center">
  Automatically calculate the electricity cost of charging a company EV at home using <b>ecoMain</b> and <b>Home Assistant</b>.
</p>

---

## 💡 What Does It Do?

Charging a company EV at home creates a simple problem:

**How much of the household electricity bill belongs to the company car?**

This Blueprint uses an **ecoMain circuit** to detect EV charging and automatically calculates the corresponding electricity cost.

It supports:

- ⚡ Automatic EV charging detection
- 💶 Fixed electricity prices
- 📈 Dynamic electricity prices such as Nord Pool
- 🧮 Automatic charging cost calculation
- 📅 Monthly reimbursement tracking

> **Select the charging circuit + select the electricity price → get the reimbursement amount.**

---

## 🔄 How It Works

<p align="center">
  <img src="images/how-it-works.png" width="950" alt="EV Charging Reimbursement Workflow">
</p>

ecoMain measures the power of the circuit used by the EV.

When charging is detected, the Blueprint calculates:

> **Charging cost = EV power × electricity price × time**

The cost is continuously added to the current month's reimbursement.

When charging stops, the calculation stops automatically.

---

# 🛠️ Setup

## 1. Import the Blueprint

Import the **Company EV Home Charging Reimbursement** Blueprint into Home Assistant and create an automation from it.

The Blueprint keeps the setup simple — the user mainly needs to select the charging circuit and electricity pricing method.

<p align="center">
  <img src="images/blueprint-config.png" width="850" alt="Company EV Reimbursement Blueprint Configuration">
</p>

---

## 2. Select the EV Charging Circuit

Choose the ecoMain real-time power sensor corresponding to the circuit used for EV charging.

For example:

```text
sensor.ecomain_xxx_main_ch1_power_rt
```

For the most accurate reimbursement calculation, the selected circuit should preferably be dedicated to EV charging.

---

## 3. Choose the Electricity Price

Two pricing methods are supported.

### 💶 Option A — Fixed Electricity Price

For a fixed electricity contract, select **Fixed Price** and enter the tariff manually.

For example:

```text
0.30 EUR/kWh
```

<p align="center">
  <img src="images/fixed-price.png" width="850" alt="Fixed Electricity Price Configuration">
</p>

This is the simplest option for users whose electricity price does not change throughout the day.

---

### 📈 Option B — Dynamic Electricity Price

For dynamic electricity pricing, select **Dynamic Price** and choose an electricity price sensor.

For example:

```text
sensor.nordpool_kwh_se3_eur_xxx
```

<p align="center">
  <img src="images/nordpool-price.png" width="850" alt="Nord Pool Dynamic Price Configuration">
</p>

The Blueprint automatically reads the latest price from the selected sensor while the EV is charging.

This allows the reimbursement calculation to follow actual electricity price changes instead of assuming one fixed tariff.

---

## 4. Select the Monthly Reimbursement Helper

Create one Number Helper in Home Assistant:

**Settings → Devices & services → Helpers → Create helper → Number**

Example:

```text
Name: Company EV Monthly Reimbursement
Minimum: 0
Maximum: 10000
Step: 0.01
Unit: EUR
```

Select this helper in the Blueprint configuration.

That's it.

> **Charging circuit + electricity price + reimbursement helper**

Once configured, the rest of the process is automatic.

---

# 📊 Real-World Dynamic Price Test

The automation was tested using an actual **ecoMain circuit** together with a live **Nord Pool electricity price**.

This allows us to verify the complete chain from measured power to calculated charging cost.

## ⚡ 1. ecoMain Measures the Charging Load

<p align="center">
  <img src="images/dynamic-power.png" width="950" alt="ecoMain EV Charging Power Test">
</p>

During the test, ecoMain measured a charging load of approximately **0.9 kW**.

At around 15:00, the power dropped to almost zero as charging stopped.

---

## 📈 2. Nord Pool Price Changes During Charging

<p align="center">
  <img src="images/dynamic-price.png" width="950" alt="Nord Pool Dynamic Electricity Price Test">
</p>

The electricity price changed several times during the same charging period.

The Blueprint automatically followed the current Nord Pool price without requiring manual tariff updates.

---

## 💶 3. Charging Cost Is Accumulated Automatically

<p align="center">
  <img src="images/dynamic-cost.png" width="950" alt="EV Dynamic Charging Cost Test">
</p>

The accumulated charging cost increased while the EV was charging.

When the charging power dropped to zero, the cost stopped increasing automatically.

The complete process is therefore:

> **ecoMain power → current electricity price → charging cost → reimbursement**

---

# 💰 Monthly Reimbursement

The final result is kept simple.

The reimbursement helper displays the accumulated amount for the company EV:

```text
Company EV Monthly Reimbursement
€ XX.XX
```

Instead of manually recording every charging session and calculating its electricity cost, the reimbursement amount can be viewed directly in Home Assistant.

It can also be added to a Home Assistant dashboard for quick access.

---

## ⚙️ Fixed vs Dynamic Pricing

| | Fixed Price | Dynamic Price |
|---|---|---|
| **Price source** | Manual tariff | Price sensor |
| **Price changes automatically** | ❌ | ✅ |
| **Nord Pool support** | — | ✅ |
| **Best for** | Fixed contracts | Hourly / dynamic contracts |

Both pricing methods use the **same Blueprint**.

---

## 📌 Requirements

- Home Assistant
- ecoMain energy monitoring
- A measurable EV charging circuit
- One Number Helper for monthly reimbursement
- Optional: Nord Pool or another compatible electricity price sensor

---

## ✅ Result

Once configured, the workflow is automatic:

> 🚗 **Charge at home → ecoMain measures → Home Assistant calculates → reimbursement is recorded**

The user does not need to manually record charging sessions or calculate electricity costs.

**One Blueprint. Fixed or dynamic pricing. One clear reimbursement amount.**
