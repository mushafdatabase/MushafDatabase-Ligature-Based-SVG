<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="readme-images/bismillah-white.svg">
    <source media="(prefers-color-scheme: light)" srcset="readme-images/bismillah-black.svg">
    <img src="readme-images/bismillah-black" width="150" alt="In the name of God, the most gracious, the most merciful">
  </picture>
</p>
<br><br>
<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="readme-images/mushafdatabase-logo-white.png">
    <source media="(prefers-color-scheme: light)" srcset="readme-images/mushafdatabase-logo-black.png">
    <img src="readme-images/mushafdatabase-logo-black.png" width="600" alt="Mushaf Database logo">
  </picture>
</p>
<br><br>



# **Mushaf Database**

**Ligature-Based SVG File Structure Specification**

**Developer Reference for the 604-page Hafs Mushaf Ligature-Based SVG Dataset**

<table>
  <tbody>
    <tr>
      <td>Purpose: Define the normative structure, metadata, naming, and querying model for one SVG per Mushaf page.

Scope: 604 SVG files representing the Holy Quran (Hafs narration), suitable for rendering, search, recitation sync, highlighting, and accessibility tooling.

Audience: Application developers, data pipeline authors, QA engineers, and maintainers of the Mushaf Database project. 

Intended Use Cases: The resulting specification is intended for projects that require both deterministic page rendering and machine-readable structural metadata. Typical use cases include audio-synchronized highlighting, word-level interaction, recitation-following interfaces, search and indexing pipelines, accessibility tooling, visual QA and validation workflows, annotation systems, educational applications, AI and machine-learning workflows, proofreading and review tools, printing and publishing pipelines, and other advanced Quran software scenarios where exact page fidelity and fine-grained structural addressing are both essential.</td>
    </tr>
  </tbody>
</table>

<p align="center">
<table>
  <thead>
    <tr>
      <th align="center" style="text-align:center;">Version 1.0</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Prepared for publication and internal developer onboarding</td>
    </tr>
    <tr>
      <td>Friday, March 13, 2026 - الجمعة 24 رمضان 1447</td>
    </tr>
    <tr>
    <td>
      <p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="readme-images/aliflammim-logo-white.png">
    <source media="(prefers-color-scheme: light)" srcset="readme-images/aliflammim-logo-black.png">
    <img src="readme-images/aliflammim-logo-black.png" width="150" alt="Alif Lam Mim logo">
  </picture>
</p>
</td>
    </tr>
  </tbody>
</table>
</p>
<br><br>

# 1. Overview

The Mushaf Database stores the Holy Quran as a corpus of 604 SVG documents, with exactly one SVG file for each printed page of the Madinah Mushaf. The source visual artwork is derived from the Madinah Mushaf issued by the King Fahd Glorious Quran Printing Complex in Al-Madinah Al-Munawwarah (مجمع الملك فهد لطباعة المصحف الشريف في المدينة المنورة), using the digital Mushaf materials published through the Quran Complex portal for the Madinah Mushaf according to the narration of Hafs from Asim (مصحف المدينة النبوية وفق رواية حفص عن عاصم) within the printing-use Mushaf collection (مصحف المدينة النبوية لأعمال الطباعة) URL: https://dm.qurancomplex.gov.sa/.

In the original source materials, the Quranic page content is provided as vector artwork intended to preserve exact printed appearance. However, this source artwork is not inherently organized as a developer-friendly semantic document model. Major portions of Quran text geometry may be embedded as large combined path structures rather than being separated into addressable textual units such as lines, words, ligatures, or diacritics.

The Mushaf Database builds on that source through extensive structural transformation and enrichment. The original vector artwork has been decomposed into meaningful graphical and logical components, then reorganized into a consistent hierarchy of SVG groups, stable element identifiers, and rich custom data-* attributes. This work makes it possible for software systems to address and interpret pages, lines, words, sub-word shapes, diacritics, verse markers, sajdah marks, page ornaments, and other recitation-related or layout-related elements programmatically while preserving exact visual fidelity to the printed Mushaf.

The resulting specification is intended for projects that require both deterministic page rendering and machine-readable structural metadata. Typical use cases include audio-synchronized highlighting, word-level interaction, recitation-following interfaces, search and indexing pipelines, accessibility tooling, visual QA and validation workflows, annotation systems, educational applications, AI and machine-learning workflows, proofreading and review tools, printing and publishing pipelines, and other advanced Quran software scenarios where exact page fidelity and fine-grained structural addressing are both essential.

# 2. Design Goals

• Preserve the visual appearance of each Mushaf page as vector paths.

• Expose stable IDs and metadata for DOM-level querying and application logic.

• Distinguish Quranic content from non-Quranic decorations and marginal markers.

• Keep the hierarchy predictable across all 604 pages.

• Support validation, migration, and tooling evolution without breaking ID contracts.

# 3. File Basics

<table>
  <thead>
    <tr>
      <th>Property</th>
      <th>Specification</th>
      <th>Notes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Dataset size</td>
      <td>604 SVG files</td>
      <td>One file per Mushaf page</td>
    </tr>
    <tr>
      <td>Narration</td>
      <td>Hafs</td>
      <td>Holy Quran page set</td>
    </tr>
    <tr>
      <td>Encoding</td>
      <td>UTF-8 with XML declaration</td>
      <td>Human-readable metadata and Arabic strings</td>
    </tr>
    <tr>
      <td>Root viewport</td>
      <td><code>viewBox="0 0 382.68 547.09"</code></td>
      <td>Common canonical page viewport</td>
    </tr>
    <tr>
      <td>Aspect handling</td>
      <td><code>preserveAspectRatio="xMidYMid meet"</code></td>
      <td>Centered responsive scaling</td>
    </tr>
    <tr>
      <td>Coordinate precision</td>
      <td>Up to 2 decimal places</td>
      <td>Recommended upper bound</td>
    </tr>
    <tr>
      <td>Lines per page</td>
      <td>15</td>
      <td>Pages 1–2 may use empty slots for centering</td>
    </tr>
    <tr>
      <td>Root title/desc</td>
      <td>Required</td>
      <td>Useful for tooling and accessibility</td>
    </tr>
  </tbody>
</table>



# 4. File Versioning

Each SVG file must declare the Mushaf SVG specification version used to generate it.
The version is stored on the root `<svg>`  element using a data attribute.

Definition:

• data-md-version: Specifies the version of the Mushaf SVG specification applied to the file.

Rules:

• The attribute must be present in every SVG file.
• The value must follow a semantic version format (e.g., 1.0, 1.1, 2.0).
• Files with different versions may have structural or metadata differences and must be interpreted accordingly by consumers.

# 5. Root SVG Contract

Every file shall begin with a canonical SVG root that declares the page identity, viewport, and document-level metadata. The document title and description shall be present and shall describe the page clearly.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<svg
  version="1.1"
  id="Mushaf_Page_001"
  xmlns="http://www.w3.org/2000/svg"
  x="0px" y="0px"
  preserveAspectRatio="xMidYMid meet"
  viewBox="0 0 382.68 547.09"
  data-md-version="1.0">
  <title>Mushaf Page 1</title>
  <desc>Vector page for Mushaf page 1 with grouped Quran words and metadata.</desc>
</svg>
```

Recommended rule: keep the root identifier page-specific, while the internal structural IDs remain generic or patterned according to this specification.

# 6. Canonical Document Hierarchy

The page is divided into two sibling regions: an outer region for page-level decorations and marginal markers, and an inner region for the Quranic text area, including any inner surah-name or bismillah decorations.

```text
svg
└── g#md-page (data-page-number)
    ├── g#md-page-outer
    │   ├── g#md-non-quranic-header-surah-name
    │   ├── g#md-non-quranic-page-number
    │   ├── g#md-non-quranic-header-juz-name
    │   ├── g#md-non-quranic-margin-juz-hizb     (if applicable)
    │   ├── g#md-non-quranic-margin-sajda        (if applicable)
    │   └── g#md-non-quranic-margin-sakta        (if applicable)
    └── g#md-page-inner (data-rect)
        ├── g#md-line-01 ... g#md-line-15
        │   ├── g#md-non-quranic-{id} (line with data-type="surah-name" or "bismillah", if applicable)
        │   ├── g#md-word-{id}
        │   │   ├── g#md-ligature-{id}-{seq}
        │   │   │   └── path[data-type="text"][data-text]
        │   │   └── g#md-diacritic-{id}-{seq}
        │   │       └── path[data-type="diacritic|dots"][data-diacritic|data-dots]
        │   └── g#md-aya-mark-{id}
        │       ├── g#md-ornament-{id}-01
        │       └── g#md-number-{id}-02
```

# 7. Page Wrapper Groups

<table>
  <thead>
    <tr>
      <th>Group</th>
      <th>Role</th>
      <th>Key attributes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>md-page</code></td>
      <td>Top-level page wrapper</td>
      <td><code>data-page-number</code></td>
    </tr>
    <tr>
      <td><code>md-page-outer</code></td>
      <td>Header, page number, and margin decorations</td>
      <td>No Quranic word content</td>
    </tr>
    <tr>
      <td><code>md-page-inner</code></td>
      <td>Text area and inner decorative elements</td>
      <td><code>data-rect="x,y,width,height"</code></td>
    </tr>
  </tbody>
</table>

The data-rect value on md-page-inner stores the bounding rectangle of the Quran text area within the Mushaf page.  This defines the main Quran section of the page and identifies the spatial region occupied by the Quran text. The text region is generally stable across the dataset, though it is not required to be identical on every page.



# 8. Non-Quranic Elements

## 8.1 Outer non-Quranic groups

<table>
  <thead>
    <tr>
      <th>ID pattern</th>
      <th>Description</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>md-non-quranic-header-surah-name</code></td>
      <td>Surah title placed in the top header region</td>
    </tr>
    <tr>
      <td><code>md-non-quranic-page-number</code></td>
      <td>Page number in the footer region</td>
    </tr>
    <tr>
      <td><code>md-non-quranic-header-juz-name</code></td>
      <td>Juz title placed in the top header region</td>
    </tr>
    <tr>
      <td><code>md-non-quranic-margin-juz-hisb</code></td>
      <td>Margin marker for juz/hisb boundaries</td>
    </tr>
    <tr>
      <td><code>md-non-quranic-margin-sajda</code></td>
      <td>Margin sajda marker when applicable</td>
    </tr>
    <tr>
      <td><code>md-non-quranic-margin-sakta</code></td>
      <td>Margin sakta marker when applicable</td>
    </tr>
  </tbody>
</table>

## 8.2 Inner non-Quranic groups

<table>
  <thead>
    <tr>
      <th>ID pattern</th>
      <th>Description</th>
      <th>Typical context</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>md-non-quranic-{id}</code></td>
      <td>Surah name title</td>
      <td>Line with <code>data-type="surah-name"</code></td>
    </tr>
    <tr>
      <td><code>md-non-quranic-{id}</code></td>
      <td>Bismillah</td>
      <td>Line with <code>data-type="bismillah"</code></td>
    </tr>
  </tbody>
</table>

Path IDs beneath these groups follow the same generic numbered model, for example md-path-001-01 and md-path-002-01. 

# 9. Line Model
Each page reserves exactly 15 line slots. The line group always carries a two-digit data-line-number and a semantic data-type that describes how the line should be interpreted.

<table>
  <thead>
    <tr>
      <th>data-type</th>
      <th>Meaning</th>
      <th>Notes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>text</code></td>
      <td>Standard Quranic text line</td>
      <td>Contains words and ayah marks</td>
    </tr>
    <tr>
      <td><code>surah-name</code></td>
      <td>Surah name title line</td>
      <td>Usually line 01 on short-surah pages</td>
    </tr>
    <tr>
      <td><code>bismillah</code></td>
      <td>Bismillah line</td>
      <td>Appears after a surah-name line when applicable</td>
    </tr>
    <tr>
      <td><code>empty</code></td>
      <td>Reserved blank line</td>
      <td>Used on early pages for centering/layout</td>
    </tr>
  </tbody>
</table>

```xml
<g id="md-line-01" data-line-number="01" data-type="text">
```
# 10. Word Group Contract
Each Quranic word is represented by a dedicated md-word group. Words are serialized in right-to-left visual order inside the line, so the first word appearing on the right side of the line is also the first word in source order.

<table>
  <thead>
    <tr>
      <th>Attribute</th>
      <th>Description</th>
      <th>Example</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>data-surah</code></td>
      <td>Three-digit surah number</td>
      <td><code>"106"</code></td>
    </tr>
    <tr>
      <td><code>data-aya</code></td>
      <td>Three-digit ayah number</td>
      <td><code>"001"</code></td>
    </tr>
    <tr>
      <td><code>data-line-number</code></td>
      <td>Two-digit page line position</td>
      <td><code>"03"</code></td>
    </tr>
    <tr>
      <td><code>data-type</code></td>
      <td>Word-level type classifier</td>
      <td><code>"text"</code></td>
    </tr>
    <tr>
      <td><code>data-hafs</code></td>
      <td>Hafs word</td>
      <td><code>"لِإِيلَافِ"</code></td>
    </tr>
    <tr>
      <td><code>data-imlaey</code></td>
      <td>Imlaey word</td>
      <td><code>"لإيلاف"</code></td>
    </tr>
    <tr>
      <td><code>data-word-index-in-ayah</code></td>
      <td>1-based position inside the ayah</td>
      <td><code>"1"</code></td>
    </tr>
    <tr>
      <td><code>data-waw-alatf</code></td>
      <td>Optional flag for waw al-ʿatf</td>
      <td><code>"true"</code></td>
    </tr>
  </tbody>
</table>

```xml
<g id="md-word-003"
   data-surah="106"
   data-aya="001"
   data-line-number="03"
   data-type="text"
   data-hafs="لِإِيلَٰفِ"
   data-imlaey="لإيلاف"
   data-word-index-in-ayah="1">
```

data-hafs: The word according to the KFGQPC Hafs Uthmanic Script released by the King Fahd Glorious Quran Printing Complex. Its exact form may vary depending on the source font version.

data-imlaey: The word in simplified spelling form ("إملائي"), written as plain orthographic text for easier reading and inference. It does not necessarily preserve the exact Uthmanic spelling used in the Mushaf.



# 11. Ayah Mark Structure
Verse markers are not encoded as md-word groups. Instead, they use the dedicated md-aya-mark prefix so consumers can distinguish them from Quranic words without inspecting child paths.

<table>
  <thead>
    <tr>
      <th>Child group</th>
      <th>Function</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>md-ornament-{id}-01</code></td>
      <td>Decorative circular or ornamental frame</td>
    </tr>
    <tr>
      <td><code>md-number-{id}-02</code></td>
      <td>Verse number digits placed inside the ornament</td>
    </tr>
  </tbody>
</table>

```xml
<g id="md-aya-mark-007"
   data-surah="112"
   data-aya="001"
   data-line-number="03"
   data-type="aya-mark">
  <g id="md-ornament-014-01">...</g>
  <g id="md-number-014-02">...</g>
</g>
```

# 12. Ligatures and Diacritics
## 12.1 Ligature groups
The letter body of a word is decomposed into one or more ligature groups. Each ligature contains one or more path elements carrying data-type="text" and a data-text fragment that identifies the Arabic portion drawn by that path.

<table>
  <thead>
    <tr>
      <th>Path attribute</th>
      <th>Meaning</th>
      <th>Example</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>data-type</code></td>
      <td>Always <code>"text"</code> for letter-body paths</td>
      <td><code>"text"</code></td>
    </tr>
    <tr>
      <td><code>data-text</code></td>
      <td>Arabic fragment represented by the path</td>
      <td><code>"با"</code> or <code>"ص"</code></td>
    </tr>
  </tbody>
</table>

## 12.2 Diacritic groups
Each ligature may own a sibling or nested md-diacritic group that contains the vowel marks, hamza, dots, and related signs associated with the ligature. This separation is valuable for highlighting, recoloring, or selectively hiding marks.

<table>
  <thead>
    <tr>
      <th>data-diacritic value</th>
      <th>Meaning</th>
    </tr>
  </thead>
  <tbody>
    <tr><td><code>fatha</code></td><td>Fathah</td></tr>
    <tr><td><code>damma</code></td><td>Dammah</td></tr>
    <tr><td><code>kasra</code></td><td>Kasrah</td></tr>
    <tr><td><code>fathatan</code></td><td>Fathatan</td></tr>
    <tr><td><code>dammatan</code></td><td>Dammatan</td></tr>
    <tr><td><code>kasratan</code></td><td>Kasratan</td></tr>
    <tr><td><code>successive fathatan</code></td><td>Double sequential fathatan</td></tr>
    <tr><td><code>successive dammatan</code></td><td>Double sequential dammatan</td></tr>
    <tr><td><code>successive kasratan</code></td><td>Double sequential kasratan</td></tr>
    <tr><td><code>shadda</code></td><td>Shaddah</td></tr>
    <tr><td><code>sukun</code></td><td>Sukūn</td></tr>
    <tr><td><code>superscript alef</code></td><td>Small alef above</td></tr>
    <tr><td><code>maddah</code></td><td>Maddah</td></tr>
    <tr><td><code>hamza</code></td><td>Hamzah</td></tr>
    <tr><td><code>wasla</code></td><td>Waslah</td></tr>
    <tr><td><code>fatha iqlab</code></td><td>Fathah with iqlab</td></tr>
    <tr><td><code>damma iqlab</code></td><td>Dammah with iqlab</td></tr>
    <tr><td><code>kasra iqlab</code></td><td>Kasrah with iqlab</td></tr>
    <tr><td><code>small meem</code></td><td>Small meem</td></tr>
    <tr><td><code>small seen</code></td><td>Small seen</td></tr>
    <tr><td><code>small waw</code></td><td>Small waw</td></tr>
    <tr><td><code>small yeh</code></td><td>Small yeh</td></tr>
    <tr><td><code>rounded zero</code></td><td>Rounded zero</td></tr>
    <tr><td><code>rectangular zero</code></td><td>Rectangular zero</td></tr>
  </tbody>
</table>

<table>
  <thead>
    <tr>
      <th>data-dots value</th>
      <th>Meaning</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>dot</code></td>
      <td>Single dot</td>
    </tr>
    <tr>
      <td><code>two dots</code></td>
      <td>Two dots</td>
    </tr>
    <tr>
      <td><code>three dots</code></td>
      <td>Three dots</td>
    </tr>
  </tbody>
</table>

# 13. Waqf and Special Signs
Pause marks are encoded at path level through data-waqf, even when the enclosing word remains a normal word-level element. This makes it possible to preserve the reading text while still detecting Waqf symbols.

<table>
  <thead>
    <tr>
      <th>data-waqf value</th>
      <th>Meaning</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>waqf lazim</code></td>
      <td>Compulsory stop</td>
    </tr>
    <tr>
      <td><code>waqf jaiz</code></td>
      <td>Permissible stop</td>
    </tr>
    <tr>
      <td><code>waqf sali</code></td>
      <td>Preferred continuation</td>
    </tr>
    <tr>
      <td><code>waqf qila</code></td>
      <td>Some say stop</td>
    </tr>
    <tr>
      <td><code>waqf taanuq</code></td>
      <td>Embrace stop pair</td>
    </tr>
  </tbody>
</table>

<table>
  <thead>
    <tr>
      <th>Special element</th>
      <th>Group prefix</th>
      <th>Word/path data-type</th>
      <th>Notes</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Sajda prostration mark</td>
      <td><code>md-word-*</code></td>
      <td><code>sajda-mehrab</code></td>
      <td>U+06E9-like sajda symbol</td>
    </tr>
    <tr>
      <td>Juz/hisb boundary star</td>
      <td><code>md-word-*</code></td>
      <td><code>juz-star</code></td>
      <td>U+06DE-style boundary star</td>
    </tr>
    <tr>
      <td>Sajda overline</td>
      <td><code>md-sajda-line-*</code></td>
      <td><code>sajda-line</code></td>
      <td>Line drawn over relevant phrase</td>
    </tr>
  </tbody>
</table>

# 14. Path-Level data-type Values

<table>
  <thead>
    <tr>
      <th>data-type</th>
      <th>Used for</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>text</code></td>
      <td>Letter body / ligature shape</td>
    </tr>
    <tr>
      <td><code>diacritic</code></td>
      <td>Vowel mark, hamza, shadda, or related sign</td>
    </tr>
    <tr>
      <td><code>dots</code></td>
      <td>Dot marks</td>
    </tr>
    <tr>
      <td><code>kaf-hamza</code></td>
      <td>Special kaf-hamza composition</td>
    </tr>
    <tr>
      <td><code>waqf</code></td>
      <td>Pause sign</td>
    </tr>
    <tr>
      <td><code>sajda-line</code></td>
      <td>Sajda overline path</td>
    </tr>
    <tr>
      <td><code>sajda-mehrab</code></td>
      <td>Sajda symbol path</td>
    </tr>
    <tr>
      <td><code>juz-star</code></td>
      <td>Juz boundary star path</td>
    </tr>
  </tbody>
</table>


# 15. ID Naming Rules
IDs must be globally unique within a single SVG file. Numeric identifier segments shall remain consistent with the project convention. In this dataset, child object numbering starts from 1, and every child object shall have a stable suffix sequence.

<table>
  <thead>
    <tr>
      <th>Pattern</th>
      <th>Example</th>
      <th>Purpose</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>md-page</code></td>
      <td><code>md-page</code></td>
      <td>Page wrapper</td>
    </tr>
    <tr>
      <td><code>md-line-{line}</code></td>
      <td><code>md-line-01</code></td>
      <td>Line group</td>
    </tr>
    <tr>
      <td><code>md-word-{id}</code></td>
      <td><code>md-word-002</code></td>
      <td>Quranic word</td>
    </tr>
    <tr>
      <td><code>md-aya-mark-{id}</code></td>
      <td><code>md-aya-mark-021</code></td>
      <td>Verse marker</td>
    </tr>
    <tr>
      <td><code>md-ligature-{id}-{seq}</code></td>
      <td><code>md-ligature-002-01</code></td>
      <td>Ligature group</td>
    </tr>
    <tr>
      <td><code>md-diacritic-{id}-{seq}</code></td>
      <td><code>md-diacritic-002-01</code></td>
      <td>Diacritic group</td>
    </tr>
    <tr>
      <td><code>md-ornament-{id}-01</code></td>
      <td><code>md-ornament-021-01</code></td>
      <td>Aya frame</td>
    </tr>
    <tr>
      <td><code>md-number-{id}-02</code></td>
      <td><code>md-number-021-02</code></td>
      <td>Aya number</td>
    </tr>
    <tr>
      <td><code>md-juz-star-{id}-{seq}</code></td>
      <td><code>md-juz-star-023-01</code></td>
      <td>Juz star path</td>
    </tr>
    <tr>
      <td><code>md-sajda-line-{id}-{seq}</code></td>
      <td><code>md-sajda-line-029-03</code></td>
      <td>Sajda overline</td>
    </tr>
    <tr>
      <td><code>md-path-{...}</code></td>
      <td><code>md-path-007-02-01</code></td>
      <td>Generic path</td>
    </tr>
  </tbody>
</table>

# 16. Normative Validation Rules
• Each file shall contain exactly one root g#md-page wrapper.

• Each file shall contain exactly one g#md-page-outer and exactly one g#md-page-inner as children of md-page.

• Each page shall reserve exactly 15 line groups, even when some are empty or decorative.

• Every ID shall be unique within the SVG document.

• Every md-word and md-aya-mark shall carry line and surah/ayah metadata sufficient for DOM querying.

• Words shall appear in right-to-left visual order within a line.

• Quranic words and ayah marks shall be distinct group types; verse ornaments shall not be modeled as ordinary words.

• Root title and desc metadata shall be present for readability and tooling.

• The root `<svg>` element shall include a data-md-version attribute.



# 17. JavaScript Query Examples

```js
// All words on line 03
const line03Words = document.querySelectorAll(
  '[id^="md-word-"][data-line-number="03"][data-type="text"]'
);

// All ayah markers on the page
const ayaMarks = document.querySelectorAll('[id^="md-aya-mark-"]');

// All standard words in Surah 002, Ayah 006
const ayahWords = document.querySelectorAll(
  '[id^="md-word-"][data-surah="002"][data-aya="006"][data-type="text"]'
);

// All shadda diacritics
const shaddaMarks = document.querySelectorAll('[data-diacritic="shadda"]');

// Highlight every path in a clicked word
word.querySelectorAll('path').forEach(p => {
  p.style.fill = '#2E86C1';
});
```

# 18. Example Skeleton for a Page

```xml
<g id="md-page" data-page-number="604">
  <g id="md-page-outer">
    <g id="md-non-quranic-header-surah-name">...</g>
    <g id="md-non-quranic-page-number">...</g>
    <g id="md-non-quranic-header-juz-name">...</g>
  </g>
  <g id="md-page-inner" data-rect="93.49,74.75,335.23,473.24">
    <g id="md-line-01" data-line-number="01" data-type="surah-name">
      <g id="md-non-quranic-001">...</g>
    </g>
    <g id="md-line-02" data-line-number="02" data-type="bismillah">
      <g id="md-non-quranic-002">...</g>
    </g>
    <g id="md-line-03" data-line-number="03" data-type="text">
      <g id="md-word-003" data-surah="112" data-aya="001"
         data-line-number="03" data-type="text"
         data-hafs="قُلۡ" data-imlaey="قل" data-word-index-in-ayah="1">
        <g id="md-ligature-003-01">
          <path id="md-path-003-01" data-type="text" data-text="قل"/>
          <g id="md-diacritic-003-01">
            <path id="md-path-003-01-01" data-type="dots" data-dots="two dots" d="…"/>
            <path id="md-path-003-01-02" data-type="diacritic" data-diacritic="damma" d="…"/>
            <path id="md-path-003-01-03" data-type="diacritic" data-diacritic="sukun" d="…"/>
          </g>
        </g>
      </g>      
    </g>
  </g>
</g>
```

# 19. Complete Attribute Reference

<table>
  <thead>
    <tr>
      <th>Attribute</th>
      <th>Found on</th>
      <th>Meaning</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><code>data-md-version</code></td>
      <td>SVG</td>
      <td>Mushaf SVG specification version</td>
    </tr>
    <tr>
      <td><code>data-page-number</code></td>
      <td><code>md-page</code></td>
      <td>Page number from 001 to 604</td>
    </tr>
    <tr>
      <td><code>data-rect</code></td>
      <td><code>md-page-inner</code></td>
      <td>Text area rectangle: <code>x,y,width,height</code></td>
    </tr>
    <tr>
      <td><code>data-line-number</code></td>
      <td><code>md-line-*</code>, <code>md-word-*</code>, <code>md-aya-mark-*</code></td>
      <td>Two-digit line position</td>
    </tr>
    <tr>
      <td><code>data-type</code></td>
      <td>Multiple levels</td>
      <td>Element classifier</td>
    </tr>
    <tr>
      <td><code>data-surah</code></td>
      <td><code>md-word-*</code>, <code>md-aya-mark-*</code></td>
      <td>Surah number</td>
    </tr>
    <tr>
      <td><code>data-aya</code></td>
      <td><code>md-word-*</code>, <code>md-aya-mark-*</code></td>
      <td>Ayah number</td>
    </tr>
    <tr>
      <td><code>data-hafs</code></td>
      <td><code>md-word-*</code></td>
      <td>Hafs orthography text</td>
    </tr>
    <tr>
      <td><code>data-imlaey</code></td>
      <td><code>md-word-*</code></td>
      <td>Imlaey orthography text</td>
    </tr>
    <tr>
      <td><code>data-word-index-in-ayah</code></td>
      <td><code>md-word-*</code></td>
      <td>1-based word order within ayah</td>
    </tr>
    <tr>
      <td><code>data-waw-alatf</code></td>
      <td><code>md-word-*</code></td>
      <td>Optional marker for waw al-atf</td>
    </tr>
    <tr>
      <td><code>data-text</code></td>
      <td>path in ligature</td>
      <td>Arabic text fragment for that path</td>
    </tr>
    <tr>
      <td><code>data-diacritic</code></td>
      <td>path in diacritic</td>
      <td>Diacritic category</td>
    </tr>
    <tr>
      <td><code>data-dots</code></td>
      <td>path in diacritic</td>
      <td>Dot-mark category</td>
    </tr>
    <tr>
      <td><code>data-waqf</code></td>
      <td>path in ligature</td>
      <td>Pause-mark category</td>
    </tr>
  </tbody>
</table>

# 20. Legal Use and Open Permission (Sadaqa-e-Jaria)
This dataset is released as Sadaqa-e-Jaria for the benefit of Muslims worldwide.

Permission is hereby granted to any person, without prior written approval, to use, copy, modify, publish, distribute, and create derivative works from this dataset, in whole or in part, for any lawful purpose, including personal, educational, research, nonprofit, and commercial use.

Users must not knowingly alter the Quranic content in any way that misrepresents or compromises the integrity of the Holy Quran.

This dataset is provided “as is”, without warranty of any kind, and the publisher shall not be liable for any claim, damage, or loss arising from its use.


