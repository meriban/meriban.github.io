---
layout: single_content
title: "CILIP MDG Library Carpentry workshop, Birmingham, Aston University, 5th and 12th May 2026"
heading: "CILIP MDG Library Carpentry workshop"
location: Aston University, Birmingham
year: 2026
month: 06
day: [05, 12]
dates: [5th & 12th June 2026]
topics: [data types, encoding, data formats, spreadsheets, regex, openrefine]
event_link: https://meriban.github.io/2026-06-05-mdgbham/
description: 2-day Library Carpentry workshop designed for library metadata professionals who want to build confidence and practical skills in working with data. It is ideal for those who are new to data wrangling, as well as for anyone with limited prior experience or knowledge.
toc: true
toc_sticky: true
author_profile: true
categories: 
  - data types
  - encoding
  - data formats
  - tabular/delimited data formats
  - XML
  - JSON
  - spreadsheets
  - regular expressions
  - OpenRefine
read_time: true
sidebar:
  - title: "DTD"
    image: "/assets/images/dtd.png"
    image_alt: 
header:
  teaser: "/assets/images/workshop_placeholder.png"
---

The workshop focuses on working with data **outside a MARC environment**, with particular attention to formats commonly encountered in metadata work, such as delimited files, XML, and JSON. While the emphasis is on non-MARC data, the concepts, strategies, and tools covered are widely transferable and highly relevant across a range of library systems and workflows.

As with all Carpentries workshops, the goal is not to teach everything, but to significantly **lower the barrier to entry** and give participants the practical skills, foundational knowledge, and confidence to apply what they learn in their own work—and to continue developing their expertise independently.

### Day 1: Data types, encoding, data formats and working with spreadsheets

Day 1 introduces common **data types**, how character **encoding** works and why it matters, and the **data formats** frequently encountered in library metadata work. Understanding how data is structured, stored, and encoded is essential when working outside an LMS – and very useful when working in one.

The second half of the day focuses on data organisation, cleaning, and quality control, using **spreadsheet** applications. Participants will learn practical techniques for working with metadata in spreadsheets and identifying and fixing common data issues.


### Day 2: Regular expressions and OpenRefine

Day 2 introduces regular expressions, a powerful tool for matching, extracting, and transforming text. Regular expressions are widely used in contexts ranging from text editors and MARCEdit to Library Management Systems, and mastering them can take data work to the next level.

The second half of the day focuses on OpenRefine, a powerful open-source tool for cleaning messy datasets, reconciling values, and exploring data at scale. Participants will learn the basics of OpenRefine, including transforming and exporting data, as well as reconciling data against external sources such as Wikidata.

**Library Carpentries event page**: [{{ page.event_link }}]({{ page.event_link }})

### Workshop write-ups and materials

These are the written up version of content covered in the workshop. Often they also include extra information or deeper looks at topics than we had time for in the workshop itself. Each has an annotated bibliography and links to mentioned software and tools where applicable.

#### Write-ups

- [Data types](/notes/data_types/)
- [Data encoding](/notes/encoding/)
- [Tabular data](/notes/tabular_data), e.g. CSV, TSV
- [XML](/notes/xml), [DTD](/notes/dtd) and [XML Schema](/_notes/xsd/)
- [Namespaces](/notes/namespaces/)
- [JSON](/notes/json/)
- [Software and tools](/notes/non_marc_data_tools/)

#### Library Carpentry lessons
- [Library Carpentries lesson "Tidy data"](https://librarycarpentry.github.io/lc-spreadsheets/) Note: this lesson assumes that ones wants to use the data in the spreadsheets computationally. If you make spreadsheets designed to be consumed by humans different rules apply especially regarding formatting.
- [Library Carpentries lesson "Introduction to Regular Expressions"](https://librarycarpentry.github.io/lc-data-intro/)
- [Library Carpentries lesson "OpenRefine"](https://librarycarpentry.github.io/lc-open-refine/)

#### Slides
- Day 1: [CILIP_MDG_Library_Carpentry_workshop_Day_1.pptx](/assets/files/CILIP_MDG_Library_Carpentry_workshop_Day_1.pptx)
- Day 2: [Regular expressions](/assets/files/CILIP_MDG_Library_Carpentry_workshop_Day2_regex.pptx) and [OpenRefine](/assets/files/CILIP_MDG_Library_Carpentry_workshop_Day2_OpenRefine.pptx)

#### Other materials

- MARC files (encoding example):
  - [mnemonic .mrk file](/assets/files/CILIP_MDG_Library_Carpentries_workshop_demo_mrk_file.mrk)
  - [raw .mrc file](/assets/files/CILIP_MDG_Library_Carpentries_workshop_demo_mrc_file.mrc)
- [Regular expressions cheat sheet](/assets/files/CILIP_MDG_Library_Carptenry_regex_cheat_sheet.docx)
- Library Carpentry [regular expressions quiz printout](/assets/files/CILIP_MDG_Library_Carpentry_multiple_choice_quiz.docx)



