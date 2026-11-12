# Poster Harness

A standalone, editable BetterPoster-inspired LaTeX template. Default print size: **48 x 36 inches, landscape**. Default pane widths: **20% left / 60% center / 20% right**, or 9.6 / 28.8 / 9.6 inches. The selected purple (`#662E7D`) is Mila's official logo purple, taken from the [official Mila SVG](https://mila.quebec/sites/default/themes/mila_v1/logo.svg) used by `assets/logos/mila-color.pdf`. Pale green (`#B9DBC1`) is taken from Mila's [official website stylesheet](https://mila.quebec/sites/default/files/css/css_J4CCfVN5SWjedZp9IH4f0a8J8wc57536YUd18MesaZQ.css), where it is used for `theme-green` backgrounds and `ckeditor-style-theme-green` bullets. This is a verified website color, rather than a claim about a published brand manual. Latin Modern Sans completes the template design. Default print typography: side body text 41.76 pt, center body text approximately 56.73 pt, side headings 51.04 pt, paper title 48.72 pt, main takeaway 208.13 pt, and subtitle 96 pt. Font commands are centralized in `poster-style.sty`. The example is deliberately generic and contains no research results.

This is a reusable default for conference posters. Keep the format consistent across projects, adapting dimensions or required elements when a conference specifies them.

[View the poster template PDF](output/pdf/main.pdf)

## Agent-assisted workflow

### Create a separate poster project

Keep this checkout as the generic starter. Create an independent project with:

```sh
python3 scripts/new-poster.py ../my-conference-poster
```

Relative destinations are resolved from your current working directory. The
command refuses existing destinations and destinations inside the template.
It copies the TeX sources, assets, brief, outline, project instructions, and build scripts;
it leaves out Git history, generated PDFs/logs, and the starter skill. No Git
repository is initialized. Run it from the generic template, rather than a
previous research poster, so previous content and assets are not carried over.

Open the new folder in Codex and say:

> Read AGENTS.md and brief.md. Create a poster from the paper at [path]. Ask me
> for missing information and record my answers in brief.md. Keep the default
> format. Fill and show outline.md before editing content.tex, then build with
> make check and visually inspect the PDF.

You can fill in `brief.md` yourself or answer in chat. It records the source
material, conference requirements, audience, takeaway, author/contact details,
QR destinations, and restrictions. The starter `outline.md` plans the center
takeaway, left overview, right discussion evidence, and footer, with sources
for each claim and figure. Fill it and show it before building; keep it current
as the content changes. Build in the new folder; its output is
`output/pdf/main.pdf`.

### Optional Codex starter skill

The repository includes `skills/new-poster/SKILL.md`. It locates the reusable
template, creates a separate project, gathers missing brief information, and
hands over to that project's `AGENTS.md`. Its default template location is
`~/Projects/poster-harness`; edit that reference if you store it elsewhere.
The skill checks for `scripts/new-poster.py` to identify a usable template.
If that file and the template folder are missing, the skill clones
`https://github.com/aumona/poster-harness.git` there (or into your
specified template location), then checks for the script again. If the folder
exists but the script is missing, it asks how to proceed rather than overwriting it.
Unless you specify a destination, new posters go in `~/Projects/presentations/posters/`,
named `YYYY-MM-DD-venue-slug` using the poster presentation date. If the parent
folder, date, or venue/topic is unclear, the skill asks before creating it.
The template's location does not determine the poster's destination.

To make it available across Codex projects, link it once (the target must not
already exist):

```sh
mkdir -p ~/.agents/skills
ln -s ~/Projects/poster-harness/skills/new-poster ~/.agents/skills/new-poster
```

Start a fresh Codex session if needed to discover it, then ask:

> Use $new-poster to create an ICLR poster in ~/Projects/iclr-poster from the
> paper at [path]. Ask me for the remaining brief information.

The skill is optional; the creation command and project instructions also work
on their own. The template folder supplies reusable files, while the new poster
folder and the source material you identify supply the context for the work.

### Where poster content comes from

There is no fixed list of content folders automatically scanned. New posters
draw from paths and URLs recorded in `brief.md`, especially:

- **What exists, and where:** papers, TeX sources, results, figures, tables,
  datasets, and material discoverable from those locations.
- **Material to reuse:** previous posters, talks, notes, blog posts, and paper
  figures.

Within the new poster folder, create these folders as needed:

- `sources/` holds copied source material, data, and `citations.json`, a keyed
  register of source titles, authors, original paths/URLs, and local files.
- `figures/` holds reused or new figure scripts and uses local dependencies.
- `assets/` holds display images and generated figures, alongside QR codes
  and logos.

Older posters or talks are considered when referenced in the brief or
discovered through its listed sources. Copy relevant data and sources into
`sources/`, figure scripts into `figures/`, and selected images into `assets/`,
preserving provenance. Do not reuse their `_src/` files or poster layout files.
Keep precise evidence locators in `outline.md` and all build/figure dependencies
inside the new project; original paths and URLs in the source register record
provenance.

The template's `content.tex`, `main.tex`, `poster-style.sty`, and example assets
serve as layout/design references, not sources of research content or results.

### Build a poster with an agent

Use Codex or another coding agent to help turn a paper into a poster. The included `AGENTS.md` describes the template’s editing and layout conventions. You can also follow these steps manually; see [Edit](#edit).

1. Download the Overleaf source ZIP or sync your existing Git integration.
2. Copy this template into a sibling `poster/` folder, keeping paper sources separate.
3. Ask the agent to read the paper, select an accurate story, and populate `content.tex`. Copy selected figures into `assets/` so the poster stays portable; extracting, cropping, or resizing paper figures may require LaTeX or graphics tools.
4. Replace authors, affiliations, logos, references, contact details, and QR image/URL. Review scientific accuracy.
5. Run `make check`, inspect the PDF, and scan the QR. Upload the template folder to a separate Overleaf project if desired; select `main.tex` and pdfLaTeX.

Example request:

> Read the paper in ../paper/ and create a poster using this template. Write for three audiences: a one- or two-sentence takeaway for passersby in the center, a standalone one-minute summary and brief method on the left, and figure-led experiments/details for discussion on the right. Keep the default dimensions and visual style. Use only supported claims, copy selected original figures into assets/, compile, and visually inspect the PDF.

## How to write the poster

The supplied BetterPoster reference organizes the poster around three audiences. Use that editorial structure with this template's default dimensions, colors, and typography.

**Center: for people walking past.** Give the main finding in one or two plain-language sentences. Say what you learned and why it matters, rather than repeating the paper title or listing methods. Someone should understand the takeaway at a glance. A central figure is optional: keep it only when it communicates the main finding immediately. Put the paper/code QR and institution logos below it.

**Left: for people who stop to read.** Start with the paper title, authors, and affiliations. Write a short summary that works without the presenter: the problem, your contribution, the main supported result, and why a reader should care. Add a brief method section or a small explanatory diagram if needed. Aim for about a minute of reading. Use short bullets and bold a few key phrases so a photo of this pane remains useful later. Rewrite the abstract into poster language instead of pasting it verbatim.

**Right: for people who stop to talk.** Use this pane as a visual aid for explaining experiments and details. Favor labeled figures, comparisons, and compact tables with short captions. Include the setup, units, uncertainty, or limitations needed to interpret each result, while letting the presenter supply the longer explanation. Add or remove subsections as the story requires. Keep references and acknowledgments compact.

Choose the story and figures from the paper before filling the layout. Preserve empty space; every panel does not need to be full. All scientific claims, numbers, and citations must be supported by the source paper. Replace the instructional sample text, example affiliations, and QR destination for each real poster. This reference guides the writing; the default visual design is reusable across conferences.

## Build

Install TeX Live or MacTeX with `pdflatex` and the geometry, tikz/pgf, graphicx, lmodern, amsmath, amssymb, and hyperref packages. No shell escape, network access, SVG converter, or Python packages are required.

```sh
make check
```

Requires Make and Python 3. The command compiles twice, rejects oversized panels, and checks for one page, overfull boxes, and LaTeX/package warnings. Output: `output/pdf/main.pdf`. Open and visually inspect it before printing at **100% actual size**. `make clean` removes auxiliary files and retains the PDF. Building replaces the checked-in example PDF with your current content.

## Edit

The template is simple enough to edit by hand with basic LaTeX familiarity, locally or in Overleaf. Routine changes—text, authors, affiliations, links, and image filenames—are made in `content.tex` using the existing commands. AI assistance is optional.

Some tasks require more technical LaTeX or graphics skills: extracting and cropping figures from a paper, resizing them while keeping labels readable, adjusting tables, or changing panel and footer layouts. The panels have fixed dimensions, so longer content may need trimming or figure resizing to fit. After edits, rebuild and inspect the PDF for clipping, overlap, and readability.

- `content.tex`: all poster text, authors, affiliations, figures, references, QR destinations, and logos.
- `poster-style.sty`: layout, palette, typography, reusable blocks, and panel height checks.
- `main.tex`: entry point; usually leave unchanged.
- `assets/`: local PDF, PNG, or JPEG figures and logos. Logo provenance is in `assets/LOGO-SOURCES.md`. Institution marks retain their owners' rights; inclusion does not imply endorsement.

## Font-size controls

Six macros in `poster-style.sty` set font sizes directly in points. Left and right panes share the side settings; the center and its footer use the center settings.

```tex
% Both side panes
\newcommand{\SideTitleFontSize}{48.72}
\newcommand{\SideHeadingFontSize}{51.04}
\newcommand{\SideBodyFontSize}{41.76}

% Center pane
\newcommand{\CenterTitleFontSize}{208.1333}
\newcommand{\CenterHeadingFontSize}{69.3333}
\newcommand{\CenterBodyFontSize}{\CenterHeadingFontSize*(\SideBodyFontSize/\SideHeadingFontSize)}
```

Edit these definitions, or override individual sizes in `main.tex` after `\usepackage{poster-style}`:

```tex
\renewcommand{\SideBodyFontSize}{44}       % Both side panes' body text.
\renewcommand{\CenterHeadingFontSize}{72} % Center headings only.
```

The center body defaults to `CenterHeadingFontSize * SideBodyFontSize / SideHeadingFontSize` (approximately 56.73 pt), preserving the same heading/body ratio as the side panes. It updates automatically when those settings change; override `\CenterBodyFontSize` with a number to set it independently. Other defaults use the print sizes listed above. Line spacing scales proportionally. Captions and placeholder labels follow their pane's body setting. Authors, affiliations, references, and URLs use smaller type proportional to their pane's body setting; the center subtitle follows the center title setting. QR labels use the center heading setting. Images, panel geometry, and fixed gaps retain their dimensions. Run `make check` and inspect the PDF after changing sizes; larger text can require shorter content.

## Reusable logos

`assets/logos/` contains only PDF and PNG files, ready for pdfLaTeX. Existing vector PDFs are preferred; otherwise, original PNGs are used.

| Organization | Color file | White/reverse file |
| --- | --- | --- |
| Mila | `mila-color.pdf` | `mila-white.pdf` |
| Université de Montréal | `udem-color.pdf` | `udem-white.pdf` |
| Utah State University (USU / UState) | `usu-color.pdf` | `usu-white.pdf` |
| Brigham Young University (BYU) | `byu-color.pdf` | `byu-white.pdf` |
| Concordia University | `concordia-color.pdf` | — |
| McGill University | `mcgill-color.pdf` | `mcgill-red-reverse.pdf` (red shield / white wordmark) |
| NeurIPS | `neurips-color.png` | — |
| ICLR | `iclr-color.png` | — |
| ICML | `icml-color.png` | — |

All included logos work directly with pdfLaTeX. Keep existing PDFs when available; otherwise use an original PNG. Do not convert or modify logos merely to supply another format.

Use color versions on light backgrounds and white versions on dark backgrounds. USU and BYU color variants are navy; UdeM is blue and Mila uses its existing purple wordmark. The original Mila and UdeM icon PNGs are included as well. See `assets/LOGO-SOURCES.md` for provenance. The example uses the color PDFs directly from `assets/logos/`.

```tex
\includegraphics[height=1.2in]{assets/logos/usu-color.pdf}
\includegraphics[height=1.2in]{assets/logos/byu-white.pdf}
```

Reusable blocks:

```tex
\PosterSection{Results}{Your text}
\PosterBullets{\item First point. \item Second point.}
\PosterFigure{assets/result.pdf}{Figure 1. Caption.}{10in}
\PosterReferences{[1] Author. Title. Venue, year.}
\PosterQR{assets/qrcodes/my-qr.png}{Paper}{https://your-paper-url}
```

Replace `\FigurePlaceholder` calls with `\PosterFigure` calls. The image fits the column width and supplied maximum height without distortion. Prefer vector PDF figures with readable labels. References are plain LaTeX for a minimal build; copy verified entries from the paper and cite their numbers in the text.

The default footer contains two 5.8-inch QR images, inward-facing Paper/Code labels and outward arrows, followed by a centered Mila / McGill / ICLR logo row. QR positions and logo dimensions are defined for the default 48 x 36-inch layout; the three selected logos are displayed. Select assets through `\PaperQRFile`, `\CodeQRFile`, and the three `*LogoFile` macros in `content.tex`. Layout is defined by `\PosterFooterLayout` in `poster-style.sty`; `\PosterQRSize` controls both QR images. The center content reserves space above this footer.

### Generate QR codes locally

Use `scripts/make-qr.py` as the default generator. No package installation is
needed: it uses the bundled [Project Nayuki QR encoder](https://www.nayuki.io/page/qr-code-generator-library)
and writes PNG images with the Python standard library. The encoder is included
in `scripts/vendor/` with its MIT license and source attribution. Keep that
folder with the scripts. Build/check/clean also use only the standard library.

For each QR code, only the **destination URL** is required. Optional inputs are:

- **Filename:** defaults to `qr.png`; use `paper-qr.png` and `code-qr.png` when creating both so they do not overwrite each other.
- **Mode:** `light` by default (purple on white), or `dark` (white on purple).

All files are saved automatically to `assets/qrcodes/`; no output directory is needed or accepted.

Example prompt:

> Generate QR codes for paper: [URL] and code: [URL], using light mode. Update the poster's QR file references and rebuild it.

When generating from a prompt, use descriptive filenames for paper and code, update `\PaperQRFile` and `\CodeQRFile` in `content.tex`, and run `make check`.

Minimal command (saves `assets/qrcodes/qr.png`):

```sh
python3 scripts/make-qr.py "https://your-url"
```

Generate paper and code QR images directly from their destination URLs:

```sh
python3 scripts/make-qr.py https://your-paper-url paper-qr.png --mode light
python3 scripts/make-qr.py https://your-code-url code-qr.png --mode light
```

QR codes are always saved in the template's `assets/qrcodes/` folder, created automatically. Provide only a PNG filename, not a directory or path. Omitting the filename saves `assets/qrcodes/qr.png`. The examples above produce `assets/qrcodes/paper-qr.png` and `assets/qrcodes/code-qr.png`.

Use `--mode dark` for a purple background. The script applies these colors automatically:

| Poster background | QR foreground | QR background |
| --- | --- | --- |
| Light | Purple `#662E7D` | White `#FFFFFF` |
| Dark / purple | White `#FFFFFF` | Purple `#662E7D` |

Set `\PaperQRFile` and `\CodeQRFile` in `content.tex` to the generated files. The included examples encode **https://example.org** and **https://example.org/code**; replace both for a real poster. Generation runs locally without network access or package installation. The script preserves a four-module quiet zone. Scan-test the final codes, including the white-on-purple version, before printing. Regenerate or select matching images when changing the background; existing assets do not switch colors automatically.

### Generate QR codes online (optional)

To generate QR codes yourself in a browser, use [QRCode Monkey](https://www.qrcode-monkey.com/). Enter the paper or code URL and use a single foreground color with the recommended template palette:

| Mode | QR foreground | QR background |
| --- | --- | --- |
| Light (recommended) | Purple `#662E7D` | White `#FFFFFF` |
| Dark / purple | White `#FFFFFF` | Purple `#662E7D` |

Keep the standard square design and a clear quiet zone. Download a high-resolution PNG or vector PDF and **save it in `assets/qrcodes/`**, for example `paper-qr.png` and `code-qr.png`. Update `\PaperQRFile` and `\CodeQRFile` in `content.tex` to those paths (including the correct extension), then run `make check` and scan-test the final poster. Check the white-on-purple version on your intended scanning devices before printing.

The layout uses inch coordinates and reserved panel heights. Changing paper dimensions requires adjusting panel coordinates and widths too; it does not automatically reflow. The left pane provides a standalone overview; the center delivers the takeaway and an optional immediately readable figure; the right pane supports discussion with experiments and details. Shorten text rather than filling every available space.

## GitHub reuse

Publish this folder as its own repository and enable GitHub's template repository setting. Start each poster using **Use this template**, or copy the folder. Keep the starter generic; save research-specific content in each new project. No nested Git repository is initialized here. No license is selected: choose one before distributing your own template code; logo rights remain separate.
