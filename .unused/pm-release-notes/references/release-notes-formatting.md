# Release Notes Formatting Reference

## Table of Contents
1. Word Document Formatting
2. Category Table Format
3. Terminology Guide
4. Sample Release Notes

---

## 1. Word Document Formatting

### Page Setup
- **Paper:** A4 (docx-js default)
- **Margins:** 1440 DXA (1 inch) all sides
- **Font:** Arial throughout

### Style Definitions

Same base styles as `pm-stakeholder-update`:

```javascript
styles: {
  default: {
    document: { run: { font: "Arial", size: 22 } } // 11pt body
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

| Element | Size (half-pts) | Weight | Color |
|---|---|---|---|
| Document title | 56 | Bold | 1F2937 |
| Version + date subtitle | 28 | Normal | 2563EB (blue) |
| Section heading (H1) | 32 | Bold | 1F2937 |
| Item title | 22 | Bold | 1F2937 |
| Item description | 22 | Normal | 374151 |
| Category badge | 20 | Bold | Per category (see below) |

### Category Colors

Each category has a distinct badge color:

| Category | Badge Fill | Badge Text | Emoji |
|---|---|---|---|
| Fitur Baru | DBEAFE (light blue) | 1E40AF (dark blue) | 🆕 |
| Peningkatan | D1FAE5 (light green) | 065F46 (dark green) | ✨ |
| Perbaikan | FEE2E2 (light red) | 991B1B (dark red) | 🔧 |
| Perubahan Tampilan | F3E8FF (light purple) | 6B21A8 (dark purple) | 🎨 |
| Optimasi | FEF3C7 (light amber) | 92400E (dark amber) | ⚡ |

---

## 2. Category Table Format

Each category section uses a simple 2-column table:

| Column | Width (DXA) | Content |
|---|---|---|
| No | 600 | Item number within category |
| Deskripsi | 8426 | Title (bold) + description (normal) in same cell |
| **Total** | **9026** | Must equal A4 content width |

### Table Styling

```javascript
// Header row per category — full width, colored
headerFill: category badge fill color
headerFont: category badge text color, bold

// Body rows
borders: thin (E5E7EB)
cellMargins: { top: 80, bottom: 80, left: 120, right: 120 }
alternatingRows: false (keep clean for short tables)
```

### Item Cell Layout

Within each Description cell, use two TextRuns:
1. **Title** — bold, 11pt, color 1F2937
2. **Description** — normal, 11pt, color 374151, preceded by line break

```javascript
new Paragraph({
  children: [
    new TextRun({ text: "Profil Member dengan field baru", bold: true, font: "Arial", size: 22, color: "1F2937" }),
    new TextRun({ text: "\n", break: 1 }),
    new TextRun({ text: "Formulir profil kini menampilkan nickname, alamat KTP, dan 3 kontak darurat untuk setiap member.", font: "Arial", size: 22, color: "374151" }),
  ]
})
```

---

## 3. Terminology Guide

Consistent terms for release notes:

### Section Headers

| English | Bahasa Indonesia |
|---|---|
| Release Notes | Catatan Rilis |
| Release Summary | Ringkasan Rilis |
| New Features | Fitur Baru |
| Enhancements | Peningkatan |
| Bug Fixes | Perbaikan |
| UI/UX Changes | Perubahan Tampilan |
| Performance | Optimasi |
| Notes | Catatan |
| Known Issues | Masalah Diketahui |
| Version | Versi |
| Release Date | Tanggal Rilis |

### Common Phrases

| English | Bahasa Indonesia |
|---|---|
| Users can now... | Pengguna kini dapat... |
| Added support for... | Ditambahkan dukungan untuk... |
| Fixed an issue where... | Diperbaiki masalah dimana... |
| Improved performance of... | Peningkatan performa pada... |
| Updated the display of... | Tampilan diperbarui pada... |
| This release includes... | Rilis ini mencakup... |
| No known issues | Tidak ada masalah yang diketahui |

### Platform Names (Client-Facing)

| Internal Code | Client-Facing Name |
|---|---|
| WEB | Web Admin |
| TAB / Staff Ops | Tablet Staf |
| MOB | Aplikasi Staf Mobile |
| FLD / Staff Field | Aplikasi Staf Lapangan |
| NOK | Aplikasi Keluarga (NOK) |
| SNR / Senior App | Aplikasi Senior |
| DOC | Aplikasi Dokter |
| PFR | Photo Frame |
| Health Station | Health Station |

---

## 4. Sample Release Notes

```
CATATAN RILIS
SSI (Senior Service Indonesia)
Versi 1.2.0 — 28 Maret 2026

────────────────────────────────────────

RINGKASAN RILIS

Rilis ini mencakup 3 fitur baru pada modul Member & Medical,
peningkatan pada manajemen aktivitas, dan perbaikan bug pada
sinkronisasi Health Station.


🆕 FITUR BARU

| No | Deskripsi                                                  |
|----|------------------------------------------------------------|
| 1  | Profil Member dengan field baru                            |
|    | Formulir profil kini menampilkan nickname, alamat KTP,     |
|    | dan 3 kontak darurat untuk setiap member.                  |
| 2  | Manajemen Data Keluarga (NOK)                              |
|    | Admin dapat menambah dan mengedit data Next of Kin         |
|    | melalui Web Admin.                                         |
| 3  | Catatan Medis Format SOAP                                  |
|    | Staf lapangan dapat mencatat kondisi medis menggunakan     |
|    | format SOAP (Subjective, Objective, Assessment, Plan).     |


✨ PENINGKATAN

| No | Deskripsi                                                  |
|----|------------------------------------------------------------|
| 1  | Penugasan Multi-Staf per Sesi Aktivitas                    |
|    | Setiap sesi aktivitas kini dapat ditugaskan ke lebih       |
|    | dari satu staf instruktur.                                 |


🔧 PERBAIKAN

| No | Deskripsi                                                  |
|----|------------------------------------------------------------|
| 1  | Sinkronisasi Health Station                                |
|    | Diperbaiki masalah koneksi Bluetooth yang menyebabkan      |
|    | data pengukuran tidak tersinkronisasi pada beberapa        |
|    | perangkat.                                                 |


CATATAN

Tidak ada masalah yang diketahui pada rilis ini.
```

### Key Observations

- Version number prominent in header (blue)
- Ringkasan leads with count: "3 fitur baru, peningkatan, perbaikan"
- Each item has bold title + optional description
- No task keys, no developer names, no platform codes
- Empty categories (Perubahan Tampilan, Optimasi) are omitted entirely
- Catatan section included even when empty ("Tidak ada masalah yang diketahui")
