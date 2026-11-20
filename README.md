# Styling Maple graphics with Ti*k*Z and LaTeX

[![super-linter](../../actions/workflows/super-linter.yml/badge.svg)](../../actions/workflows/super-linter.yml)

This repository holds digital resources associated with the article "Styling
Maple graphics with Ti*k*Z and LaTeX" [[1](#references)]. The article discusses
a workflow that separates mathematical computation from typographic
presentation. The Maple mathematics software package generates graph geometry,
while Ti*k*Z and LaTeX control visual styling. A graph of the standard
cumulative distribution function was used to demonstrate the technique.

<br>
<table>
<tr>
<td width="58%">
<figure>
  <div align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/standard-normal-cumulative-distribution-graph-dm.svg">
    <img src="assets/standard-normal-cumulative-distribution-graph-lm.svg" loading="lazy" alt="S-shaped graph of phi of z on the y-axis and z on the x-axis, with horizontal asymptotes at phi of z equals zero and one." width="100%">
  </picture>
  </div>
</figure>
</td>
<td width="41%">
<sup>Figure 1. The standard normal cumulative distribution function Φ(<i>z</i>). Maple-generated: graph body. Ti<i>k</i>Z and LaTeX-generated: axes, bounding box, tickmarks and text.</sup>
</td>
</tr>
</table>

## Table of Contents

- [Key Files](#key-files)
- [Supplementary Files](#supplementary-files)
- [Software Requirements](#software-requirements)
- [Getting Started](#getting-started)
- [Next Steps](#next-steps)
- [Quality Assurance](#quality-assurance)
- [References](#references)
- [Citation](#citation)
- [License](#license)

## Key Files

| File                              | Notes            |
| :-------------------------------- | :--------------- |
| `src/graph-maple-distribution.mw` | Maple worksheet. |
| `src/graph-tikz-styler.tex`       | LaTeX document.  |

## Supplementary Files

| File                                       | Notes                                                          |
| :----------------------------------------- | :------------------------------------------------------------- |
| `out/graph-example-maple-bare-input.pdf`   | Example input bare Maple graph.                                |
| `out/graph-example-tikz-styled-output.pdf` | Example output Ti*k*Z and LaTeX-styled graph.                  |
| `src/graph-maple-distribution.mpl`         | Maple language version of the worksheet, text editor-friendly. |

## Software Requirements

| Software                              | Notes                                                                                                                                                                                                                                                                                                                |
| :------------------------------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| LaTeX                                 | [Available here](https://www.latex-project.org). Free.                                                                                                                                                                                                                                                               |
| Maple                                 | [Available here](https://www.maplesoft.com). Proprietary.                                                                                                                                                                                                                                                            |
| epstopdf<br>&nbsp;                    | [Details here](https://tug.org/epstopdf/). Free.<br>&nbsp;&nbsp;&nbsp;Optional. Called from Maple for PDF cropping.                                                                                                                                                                                                  |
| pdfcrop<br>&nbsp;<br>&nbsp;           | Free.<br>&nbsp;&nbsp;&nbsp;Optional. Called from Maple for PDF cropping.<br>&nbsp;&nbsp;&nbsp;TeX distributions: available in MiKTeX and TeX Live packages.                                                                                                                                                          |
| pdftops<br>&nbsp;<br>&nbsp;<br>&nbsp; | Free.<br>&nbsp;&nbsp;&nbsp;Optional. Called from Maple for PDF cropping.<br>&nbsp;&nbsp;&nbsp;Linux, macOS: included in poppler-utils or [Xpdf](https://www.xpdfreader.com/).<br>&nbsp;&nbsp;&nbsp;Windows: included in poppler-utils, [Xpdf](https://www.xpdfreader.com/) or the MiKTeX package miktex-poppler-bin. |
| Ghostscript<br>&nbsp;<br>&nbsp;       | [Available here](https://www.perl.org/). Free.<br>&nbsp;&nbsp;&nbsp;Optional. pdfcrop depends on it.<br>&nbsp;&nbsp;&nbsp;TeX distributions: included in [MiKTeX](https://miktex.org/) on Windows and [MacTeX](https://www.tug.org/mactex/).                                                                         |
| Perl<br>&nbsp;<br>&nbsp;<br>&nbsp;    | [Available here](https://www.perl.org/). Free.<br>&nbsp;&nbsp;&nbsp;Optional. pdfcrop depends on it.<br>&nbsp;&nbsp;&nbsp;TeX distributions: included in [TeX Live](https://www.tug.org/texlive/).<br>&nbsp;&nbsp;&nbsp;Linux, macOS: included in most systems.                                                      |

### LaTeX Configuration

Please ensure the LaTeX environment has the following packages installed:

- standalone.
- tikz.

## Getting Started

### Overview

The exemplar graph may be reproduced by:

- Running the Maple code to generate an underlying bare graph.
- Compiling `graph-tikz-styler.tex` to add graph axes, text, tickmarks, etc.

### Maple Graphics

The Maple worksheet should be run in Maple.

The worksheet graphs the standard normal cumulative distribution function. In
addition to default Maple output, a bare version with the graph body but no
text or axes, is output. The bare graph is used during Ti*k*Z styling,
discussed below.

For users without Maple:

- A Maple language version of the worksheet, accessible to text editors, is [available here](src/graph-maple-distribution.mpl).
- A pre-generated bare Maple graph is [available here](out/graph-example-maple-bare-input.pdf).

### Ti*k*Z Styling

The LaTeX document `graph-tikz-styler.tex` holds the file path to a
Maple-generated bare graph to import. That file path, associated with an
`\includegraphics` command, should be updated to match your desired target bare
graph. The pre-generated graph `graph-example-maple-bare-input.pdf` may be used
for testing.

The LaTeX document `graph-tikz-styler.tex` should then be compiled. Graph text
and axes will be reconstructed by Ti*k*Z, overriding Maple's styling.

For users without Ti*k*Z and LaTeX, a pre-generated example Ti*k*Z-styled graph
is [available here](out/graph-example-tikz-styled-output.pdf).

## Next Steps

This repository, and the associated article, give an example of how to style
Maple graphics with Ti*k*Z and LaTeX.

The code handles a specific example graph. Users are encouraged to adapt the
workflow for their own Maple graphics.

## Quality Assurance

The Maple code was tested in the following environment.

<details>
<summary>Maple: Windows Test Environment</summary>

<br>

| Type     | Component        | Version                           |
| :------- | :--------------- | :-------------------------------- |
| Platform | Operating system | Windows 11, 24H2                  |
| Software | Maple            | 2025.1                            |
| &quot;   | Perl             | Strawberry Perl (64-bit) 5.40.2.1 |

</details>

The LaTeX code has been tested in the following environment.

<details>
<summary>LaTeX: Windows Test Environment</summary>

<br>

| Type          | Component        | Version                                |
| :------------ | :--------------- | :------------------------------------- |
| Platform      | Operating system | Windows 11, 26H2 (OS Build 26300.9974) |
| Software      | MiKTeX           | 26.5                                   |
| LaTeX package | standalone       | 1.5a                                   |
| &quot;        | tikz             | 3.1.12                                 |

</details>

## References

1. T. Stenborg, "Styling Maple graphics with Ti*k*Z and LaTeX", <i>TUGboat</i>,
   vol. 46, no. 3, pp. 379&ndash;380, Oct. 2025, doi:
   10.47397/tb/46-3/tb144stenborg-maple.\
   [View PDF](https://tug.org/TUGboat/tb46-3/tb144stenborg-maple.pdf) &nbsp;
   [View at publisher](https://tug.org/TUGboat/tb46-3/tb144stenborg-maple.html)
   &nbsp; [SciX](https://scixplorer.org/abs/2025TUGbt..46..379S/abstract)

## Citation

Citation details are available by clicking **Cite this repository** in the
GitHub sidebar.

## License

This repository is licensed under the BSD-3-Clause license. Details are
available in the [LICENSE](./LICENSE) file.
