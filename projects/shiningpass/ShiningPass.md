# ShiningPass — Architecture Interior Platform

[English](ShiningPass.md) · [한국어](ShiningPass_ko.md) · [日本語](ShiningPass_ja.md)

---

## In one line

Built the official site with **Python + AWS** (backend + frontend),  
then a **UE5 client** that only **authenticated customers** can enter.

On the web: acquire and manage clients.  
In the client: swap **materials and furniture** inside architectural interiors in real time.

**Official site:** [https://shiningpass.com](https://shiningpass.com)

---

## Why this project

In architecture / interior sales and review,  
“what does this finish look like *in the room*?”  
is hard to answer with drawings alone.

ShiningPass closes that gap with one pipeline:

1. **Web** — brand, acquisition, accounts  
2. **UE5 client** — near-real space walkthrough and material swaps  

---

## System outline

```text
Official website (frontend)
        │
        ▼
Python backend  ·  AWS
  · accounts / customer auth
  · user dashboard
  · session / access control
        │
        ▼
UE5 client (authenticated customers only)
  · explore spaces
  · change materials · furniture
  · multi-material preview UI
```

After onboarding a customer company,  
only **verified accounts** can open the UE5 client.

---

## Web · backend

| Area | Work |
|:-----|:-----|
| **Official site** | [shiningpass.com](https://shiningpass.com) · frontend · service entry flow |
| **Backend** | Python APIs · business logic |
| **Infra** | Deployed / integrated on **AWS** |
| **Dashboard** | User (customer) admin dashboard |
| **Access** | Client entry limited to authenticated companies |

### One account · one session

> [!TIP]
> This is a security feature, not a paywall — it kills shared-password abuse and multi-seat use on a single license.

Customer logins are limited to **one concurrent session per account**.

If the same ID signs in on another PC (or another session),  
the previous session is **logged out automatically**.

Built to reduce shared-password abuse and multi-seat use on a single license.

---

## UE5 client

Users walk architectural interiors and  
**change materials, furniture, and finishes** while seeing results immediately.

My focus areas:

### Multi-material preview (UI)

The UI shows **many material swatches at once** —  
categories (wood / marble / solid / fabric), search, design reset —  
pick one and apply it into the space.

Lots of swatches mean GPU and memory pressure.  
Preview tiles are **not spawned from scratch every time**;  
they use **object pooling**:

- Only the visible count is active  
- On scroll / filter change, slots are borrowed from the pool and returned  
- Stops widget/texture instance explosion and keeps **memory & frame** stable  

This was the path where **hardware memory and runtime cost** mattered most.

### Apply to level actors on click

Choosing a material in the UI does not stop at a thumbnail.  
It applies the material to the **matching actors in the level**  
(walls, floors, furniture, etc.).

Selection in UI → material set on the actor —  
so “what you picked” is “what the room shows.”

### Render · lighting

- **DLSS** for upscaled performance  
- **Lighting setup optimization** in UE5 for interior quality vs. frame rate  

---

## Stack

| | |
|:--|:--|
| Client | **Unreal Engine 5**, **C++** |
| Server · web | **Python**, frontend |
| Cloud | **AWS** |
| Other | Session control, dashboard, Git, etc. |

---

## Screenshots

### 0. Official homepage

<p align="center">
  <img src="assets/screenshots/shot-00-homepage.png" width="900" alt="ShiningPass official homepage" />
</p>

**Caption**

Landing page at [shiningpass.com](https://shiningpass.com).

Top **SHINING PASS** nav (About Us · Support · Login · KO/EN),  
hero line **AI 3D Automatic Generator**,  
and Windows / Mac client download buttons.

This is the **official web frontend** entry — product intro, acquisition, and client download.

### 1. Guest Bath — flooring picker

<p align="center">
  <img src="assets/screenshots/shot-01.png" width="900" alt="Guest Bath flooring material UI" />
</p>

**Caption**

Guest bath space. Top bar switches **Flooring / Wall**;  
**Wood · Marble · Solid · Fabric** filters and search narrow the list.

Left grid shows multiple `N500`-series swatches;  
the vanity and floor on the right reflect the selected tone (e.g. N501).

Current-selection label, **Design Reset**, and Rooms / Reset / Exit  
complete the pick → see → undo loop on one screen.

That grid is the **multi-material preview**;  
as the catalog grows, swatch slots are recycled via **object pooling**.

### 2. Marble category · bedroom tone

<p align="center">
  <img src="assets/screenshots/shot-02.png" width="900" alt="Marble swatches in a classic bedroom" />
</p>

**Caption**

Same material UI with the **Marble** tab open.  
Swatches such as `N824`–`N835`, search, and Design Reset stay in place.

Background: gold moulding, fabric wall, classic bedroom.  
Bottom-right shows the **active material name** (e.g. `Breccia_marble`).

Changing category reuses the grid slots;  
a click still pushes the material onto the level actors.

### 3. Living room — Material change system

<p align="center">
  <img src="assets/screenshots/shot-03.png" width="900" alt="Living room wall material UI" />
</p>

**Caption**

Living / TV-wall space. Room tabs (entry, living, laundry, pantry, dressing, balcony)  
plus **flooring / wall** and wood · marble · solid · fabric filters.

Left panel: `N948-Full` … `N959-Full` swatches in a grid;  
the large wall on the right takes the selected tone.

The **Material change system** label underlines the product core:  
list preview + live space update.

Room, surface, and category switches share one UI pipeline,  
so pooling and apply logic stay one system.

---

## Summary

| | |
|:--|:--|
| Web | Official site + acquisition · auth |
| Server | Python · AWS · dashboard · **1 account / 1 session** |
| Client | UE5 interior walkthrough · material / furniture change |
| My focus | Multi-material preview (**object pooling**), actor apply, DLSS · lighting |

---

## Disclosure limit

> [!IMPORTANT]
> Dashboard captures, internal APIs, and infra details stay private under NDA — only what's shown here is public.

Everything on this page — overview, architecture notes, and in-client screenshots — is what can be shown **publicly**.

Extra video, admin / user **dashboard** captures, and internal API or infra details  
cannot be shared further because of **security and NDA / confidentiality contracts**.
