# Formatting & Terminology Reference

## Table of Contents
1. Word Document Formatting
2. Table Format
3. Translation Guide (EN → ID)
4. Sample Report Content

---

## 1. Word Document Formatting

### Page Setup
- **Paper:** A4 (11906 × 16838 DXA — docx-js default)
- **Margins:** 1440 DXA (1 inch) all sides
- **Font:** Arial throughout
- **Content width:** 9026 DXA (A4 minus margins)

### Style Definitions

```javascript
// Use these exact styles when generating the docx
styles: {
  default: {
    document: {
      run: { font: "Arial", size: 22 } // 11pt body
    }
  },
  paragraphStyles: [
    {
      id: "Heading1", name: "Heading 1",
      basedOn: "Normal", next: "Normal", quickFormat: true,
      run: { size: 32, bold: true, font: "Arial", color: "1F2937" },
      paragraph: { spacing: { before: 360, after: 200 }, outlineLevel: 0 }
    },
    {
      id: "Heading2", name: "Heading 2",
      basedOn: "Normal", next: "Normal", quickFormat: true,
      run: { size: 28, bold: true, font: "Arial", color: "1F2937" },
      paragraph: { spacing: { before: 240, after: 120 }, outlineLevel: 1 }
    }
  ]
}
```

### Element Sizing

| Element | Size (half-pts) | Readable | Weight | Color |
|---|---|---|---|---|
| Document title | 56 | 28pt | Bold | 1F2937 (near black) |
| Subtitle (period/date) | 24 | 12pt | Normal | 6B7280 (gray) |
| Section heading (H1) | 32 | 16pt | Bold | 1F2937 |
| Sub-heading (H2) | 28 | 14pt | Bold | 1F2937 |
| Body text | 22 | 11pt | Normal | 374151 (dark gray) |
| Table header text | 22 | 11pt | Bold | FFFFFF (white) |
| Table body text | 22 | 11pt | Normal | 374151 |

### Color Palette

| Purpose | Hex | Usage |
|---|---|---|
| Primary text | 1F2937 | Headings, titles |
| Body text | 374151 | Paragraphs, table cells |
| Muted text | 6B7280 | Subtitles, dates, labels |
| Table header bg | 2563EB | Blue header row |
| Table alt row bg | F3F4F6 | Alternating row shading |
| Status: completed | 059669 | Green accent for ✅ items |
| Status: in progress | D97706 | Amber accent for 🔄 items |
| Status: blocked | DC2626 | Red accent for ⚠️ items |
| Border | E5E7EB | Table borders, dividers |

---

## 2. Table Format

### Progress Table Structure

Use this table layout for sections 3-6 of the report. 4 columns:

| Column | Width (DXA) | Content |
|---|---|---|
| No | 600 | Row number |
| Fitur / Epic | 2826 | Feature name from WBS/FBD |
| Status | 2000 | Status label (✅ Selesai, 🔄 Berjalan, etc.) |
| Keterangan | 3600 | Brief description in client language |
| **Total** | **9026** | Must equal content width |

### Table Styling Rules

```javascript
// Header row
shading: { fill: "2563EB", type: ShadingType.CLEAR }
// text: white, bold

// Alternating body rows
// Even rows: no shading (white)
// Odd rows: shading fill "F3F4F6"

// All cells
borders: {
  top: { style: BorderStyle.SINGLE, size: 1, color: "E5E7EB" },
  bottom: { style: BorderStyle.SINGLE, size: 1, color: "E5E7EB" },
  left: { style: BorderStyle.SINGLE, size: 1, color: "E5E7EB" },
  right: { style: BorderStyle.SINGLE, size: 1, color: "E5E7EB" }
}
margins: { top: 80, bottom: 80, left: 120, right: 120 }
```

### Status Column Formatting

The Status column should use colored text to match the status:

| Status | Text Color |
|---|---|
| ✅ Selesai | 059669 (green) |
| 🔄 Berjalan | D97706 (amber) |
| ⚠️ Terhambat | DC2626 (red) |
| 📋 Direncanakan | 2563EB (blue) |
| ⏸️ Ditunda | 6B7280 (gray) |

---

## 3. Translation Guide (EN → ID)

Use these terms **consistently** across every report. Do not alternate between synonyms.

### Section Headers

| English | Bahasa Indonesia (use this) |
|---|---|
| Weekly Progress Report | Laporan Progress Mingguan |
| Executive Summary | Ringkasan Eksekutif |
| This Week's Progress | Progress Minggu Ini |
| Work In Progress | Pekerjaan Sedang Berjalan |
| Blockers & Risks | Hambatan & Risiko |
| Next Week Plan | Rencana Minggu Depan |
| Progress Metrics | Metrik Progress |

### Status Labels

| English | Bahasa Indonesia (use this) |
|---|---|
| Completed | Selesai |
| In Progress | Berjalan |
| Blocked | Terhambat |
| Planned | Direncanakan |
| Deferred | Ditunda |

### Common Terms

| English | Bahasa Indonesia (use this) | Do NOT use |
|---|---|---|
| Feature | Fitur | Fitur-fitur, feature |
| Overall Progress | Progress Keseluruhan | Kemajuan keseluruhan |
| MVP Milestone | Target MVP | Milestone MVP |
| Dependency | Ketergantungan | Dependensi |
| Risk | Risiko | Resiko (wrong spelling) |
| Mitigation | Penanganan | Mitigasi |
| Estimated Completion | Estimasi Penyelesaian | Perkiraan selesai |
| None | Tidak ada | Nihil, kosong |
| Reporting Period | Periode Pelaporan | Masa laporan |
| Report Date | Tanggal Laporan | — |
| Blocker | Hambatan | Bloker, penghalang |
| Investigation | Investigasi | Penyelidikan |
| Testing | Pengujian | Testing (keep Indonesian) |
| Development | Pengembangan | Development |
| Team | Tim | Team |
| This week | Minggu ini | Pekan ini |
| Next week | Minggu depan | Pekan depan |

### Number & Date Formatting

- Dates: `DD MMMM YYYY` → "24 Maret 2026" (Indonesian month names)
- Percentages: `XX%` → "78%" (no space before %)
- Decimals: use comma → "3,5 hari" (Indonesian convention)

### Indonesian Month Names

| # | Month |
|---|---|
| 1 | Januari |
| 2 | Februari |
| 3 | Maret |
| 4 | April |
| 5 | Mei |
| 6 | Juni |
| 7 | Juli |
| 8 | Agustus |
| 9 | September |
| 10 | Oktober |
| 11 | November |
| 12 | Desember |

---

## 4. Sample Report Content

This is what a complete report looks like when filled in. Use as a structural reference.

```
LAPORAN PROGRESS MINGGUAN
Proyek SSI (Senior Service Indonesia)
Periode: 24 — 28 Maret 2026
Tanggal Laporan: 28 Maret 2026


RINGKASAN EKSEKUTIF

Minggu ini tim berhasil menyelesaikan fitur Profil Member dan integrasi
data NOK (Next of Kin). Satu hambatan ditemukan pada koneksi Bluetooth
Health Station yang sedang ditangani. Target minggu depan adalah
penyelesaian modul Activity Management.


PROGRESS MINGGU INI

| No | Fitur / Epic        | Status     | Keterangan                                             |
|----|---------------------|------------|--------------------------------------------------------|
| 1  | Profil Member       | ✅ Selesai | Formulir profil menampilkan nickname, alamat KTP,      |
|    |                     |            | dan 3 kontak darurat                                   |
| 2  | Data NOK            | ✅ Selesai | Admin dapat menambah dan mengedit data Next of Kin     |
| 3  | Catatan Medis SOAP  | ✅ Selesai | Formulir catatan medis dengan format SOAP sudah aktif  |


PEKERJAAN SEDANG BERJALAN

| No | Fitur / Epic        | Status      | Keterangan                                            |
|----|---------------------|-------------|-------------------------------------------------------|
| 1  | Family Tree         | 🔄 Berjalan | Visualisasi pohon keluarga sedang dikembangkan,       |
|    |                     |             | estimasi selesai minggu depan                         |
| 2  | Manajemen Aktivitas | 🔄 Berjalan | Formulir pembuatan aktivitas dalam pengerjaan         |


HAMBATAN & RISIKO

| No | Area                   | Status        | Keterangan                                         |
|----|------------------------|---------------|----------------------------------------------------|
| 1  | Health Station Sync    | ⚠️ Terhambat | Kendala koneksi Bluetooth pada beberapa perangkat.  |
|    |                        |               | Tim sedang investigasi dan testing perangkat        |
|    |                        |               | alternatif. Estimasi penyelesaian: 2 hari kerja.   |


RENCANA MINGGU DEPAN

| No | Fitur / Epic           | Status          | Keterangan                                       |
|----|------------------------|-----------------|--------------------------------------------------|
| 1  | Family Tree            | 📋 Direncanakan | Penyelesaian visualisasi pohon keluarga          |
| 2  | Manajemen Aktivitas    | 📋 Direncanakan | Penjadwalan sesi dan penugasan staf              |
| 3  | Absensi Aktivitas      | 📋 Direncanakan | Formulir pencatatan kehadiran peserta            |


METRIK PROGRESS

Fitur MVP selesai    : 12 dari 48 (25%)
Fitur sedang berjalan: 5
Fitur belum dimulai  : 31
Target MVP           : Mei 2026
```

### Key Observations About the Sample

- Grouped by **Feature/Epic**, not by developer or platform
- No task keys visible (SSI-API-002, etc.)
- No developer names
- No technical language (no "endpoint", "BLoC", "model", "migration")
- Blocker includes **what's being done about it** and **estimated resolution**
- Each section is present, even if the report is short
