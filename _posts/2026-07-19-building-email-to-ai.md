---
layout: single
title: "Building email-to-ai for a real problem"
category: notes
tag: ai
author_profile: false
sidebar:
    nav: "counts"
---

There is something especially rewarding about using what I have learned to solve a real problem for someone close to me.

🔗 **GitHub repository and Windows installer:**
[github.com/Dae-Y/email-to-ai](https://github.com/Dae-Y/email-to-ai)


Over the past few years, my father had accumulated hundreds of exported Outlook emails related to strata matters. They were stored as `.msg` and `.eml` files across different folders, often with important PDF, Word, or image attachments inside them.

The files were not broken. He could still double-click each one and open it in Outlook.

The problem was finding anything again.

Looking for an old decision, date, agreement, meeting discussion, or attachment meant opening emails one at a time and trying to remember which folder or message contained the information. When the archive covered several years, this became very inefficient.

As AI tools became more useful for reviewing documents and answering questions across large collections of information, I started thinking about whether they could help.

The basic idea seemed simple:

> What if an old email archive could be converted into a clean package that an AI assistant could actually review?

That question eventually became my side project, **email-to-ai**.

## The problem was not just the file extension

At first, I thought the main issue was that AI tools did not support `.msg` or `.eml` files very well.

That turned out to be only part of the problem.

Some AI assistants can read an individual `.eml` file. However, being able to open one email is very different from preparing hundreds or thousands of emails for useful analysis.

A raw email file contains much more than the message that appears in Outlook. Depending on the format, it can include:

* routing and delivery headers
* MIME boundaries
* HTML and plain-text versions of the same message
* repeated signatures and confidentiality notices
* quoted copies of previous replies
* inline company logos and social media icons
* tracking links and invisible elements
* encoded attachments
* inconsistent dates, filenames, and character encodings

An AI model may technically be able to read this data, but much of its context window can be wasted on duplicated or irrelevant material.

The distinction eventually became clearer to me:

> The project is not for reading one email. It is for preparing an entire email archive for AI-assisted review.

This also changed how I thought about the product.

It was not simply another `.msg`-to-PDF converter. Existing conversion tools are usually designed to create a document that a person can view or print. My goal was to create a structured, traceable package that an AI system could navigate more effectively.

## Why I chose a local-first design

The emails my father was working with could contain personal information, contracts, meeting records, financial details, and material related to disputes or property management.

Uploading the original archive to a random online converter did not feel appropriate.

Some enterprise platforms already provide sophisticated email search, compliance, or eDiscovery tools, but they can be too expensive and complicated for an individual user with an old folder sitting on a personal computer.

I wanted the conversion itself to happen locally.

The application does not need an LLM, an API key, a cloud backend, or access to the user’s live inbox. It scans exported `.msg` and `.eml` files on the computer and produces a package that can later be used with the AI assistant chosen by the user.

This separation felt important.

The converter prepares the data, but it does not decide where the user must analyse it.

The user can choose ChatGPT, Claude, Gemini, NotebookLM, another tool, or simply keep the organised output as an archive.

## Starting with the Python core

I began with the least glamorous but most important part: the processing pipeline.

The first version was written in Python and could:

* parse Outlook `.msg` and standard `.eml` files
* extract sender, recipient, date, subject, and body information
* convert email bodies into consistent Markdown
* preserve original attachments
* separate inline and boilerplate assets
* identify and deduplicate repeated files
* generate email and attachment manifests
* build sender profiles
* split large archives into AI-sized Markdown chunks
* produce an archive index and upload guide
* record warnings and parsing errors
* package the result into a ZIP file

The goal was not to produce the prettiest possible document.

Consistency and traceability were more important.

An email in the output needed a predictable structure showing who sent it, when it was sent, what the subject was, what attachments belonged to it, and where it came from.

I also wanted to preserve the connection between each email and its attachments. If an AI assistant later mentioned a contract or report, the user should be able to trace it back to the original message rather than receiving an answer with no evidence trail.

This part became more complicated than I initially expected.

Email archives are messy.

One message might contain a clean plain-text body. Another might contain deeply nested HTML. Some attachments are incorrectly marked as inline. A tiny social media logo might appear hundreds of times under different names. A reply may contain the full history of the conversation, causing the same content to appear repeatedly across many files.

The more real emails I tested, the more exception cases appeared.

That was also where the project became interesting. The value was not in calling one parsing library. It was in gradually building rules around all the awkward material that appears in real archives.

## Validating the first MVP with real emails

I deliberately avoided beginning with a polished desktop application.

Before spending time on packaging and design, I wanted to know whether the output was genuinely useful.

The first interface was a lightweight local Streamlit app built on top of the Python pipeline. It allowed a user to choose email files, run the conversion, and download the generated package through a browser-based interface.

The initial real-world test used around 40 Outlook emails.

All of them were parsed successfully, and the application extracted the messages and attachments, generated the manifests, removed some repeated assets, and produced readable Markdown.

The more important test came afterward: putting the results into an AI assistant and asking questions about the archive.

The AI was able to identify important people, dates, requests, responses, attachments, and the general timeline of the correspondence.

That was the first point when the project felt genuinely useful rather than simply technically functional.

Later, we tested approximately 100 emails through the Streamlit interface on my father’s Windows computer.

This was valuable because it was no longer just me using my own development environment. A non-technical user was able to run the application, convert a real archive, and use the generated files for practical AI-assisted consultation.

My father started using the Markdown chunks and extracted attachments inside a Claude Project to ask questions across old strata-related correspondence.

That was exactly the use case I had imagined at the beginning.

## The test that broke the Streamlit approach

The next archive was much larger: approximately 1,600 `.msg` and `.eml` files.

This test exposed a problem.

Only around 1,480 files reached the Streamlit backend. Some of the remaining files showed an `AxiosError: Network Error`, even though those same files still opened normally in Outlook.

The parser was not rejecting corrupt emails.

The bottleneck was earlier in the workflow.

The browser was uploading a very large number of local files to the Streamlit process, even though both were running on the same computer. That architecture was convenient for a small prototype, but it was not reliable enough for the scale of the real archive.

At first, this was frustrating. After thinking about it, I realised it was actually useful feedback.

The failure told me two things:

1. The Python processing engine was still doing its job.
2. The interface and delivery architecture had reached their limit.

This is one of the lessons I have taken from the project.

A successful small test does not necessarily validate the complete system. Testing with realistic scale can reveal problems that are invisible in a neat sample folder.

The larger archive gave me a real reason to move beyond the prototype.

## Moving from Streamlit to Tauri

I had already considered turning the project into a desktop application, but the 1,600-email test made the decision much clearer.

The Streamlit workflow still required several steps:

1. open PowerShell
2. activate a Python virtual environment
3. start the Streamlit command
4. open the local browser page
5. upload the email folder
6. run the conversion
7. find the downloaded result

That was acceptable for development and early validation, but not for the person I was actually building the application for.

The intended experience was much simpler:

1. install the application
2. open it from the desktop or Start menu
3. choose the email folder
4. choose the output folder
5. select **Convert**
6. open the completed result

A native desktop application could also avoid the browser upload stage entirely.

Instead of copying hundreds or thousands of files through a browser component, it could use a native folder picker and pass the actual directory path directly to the processing engine.

The new flow became:

```text
Native folder picker
→ direct filesystem scan
→ Python processing pipeline
→ local output package
```

For the desktop version, I chose **Tauri 2**, with a TypeScript frontend and Rust command layer.

I considered other approaches, including Electron and a FastAPI-based web application. However, I wanted the application to remain lightweight and local, without running a localhost web server or uploading private email data to a cloud backend.

Tauri provided a native desktop shell while using the operating system’s webview, making it lighter than bundling a complete browser runtime.

## Keeping the Python engine instead of rewriting everything

Moving to Tauri did not mean throwing away the existing project.

The email parsing, cleaning, attachment processing, deduplication, indexing, and exporting logic already existed in Python and had been tested with real data.

Rewriting the entire core in Rust or TypeScript would have added risk without directly improving the user’s outcome.

Instead, I kept the Python engine and treated Tauri as a new interface around it.

The resulting architecture became roughly:

```text
Tauri frontend
    ↓
Rust command layer
    ↓
Python sidecar
    ↓
email-to-ai processing pipeline
```

The Python engine is packaged into a standalone executable using PyInstaller. Tauri launches it as a sidecar process and exchanges structured commands and results with it.

This introduced a completely new set of challenges for me:

* connecting Rust commands to the frontend
* passing Windows paths safely
* handling folders containing spaces and Unicode characters
* packaging a Python application as an executable
* locating bundled resources after installation
* capturing progress, output, and errors from a sidecar process
* producing a Windows installer
* testing both development and packaged modes
* confirming that sensitive files were not accidentally included in release artefacts

It was my first time building and publicly releasing a native Windows application.

## Learning a new stack with coding agents

AI coding tools helped me move into Tauri and Rust faster than I could have done entirely alone.

However, the process was not effortless.

An agent could scaffold a command, suggest configuration, or help explain an unfamiliar compiler error. It could not take responsibility for whether the final application was safe, correctly packaged, or genuinely usable.

There were still many moments where I had to inspect paths, compare development and release behaviour, read logs, verify generated files, and understand why something worked in one environment but failed in another.

I also learned that using coding agents effectively involves more than sending a large request and hoping for a complete application.

Different tasks benefited from different approaches. Lightweight scaffolding and repetitive fixes could be handled by faster models, while architectural changes required more careful reasoning and smaller, reviewed steps.

I had to keep the work organised across two development environments as well:

* WSL for much of the Python core development
* native Windows for Tauri, Rust, folder-picker, sidecar, and installer work

GitHub became the synchronisation point between them. I avoided manually copying project folders because that would have made it easy to lose track of which environment contained the latest changes.

This setup made the project more complicated, but it also taught me a lot about cross-platform development and release workflows.

## From a script to an actual application

A command-line tool can be technically complete while still being unusable for the intended audience.

That difference became very clear during this project.

For me, running a Python command inside a virtual environment is normal. For my father, those steps are unrelated obstacles standing between him and the result he needs.

Packaging changed the project from:

> Here is some code that can solve the problem.

to:

> Here is an application that the intended user can install and operate.

That transition required work that is easy to underestimate:

* creating a preflight scan before conversion
* showing how many `.msg` and `.eml` files were found
* reporting unsupported files and total archive size
* allowing native input and output folder selection
* displaying conversion results, warnings, and errors
* opening the output folder after completion
* producing a self-contained installer
* generating checksums for release files
* confirming that the application did not require Python to be installed separately

By the time the native Windows build was ready, the existing test suite contained 45 tests covering areas such as archive generation, attachment deduplication, boilerplate handling, sender profiles, sign-off cleaning, and general smoke testing.

Passing the automated tests did not guarantee that the desktop experience was perfect, but it gave me much more confidence that packaging changes had not silently broken the original processing engine.

## The first public MVP

I have now published the first public MVP release of **email-to-ai**.

It supports `.msg` and `.eml` archives and produces clean Markdown chunks, archive indexes, manifests, sender profiles, preserved attachments, error reports, and an AI upload guide.

The conversion happens locally, and the application itself does not require an AI model or API key.

The most satisfying part is still not the technology stack or the release page.

It is the fact that my father is already using the output to find information across an archive that was previously very difficult to search.

The project began because I wanted to make one repetitive task easier for him. Everything else—the parser architecture, Streamlit prototype, large-scale failure, Tauri migration, Rust bridge, Python sidecar, and Windows installer—grew out of trying to solve that original problem properly.

## What still needs improvement

The application is not finished.

Real email data contains many difficult cases, and there are still several areas I want to improve:

* more reliable removal of repeated quoted reply chains
* better handling of unusually complex HTML emails
* stronger filtering of signatures and confidentiality notices
* improved classification of inline images and real attachments
* better compression of repeated boilerplate
* clearer progress reporting for very large archives
* more testing across unusual `.msg` and `.eml` files
* simpler guidance for choosing which generated files to upload to an AI assistant

I also want to be careful not to overcomplicate the application.

It would be easy to add accounts, cloud storage, built-in AI APIs, automatic document summarisation, payments, and many other features. However, those additions could weaken the local-first simplicity that makes the current tool useful.

For now, I prefer a focused application that does one job:

> Turn a messy legacy email archive into a clean, traceable, AI-ready package.

## A useful reminder before Semester 2

Semester 2 is about to begin, along with my Honours research, UniPASS sessions for UCP and DSA, an industrial mathematics project, certification study, and other commitments.

That makes me especially glad that I managed to bring this idea from an initial script to a working public MVP before everything becomes busy again.

The project also reminded me why I enjoy software development.

It is easy to become distracted by new frameworks, models, and trends. Those things are interesting, but the most meaningful starting point is often much simpler:

Someone has a real problem.

You understand enough of it to begin.

Then you keep improving the solution until they can actually use it.
