# PDF Generation Skill

Use this skill whenever the user wants to create, update, or manipulate PDF files.
This includes generating new PDFs from scratch, filling or modifying existing PDFs, reading
and extracting content from PDFs, merging/splitting/rotating PDFs, and encrypting/decrypting
PDFs. Also trigger when the user provides a template PDF and wants a new PDF based on it.

## Libraries

Only two libraries are used:

- **reportlab** — creating new PDFs from scratch (Platypus for multi-page documents, Canvas for
  overlays and simple one-pagers)
- **pypdf** — reading, merging, splitting, rotating, encrypting/decrypting, and overlaying
  existing PDFs

Both libraries are preinstalled in the sandbox. If you need to write a long Python script to
generate a PDF, save the script to `/tmp` first, and then execute it. In case of errors,
iteratively modify the script as needed.

## Working Directory Convention

```
/tmp/          ← all intermediate work happens here
/agent/home/   ← final PDF artifacts go here
```

---

## Task Router

| User wants to…                 | Approach                                                                  |
| ------------------------------ | ------------------------------------------------------------------------- |
| Read / extract text from a PDF | pypdf `PdfReader` → `page.extract_text()`                                 |
| Get PDF metadata               | pypdf `PdfReader` → `reader.metadata`                                     |
| Create a new PDF from scratch  | reportlab (Platypus for multi-page, Canvas for one-pagers)                |
| Create a PDF from a template   | Read with pypdf → overlay with reportlab → merge                          |
| Merge multiple PDFs            | pypdf `PdfWriter` + `add_page()` in a loop                                |
| Split a PDF into pages         | pypdf `PdfWriter` per page                                                |
| Rotate pages                   | pypdf `page.rotate(degrees)`                                              |
| Add watermark / overlay        | pypdf `page.merge_page(watermark_page)`                                   |
| Encrypt / decrypt              | pypdf `writer.encrypt()` / `reader.decrypt()`                             |
| Fill a PDF form                | pypdf for fillable fields; reportlab overlay for non-fillable             |
| Update text in an existing PDF | Extract → rebuild with reportlab (PDFs don't support in-place text edits) |

Code samples for merge, split, rotate, watermark, encrypt/decrypt, and other advanced operations are
in `advanced-pdf-generation.md`.

---

## 1 · Reading PDFs

### Extract all text

```python
from pypdf import PdfReader

reader = PdfReader("/tmp/input.pdf")
for i, page in enumerate(reader.pages):
    print(f"--- Page {i+1} ---")
    print(page.extract_text())
```

### Extract text from specific pages

```python
from pypdf import PdfReader

reader = PdfReader("/tmp/input.pdf")
for i in [0, 2, 4]:  # pages 1, 3, 5 (0-indexed)
    print(reader.pages[i].extract_text())
```

### Get metadata

```python
from pypdf import PdfReader

reader = PdfReader("/tmp/input.pdf")
meta = reader.metadata
print(f"Title: {meta.title}, Author: {meta.author}, Pages: {len(reader.pages)}")
```

---

## 2 · Creating PDFs from Scratch

Use **reportlab** for all PDF generation. Platypus (`SimpleDocTemplate` / `BaseDocTemplate`) is
preferred for multi-page documents because it handles pagination, page breaks, and content flow
automatically.

For complex documents (custom fonts, images, multi-page layouts), write a short diagnostic script
first to verify fonts load and images render correctly before building the full document.

### Fonts

Register TTF fonts for better glyph coverage. Several font families are preinstalled.
Prefer **Inter** (clean sans-serif) and **Libre Baskerville** (elegant serif) as defaults.

Preinstalled font paths:

- `/usr/share/fonts/inter/Inter.ttf` (and `-Italic`, `-Bold`, `-BoldItalic`)
- `/usr/share/fonts/libre-baskerville/LibreBaskerville.ttf` (and `-Italic`, `-Bold`, `-BoldItalic`)
- `/usr/share/fonts/dejavu/DejaVuSans.ttf` (and `-Bold`, `-Oblique`)
- `/usr/share/fonts/noto/` (Noto Sans/Serif, excellent Unicode coverage)

```python
from reportlab.pdfbase import pdfmetrics
from reportlab.pdfbase.ttfonts import TTFont

pdfmetrics.registerFont(TTFont("Inter", "/usr/share/fonts/inter/Inter.ttf"))
pdfmetrics.registerFont(TTFont("Inter-Italic", "/usr/share/fonts/inter/Inter-Italic.ttf"))
pdfmetrics.registerFont(TTFont("Inter-Bold", "/usr/share/fonts/inter/Inter-Bold.ttf"))
pdfmetrics.registerFont(TTFont("Inter-BoldItalic", "/usr/share/fonts/inter/Inter-BoldItalic.ttf"))
pdfmetrics.registerFontFamily("Inter", normal="Inter", bold="Inter-Bold",
                              italic="Inter-Italic", boldItalic="Inter-BoldItalic")

pdfmetrics.registerFont(TTFont("LibreBaskerville", "/usr/share/fonts/libre-baskerville/LibreBaskerville.ttf"))
pdfmetrics.registerFont(TTFont("LibreBaskerville-Italic", "/usr/share/fonts/libre-baskerville/LibreBaskerville-Italic.ttf"))
pdfmetrics.registerFont(TTFont("LibreBaskerville-Bold", "/usr/share/fonts/libre-baskerville/LibreBaskerville-Bold.ttf"))
pdfmetrics.registerFont(TTFont("LibreBaskerville-BoldItalic", "/usr/share/fonts/libre-baskerville/LibreBaskerville-BoldItalic.ttf"))
pdfmetrics.registerFontFamily("LibreBaskerville", normal="LibreBaskerville", bold="LibreBaskerville-Bold",
                              italic="LibreBaskerville-Italic", boldItalic="LibreBaskerville-BoldItalic")
```

With `registerFontFamily`, `<b>` and `<i>` tags in Platypus `Paragraph` objects automatically
use the correct font variant.

If the user requests a specific font that is not preinstalled, download it with `curl` or `apk`
and register the TTF file.

### Formatting Best Practices

- **Use Platypus** instead of raw Canvas for anything beyond a one-page throwaway. Platypus
  handles page breaks, flowing content, and consistent margins automatically.
- **Never use emoji or Unicode special characters** (e.g. `🎨📊`, `₀₁₂₃`, `⁰¹²³`). ReportLab
  cannot render emoji — they appear as empty boxes with all available fonts. Unicode
  superscript/subscript characters also render as black boxes. Use `<sub>` and `<super>` markup
  inside `Paragraph` objects for super/subscripts, and use text labels or drawn shapes instead of
  emoji.

- **Always set explicit table column widths** to prevent columns from collapsing or overflowing:

  ```python
  from reportlab.lib.pagesizes import letter
  usable = letter[0] - 2 * 72  # 1-inch margins
  col_widths = [usable * f for f in [0.4, 0.2, 0.2, 0.2]]
  ```

- **Wrap long strings in `Paragraph()` objects** inside table cells so they word-wrap properly.

### Simple one-page PDF (Canvas)

```python
from reportlab.lib.pagesizes import letter
from reportlab.pdfgen import canvas

c = canvas.Canvas("/tmp/output.pdf", pagesize=letter)
w, h = letter
c.setFont("Helvetica-Bold", 18)
c.drawString(72, h - 72, "Hello World")
c.setFont("Helvetica", 12)
c.drawString(72, h - 100, "Generated with reportlab.")
c.save()
```

### Multi-page report (Platypus)

```python
from reportlab.lib.pagesizes import letter
from reportlab.platypus import SimpleDocTemplate, Paragraph, Spacer, Table, TableStyle, PageBreak
from reportlab.lib.styles import getSampleStyleSheet
from reportlab.lib import colors
from reportlab.lib.units import inch

doc = SimpleDocTemplate("/tmp/report.pdf", pagesize=letter,
                        leftMargin=72, rightMargin=72,
                        topMargin=72, bottomMargin=72)
styles = getSampleStyleSheet()
story = []

# Title
story.append(Paragraph("Quarterly Report", styles["Title"]))
story.append(Spacer(1, 0.3 * inch))

# Body
story.append(Paragraph(
    "This report summarizes Q4 performance across all product lines. "
    "Key metrics are presented in the table below.",
    styles["Normal"]
))
story.append(Spacer(1, 0.2 * inch))

# Table with explicit column widths
data = [
    ["Product", "Revenue", "Growth"],
    ["Widget A", "$1.2M", "+12%"],
    ["Widget B", "$0.8M", "+5%"],
    ["Widget C", "$2.1M", "+18%"],
]
usable_w = letter[0] - 144  # page width minus margins
t = Table(data, colWidths=[usable_w * 0.5, usable_w * 0.25, usable_w * 0.25])
t.setStyle(TableStyle([
    ("BACKGROUND", (0, 0), (-1, 0), colors.HexColor("#2c3e50")),
    ("TEXTCOLOR", (0, 0), (-1, 0), colors.white),
    ("FONTNAME", (0, 0), (-1, 0), "Helvetica-Bold"),
    ("ALIGN", (1, 0), (-1, -1), "CENTER"),
    ("GRID", (0, 0), (-1, -1), 0.5, colors.grey),
    ("ROWBACKGROUNDS", (0, 1), (-1, -1), [colors.white, colors.HexColor("#ecf0f1")]),
]))
story.append(t)
story.append(PageBreak())

# Page 2
story.append(Paragraph("Appendix", styles["Heading1"]))
story.append(Paragraph("Additional details would go here.", styles["Normal"]))

def add_page_number(canvas_obj, doc_obj):
    """Draw centered page number in the footer."""
    canvas_obj.saveState()
    canvas_obj.setFont("Helvetica", 9)
    canvas_obj.drawCentredString(
        doc_obj.pagesize[0] / 2.0,
        36,  # 0.5 inch from bottom
        f"Page {canvas_obj.getPageNumber()}"
    )
    canvas_obj.restoreState()

doc.build(story, onFirstPage=add_page_number, onLaterPages=add_page_number)
```

### Headers, footers, and page numbers

For documents that need repeating headers/footers or page numbers, use `BaseDocTemplate` with
`PageTemplate` and frame callbacks. See `advanced-pdf-generation.md` for a full working
example.

---

## 3 · Filling a Template PDF

PDFs don't support in-place text editing. The strategy is:

1. **Read the template** with pypdf to get page dimensions and existing content.
2. **Create an overlay PDF** with reportlab containing new content positioned precisely on the
   template's layout.
3. **Merge the overlay onto the template** using pypdf's `page.merge_page()`.

```python
from io import BytesIO
from pypdf import PdfReader, PdfWriter
from reportlab.pdfgen import canvas

# Read the template
reader = PdfReader("/tmp/template.pdf")
writer = PdfWriter()

for i, page in enumerate(reader.pages):
    box = page.mediabox
    pw, ph = float(box.width), float(box.height)

    # Create overlay with new content
    buf = BytesIO()
    c = canvas.Canvas(buf, pagesize=(pw, ph))
    c.setFont("Helvetica", 12)
    c.drawString(150, 680, "John Doe")       # position text on the template
    c.drawString(150, 650, "2026-02-27")
    c.save()
    buf.seek(0)

    # Merge overlay onto template page
    overlay = PdfReader(buf).pages[0]
    page.merge_page(overlay)
    writer.add_page(page)

with open("/tmp/filled.pdf", "wb") as f:
    writer.write(f)
```

### Coordinate system

PDF coordinates use origin at **bottom-left**, with y increasing upward. Units are points
(72 points = 1 inch). To position fields on a template:

1. Use pypdf to get page dimensions: `page.mediabox.width`, `page.mediabox.height`.
2. Place fields by estimating from known reference points (e.g. 72pt = 1 inch from an edge).
3. Iterate with small adjustments until positioning looks right.

---

## 4 · Common Pitfalls

- **"I want to edit the text in this PDF"** — PDFs don't support in-place text editing. Either
  (a) overlay new content on top, or (b) extract the content and rebuild from scratch with
  reportlab. Approach (b) gives better results but is more work. Be upfront about this limitation.
- **Emoji render as empty boxes** — ReportLab cannot render emoji with any available font. Never
  use emoji characters in PDF content. Use text labels or drawn shapes instead.
- **Unicode black boxes** — Unicode subscript/superscript characters (`₀₁₂₃`, `⁰¹²³`) also
  render as black boxes. Use `<sub>` / `<super>` markup inside `Paragraph` objects instead.
- **Tables overflowing the page** — Always set explicit `colWidths`. Wrap cell content in
  `Paragraph` objects for word-wrapping.
- **Fonts not rendering** — Register TTF fonts explicitly for characters outside basic Latin
  (math symbols, CJK, etc.). Noto Sans/Serif have excellent Unicode coverage.
- **Images stretched or squashed** — Always compute display height from the image's natural
  aspect ratio. Never hardcode both width and height independently. See
  `advanced-pdf-generation.md` for the correct pattern.
- **Coordinate confusion** — PDF origin is bottom-left with y going up. Image coordinates are
  top-left with y going down. When mapping between the two:
  `pdf_y = page_height - image_y_scaled`.

---

## 5 · Advanced Topics

For modifying existing PDFs (merge, split, rotate, watermark, encrypt/decrypt), headers/footers,
multi-column layouts, images, custom flowables, and complex page templates using
`BaseDocTemplate`, see `advanced-pdf-generation.md`.
