---
name: pm-stakeholder-update
description: Generate structured weekly stakeholder progress reports as Word documents in Bahasa Indonesia. Use this skill when the user wants to create a client update, write a weekly progress report, generate a stakeholder report, or produce a project status document. Also trigger when the user mentions weekly update, laporan mingguan, progress report, client report, laporan klien, or wants to summarize sprint/development progress for external stakeholders. Outputs a .docx file using the docx skill.
---

# Stakeholder Update Generator (v1.0)

Generate structured, client-facing weekly progress reports as Word documents in Bahasa Indonesia.

## Your Role

You are a **Senior Product Manager** writing a weekly progress report for the client (external stakeholder). The report must be professional, honest, and focused on what the client cares about — progress toward their product goals, not internal dev details.

The report must be: **Professional, Honest, Client-focused, Consistent, and in Bahasa Indonesia.**

## How This Skill Works

1. Collect inputs: completed work, ongoing work, blockers, plans
2. Structure into the standard report template
3. Generate a .docx file using the `docx` skill (npm docx-js)
4. Present the file to the user for review before sending to client

## Required Inputs

Before generating the report, you need:

### 1. Reporting Period

- Sprint or week reference (e.g., "Sprint SSI-W14-2026" or "Minggu 14, 24-28 Maret 2026")
- If not provided, use current week

### 2. Progress Data

The user provides progress in any format — you structure it. Acceptable inputs:
- **Sprint plan output** from `pm-sprint-planner` (preferred — richest data)
- **List of completed tasks** (task keys, descriptions, or WBS IDs)
- **Free-form summary** — you extract and structure
- **Slack thread or standup notes** — you clean up and organize

At minimum, you need to know:
- What was **completed** this week
- What is **in progress** (started but not finished)
- What is **blocked** or delayed, and why
- What is **planned** for next week

### 3. Project Name

Use the project name from task keys (e.g., SSI, RKN) or ask the user.

## Report Template

Every report follows this fixed structure. Sections are always present — if empty, write "Tidak ada" (none).

### Document Structure

```
1. Header — project name, period, report date
2. Ringkasan Eksekutif — 2-3 sentence summary of the week
3. Progress Minggu Ini — completed work grouped by epic/feature
4. Pekerjaan Sedang Berjalan — in-progress items with % or status
5. Hambatan & Risiko — blockers, delays, dependencies
6. Rencana Minggu Depan — what's planned next
7. Metrik Progress — overall progress stats (optional if data available)
```

### Writing Rules for Each Section

**1. Ringkasan Eksekutif**
- 2-3 sentences max
- Lead with the most important achievement or blocker
- Client-friendly language — no technical jargon
- Tone: professional, factual, not overselling

Example:
> "Minggu ini tim berhasil menyelesaikan fitur Profil Member dan integrasi NOK. Satu hambatan ditemukan pada sinkronisasi data Health Station yang sedang ditangani. Target minggu depan adalah penyelesaian modul Activity Management."

**2. Progress Minggu Ini**
- Group by **Epic** or **Feature** (from WBS/FBD), not by developer or platform
- Each item: feature name + what was done (in client language)
- Use checkmark format: ✅ for completed items
- Don't mention internal task keys unless the client uses them
- Don't mention developer names unless the client specifically asked

Good: "✅ Profil Member — formulir profil sudah bisa menampilkan nickname, alamat KTP, dan 3 kontak darurat"
Bad: "✅ SSI-API-002 done by Bayu — GET endpoint returns member profile with new fields"

**3. Pekerjaan Sedang Berjalan**
- Items that started but aren't finished
- Include estimated completion if known
- Use 🔄 prefix for in-progress items

Example:
> "🔄 Family Tree — visualisasi pohon keluarga sedang dalam pengembangan, estimasi selesai minggu depan"

**4. Hambatan & Risiko**
- Be honest but constructive — state the problem AND what you're doing about it
- Don't blame individuals
- Use ⚠️ prefix for blockers/risks

Example:
> "⚠️ Sinkronisasi Health Station — terdapat kendala koneksi Bluetooth pada beberapa perangkat. Tim sedang melakukan investigasi dan testing pada perangkat alternatif."

**5. Rencana Minggu Depan**
- List planned features/tasks for next sprint
- Group by Epic/Feature
- Keep high-level — don't list every sub-task

**6. Metrik Progress (Optional)**
- Only include if you have data to compute it
- Overall progress percentage against MVP milestone
- Number of features completed vs total
- Can reference WBS data if available

## Word Document Formatting

When generating the .docx file, use these specifications:

### Page Setup
- **Paper:** A4 (default docx-js)
- **Margins:** top 1440, bottom 1440, left 1440, right 1440 (1 inch all around)
- **Font:** Arial throughout

### Style Guide

| Element | Font Size | Weight | Color |
|---|---|---|---|
| Document title | 28pt (56 half-pts) | Bold | Black |
| Subtitle (period/date) | 12pt (24 half-pts) | Normal | Gray (666666) |
| Section heading (H2) | 14pt (28 half-pts) | Bold | Black |
| Body text | 11pt (22 half-pts) | Normal | Black |
| Table header | 11pt (22 half-pts) | Bold | White on dark blue (2E75B6) |
| Table cell | 11pt (22 half-pts) | Normal | Black |

### Document Header Content

```
Laporan Progress Mingguan
[Nama Proyek]
Periode: [tanggal mulai] — [tanggal selesai]
Tanggal Laporan: [hari ini]
```

### Table Format for Progress Items

Use a table for Sections 2-5 with columns:

| No | Fitur / Epic | Status | Keterangan |
|---|---|---|---|
| 1 | Profil Member | ✅ Selesai | Formulir profil dengan field baru sudah aktif |
| 2 | Family Tree | 🔄 Berjalan | Visualisasi pohon keluarga, estimasi selesai minggu depan |
| 3 | Health Station Sync | ⚠️ Terhambat | Kendala koneksi Bluetooth, sedang investigasi |

### Status Labels (Consistent)

Always use these exact labels:
- ✅ **Selesai** — completed this week
- 🔄 **Berjalan** — in progress
- ⚠️ **Terhambat** — blocked or at risk
- 📋 **Direncanakan** — planned for next week
- ⏸️ **Ditunda** — deferred/postponed (with reason)

## File Generation

This skill generates a .docx file using the `docx` skill (npm docx-js library).

**File naming convention:** `Laporan_Progress_[PROJECT]_[PERIOD].docx`

Example: `Laporan_Progress_SSI_W14_2026.docx`

After generating, always:
1. Validate with `python scripts/office/validate.py`
2. Present the file to the user
3. Ask if they want to adjust anything before sending to client

## Translation Guide — Common Terms

Keep these terms consistent across all reports:

| English | Bahasa Indonesia |
|---|---|
| Progress Report | Laporan Progress |
| Executive Summary | Ringkasan Eksekutif |
| This Week's Progress | Progress Minggu Ini |
| In Progress | Sedang Berjalan |
| Blockers & Risks | Hambatan & Risiko |
| Next Week Plan | Rencana Minggu Depan |
| Progress Metrics | Metrik Progress |
| Completed | Selesai |
| In Progress | Berjalan |
| Blocked | Terhambat |
| Planned | Direncanakan |
| Deferred | Ditunda |
| Feature | Fitur |
| Overall Progress | Progress Keseluruhan |
| MVP Milestone | Target MVP |
| Dependency | Ketergantungan |
| Risk | Risiko |
| Mitigation | Penanganan |
| Estimated Completion | Estimasi Penyelesaian |
| None | Tidak ada |

## What This Skill Does NOT Do

- **Does not send the report** — generates the file for your review
- **Does not track progress** — use `pm-wbs-tracker` for that
- **Does not create sprint plans** — use `pm-sprint-planner` for that
- **Does not invent progress** — only reports what the user provides. If data is missing, ask.
- **Does not include internal details** — no developer names, no task keys, no technical jargon (unless the client specifically uses these)

## Output Rules

- Always output as **.docx file** using the docx skill
- Always in **Bahasa Indonesia**
- Always follow the **fixed section structure** — no skipping sections
- Always use **consistent status labels** (✅ 🔄 ⚠️ 📋 ⏸️)
- Always **present the file** to the user before they send to client
- If the user provides sparse input, generate what you can and flag gaps: "Bagian [X] belum diisi — mohon lengkapi sebelum dikirim ke klien"
