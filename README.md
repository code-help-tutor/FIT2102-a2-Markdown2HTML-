
# FIT2102 Programming Paradigms 2024 - Assignment 2: Markdown to HTML

## Due Date
- Friday, 18th October, 11:55 pm

## Weighting
- 30% of your final mark for the unit

## Interview
- SWOTVAC + Week 13

### Overview
- Students will create a parser for a subset of the Markdown specification using functional programming techniques in Haskell.
- Demonstrate understanding of functional programming, including documentation of design decisions.

### Submission Instructions
- Submit a zipped file named `<studentNo>_<name>.zip` which extracts to a folder named `<studentNo>_<name>`.
    - Must contain all code, report (named `<studentNo>_<name>.pdf`).
    - No additional Haskell libraries (except for testing).
    - Run `stack clean --full` before zipping.
    - Don't submit `node_modules` or `.git` folder.
    - Ensure code executes properly.
- Marking process:
    - Extract zip.
    - Copy submission folder contents.
    - Execute `stack build`, `stack test`, `stack exec main/npm run dev` for front end.

### Late Submissions
- Penalised at 5% per calendar day, rounded up.
- After seven days, receive zero marks and no feedback.

### Table of Contents
1. [Introduction](#introduction)
2. [Goals / Learning Outcomes](#goals--learning-outcomes)
3. [Scope of assignment](#scope-of-assignment)
4. [Exercises (24 marks)](#exercises-24-marks)
    - [Part A: (12 marks): Parsing Markdown](#part-a-12-marks-parsing-markdown)
        - [Aside - Text Modifiers (2 marks)](#aside---text-modifiers-2-marks)
        - [Footnote References (0.5 marks)](#footnote-references-05-marks)
        - [Images (0.5 marks)](#images-05-marks)
        - [Free Text (1 mark)](#free-text-1-mark)
        - [Headings (1 mark)](#headings-1-mark)
        - [Blockquotes (1 mark)](#blockquotes-1-mark)
        - [Code (1 mark)](#code-1-mark)
        - [Ordered Lists (2 marks)](#ordered-lists-2-marks)
        - [Tables (3 marks)](#tables-3-marks)
    - [Part B: (6 marks): HTML Conversion](#part-b-6-marks-html-conversion)
        - [Text Modifiers (1 mark)](#text-modifiers-1-mark)
        - [Images (0.5 marks)](#images-05-marks-1)
        - [Footnote References (0.5 marks)](#footnote-references-05-marks-1)
        - [Free Text (0.5 marks)](#free-text-05-marks)
        - [Headings (0.5 marks)](#headings-05-marks)
        - [Blockquotes (0.5 marks)](#blockquotes-05-marks)
        - [Code (0.5 marks)](#code-05-marks)
        - [Ordered Lists (1 marks)](#ordered-lists-1-marks)
        - [Tables (1 mark)](#tables-1-mark)
    - [Part C (6 marks): Adding extra functionality to the webpage](#part-c-6-marks-adding-extra-functionality-to-the-webpage)
    - [Part D (up to 6 bonus marks): Extension](#part-d-up-to-6-bonus-marks-extension)
5. [Report (2 marks)](#report-2-marks)
6. [Code Quality (4 marks)](#code-quality-4-marks)
7. [Marking breakdown](#marking-breakdown)
8. [Correctness](#correctness)
9. [Changelog](#changelog)

### Minimum Requirements
- Parse up to and including code blocks (with high code quality and good report) for a passing grade.
- Higher mark requires parsing more difficult data structures and modifying the HTML page.

### Introduction
- Develop a transpiler to convert Markdown to HTML in Haskell.
- Web page sends Markdown to Haskell backend server, which converts and returns HTML.
- Use materials from previous weeks, reference external sources.
- Assignment split into parsing, pretty printing, and extras. Recommend doing Part A and B in tandem.
- Parse based on Markdown specification with restrictions.

### Goals / Learning Outcomes
- Use functional programming and parsing effectively.
- Understand and use key functional programming principles.
- Apply Haskell and FP techniques to parse Markdown.

### Scope of assignment
- Parse expression into data types and convert to HTML string.
- Not required to render Markdown or HTML strings.

### Exercises (24 marks)
#### Part A: (12 marks): Parsing Markdown
- Parse markdown string into Algebraic Data Type (ADT).
- Define own ADT and functions to parse requirements.
- ADT should have enough info for HTML conversion.
- Add `deriving Show` to ADT and custom types.
- Export `markdownParser :: Parser ADT` and `convertADTHTML :: ADT -> String`.
- Example scripts:
    - Run `stack test` to parse Markdown and save output.
    - Use `npm run dev` with `stack run main` to test in real-time.

#### Part A - Details
##### Aside - Text Modifiers (2 marks)
- Italic Text: `_italics_`
- Bold Text: `**bold**`
- Strikethrough: `~~strikethrough~~`
- Link: `[link text](URL)`
- Inline Code: `` `code` ``
- Footnotes: `[^ℤ+]` (e.g., `[^1]`, `[^2]`)
    - No whitespace inside `[` and `]`.
    - No nested modifiers.
    - Text inside modifiers can have whitespace (excluding new lines).

##### Images (0.5 marks)
- `![Alt Text](URL "Caption Text")`
    - Alt Text, URL, Caption Text don't consider text modifiers.
    - At least one non-newline whitespace between URL and caption text.
    - No spaces after `!` and before `[`.

##### Footnote References (0.5 marks)
- `[^2-^]: text` (ignore leading whitespace before text).

##### Free Text (1 mark)
- Any text not following other types, may contain modifiers.

##### Headings (1 mark)
- `# Heading 1` to `###### Heading 6`
- Alternative syntax: `Heading 1` followed by `======` for level 1, `Heading 2` followed by `------` for level 2.
- Headings may have modifiers.

##### Blockquotes (1 mark)
- Start with `>` at the beginning of a line.
- Ignore leading whitespace after `>` and before text.
- Text inside can have modifiers.
- No nested block quotes.

##### Code (1 mark)
- Starts and ends with three backticks (```), with optional language identifier.
- Code block should not consider text modifiers.

##### Ordered Lists (2 marks)
- Starts with number 1, followed by `.` and at least one whitespace.
- Can have sublists with 4 spaces before each item.
- Items may contain text modifiers.
- Don't handle unordered lists.

##### Tables (3 marks)
- Use pipes `|` to separate columns and dashes `-` to separate header and content rows.
- Compulsory beginning and ending pipes.
- Each row must have the same amount of columns.
- Cells may contain text with modifiers, ignore leading and trailing whitespace.

#### Part B: (6 marks): HTML Conversion
- Convert ADT to HTML representation.
- Indent HTML with 4 spaces to reflect tree structure.
- HTML must be a self-contained webpage with appropriate tags.

#### Part B - Details
##### Text Modifiers (1 mark)
- Italics: `<em>italics</em>`
- Bold: `<strong>bold</strong>`
- Strikethrough: `<del>strikethrough</del>`
- Link: `<a href="URL">link text</a>`
- Inline Code: `<code>code</code>`
- Footnotes: `<sup><a id="fn1ref" href="#fn1">1</a></sup>`

##### Images (0.5 marks)
- `<img src="URL" alt="Alt Text" title="Caption Text">`

##### Footnote References (0.5 marks)
- `<p id="fn1">My reference.</p>`

##### Free Text (0.5 marks)
- `<p>Text</p>`

##### Headings (0.5 marks)
- `<h1>Heading 1</h1>` to `<h6>Heading 6`

##### Blockquotes (0.5 marks)
- `<blockquote><p>Text</p></blockquote>`

##### Code (0.5 marks)
- `<pre><code class="language-haskell">Code</code></pre>`

##### Ordered Lists (1 marks)
- `<ol><li>Item</li></ol>`

##### Tables (1 mark)
- `<table><tr><th>Header</th><td>Data</td></tr></table>`

#### Part C (6 marks): Adding extra functionality to the webpage
- Add a button to save converted HTML using Haskell (saved with ISO 8601 formatted time).
- Add an input box to change the page title.
- Involves changes to HTML page and TypeScript code.

#### Part D (up to 6 bonus marks): Extension
- Implement interesting, impressive, or "show-off" features related to Haskell, FP, or parsing.
- Suggestions:
    - Markdown validation (e.g., enforce table column width).
    - Correct BNF for parsing in report (worth 2 marks).
    - Further extensions to webpage using RxJS.
    - Parse nested text modifiers.
    - Parse further Markdown parts.
    - Comprehensive test cases.

### Report (2 marks)
- Max 600 words.
- Summarise code intention, highlight interesting parts and difficulties.
- Include:
    - Design of code (data structures, approach, structure, architecture choices).
    - Parsing (usage of parser combinators, choices, construction using typeclasses).
    - Functional Programming (small functions, composition, declarative style).
    - Haskell Language Features Used (typeclasses, custom types, higher order functions, composition).
    - Description of Extensions (if applicable).

### Code Quality (4 marks)
- Readable and functional code, comment when necessary.
- Keep lines < 80 characters.
- Comment non-trivial functions and unclear code sections.
- Functions should be small, modular, use built-in functions.
- Reuse previous functions, avoid repeating work.
- Well-structured ADT.

### Marking breakdown
- Main marking criteria for each parsing and pretty printing exercise: correctness (50%) and FP style (50%).
- Provided with sample input and tests, encouraged to add own for edge cases.

### Correctness
- Series of tests to determine marks based on passed tests.

### FP Style
- Relates to code alignment with unit content and functional programming.
- Apply course concepts effectively.
- Avoid imperative style coding.

### Changelog
- Added note about non-empty text modifiers.
- Added note about BNF simplification for non-context-free parsers.
- Removed requirement to parse nested text modifiers (made it an extension).
- Fixed issue in scaffold (frontend HTML output).
- Changed "Abstract Data Type" to "Algebraic Data Type".
- Clarified image URL and caption text rules.

## Running the Code

```
$ stack test
```

This will generate the HTML files using the sample input markdown files, by running your code for each exercise.

All example markdown files are stored within `examples/input` and the output of your parser will be saved in `examples/output`.


## Running the Interactive Page

In the Haskell folder run:

```
$ stack run
```

In a separate terminal, in the javascript folder run:

```
$ npm i
$ npm run dev
```

You can type markdown in to the LHS of the webpage and inspect the converted HTML.
# FIT2102 a2 Markdown2HTML 

# CS Tutor | 计算机编程辅导 | Code Help | Programming Help

# WeChat: cstutorcs

# Email: tutorcs@163.com

# QQ: 749389476

# 非中介, 直接联系程序员本人
