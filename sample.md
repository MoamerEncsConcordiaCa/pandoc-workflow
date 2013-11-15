---
title: Plain Text Workflow 
author: Dennis Tenen, Grant Wythoff
date: November 4, 2013
---
...

# Philosophy 
Authoring, storing, and retrieving documents are activities central to the humanities research workflow. And yet, many scholars base their practice on [proprietary tools](http://www.antipope.org/charlie/blog-static/2013/10/why-microsoft-word-must-die.html) and formats that fall short of even the most basic requirements of academic writing. The reader will relate to being frustrated with the fragility of footnotes, bibliographies, figures, and book drafts authored in MS Word. Still, most journals insist on submissions in .docx format. More than causing personal frustration, the reliance on fragile tools and formats has long-term negative implications for the community. In such an environment, journals must outsource typesetting, alienating authors from the material contexts of publication and furthermore adding unnecessary barriers to the unfettered circulation of knowledge. Closed formats ultimately lead to closed intellectual communities.

In this tutorial, we would like to suggest an alternative workflow built around open-source tools and transparent formats. Inspired by best practices in a variety of disciplines, we were guided by the following principles:

# Principles
1. *Preference for plain-text, fully transparent, human-readable formats.* When writing in Word or Google Docs, what you see is not what you get. The file contains hidden automatically-generated formatting characters. When things go wrong, the obfuscated typesetting layer is difficult to troubleshoot. Plain text ensures transparency and answers to the standards of long-term preservation. MS Word may go the way of Word Perfect in the future, but plain text will always remain easy to read, catalog, mine, and transform. As an additional feature, plain text enables easy and powerful versioning of the document. 

2. *Platform independence.* Writing in plain text means you can easily share, edit, and archive your documents in virtually any environment. Your plain text files will be accessible on cell phones, tablets, or, perhaps, on a low-powered terminal in some remote library. Plain text is backwards compatible and future proof. Whatever software or hardware comes along next, it will be able to understand your plain text files. 

3. *Separation of form and content.* Writing and formatting at the same time is distracting. The idea is to write first, and format later, as close as possible to the time of publication. A task like switching from Chicago to MLA formatting should be painless. Journal editors who want to save time on needless formatting and copy editing should be able to provide their authors with a formatting template which takes care of the typesetting minutia. 

4. *Support for the academic apparatus.* The workflow needs to handle footnotes, figures, international characters, and bibliographies gracefully.

5. *One source many destinations.* As the vectors of publication multiply, we need to be able to generate a multiplicity of formats including for slide projection, print, web, and mobile. Ideally, we would like to be able to generate most common formats without breaking bibliographic dependencies. Our workflow needs to be portable as well--it would be nice to be able to copy a folder to a thumbdrive and know that it contains everything needed for publication.

Markdown and LaTeX answer all of these requirements. We choose to author the primary text in Markdown (and not LaTeX) because it offers the most light-weight and clutter free syntax (hence, mark  *down*) and because when coupled with Pandoc it allows for the greatest flexibility in outputs (including .doc and .tex files).^[There are no good solutions for arriving at MS Word from LaTeX.]

# Use cases
In this tutorial, you will learn to use [Pandoc](http://johnmacfarlane.net/pandoc/)--a command line tool that converts plain text files a number of beautifully formatted file types: PDF, .docx, HTML, .tex, slide decks, and more. 

We will cover three use cases here. In the first, you will learn the basics of markdown--an easy to read and write plain text markup syntax -- and use Pandoc to generate a simple .pdf file.^[Don't worry if you don't understand any of this terminology yet!] In the second, we will add some figures and bibliography, generate an MS Word document, and then change the citation style form MLA to Chicago. Finally, we will use Pandoc in conjunction with [reveal.js](http://lab.hakim.se/reveal-js/#/) to make some attractive slides. File management and versioning using Git will be covered in another tutorial.

As a side-effect of this tutorial, you will be introduced to the basics of command line file management--a skill necessary for many more advanced research workflows. 

# Mindset
Readers with experience in data science, software development, or server administration may want to skip this section and go straight to software requirements. For all others, we would like to take a moment to discuss what to do when things go wrong. Something is likely to go wrong in any tutorial. The magic of computing is in its universal reproducibility. Software works in similar ways on varied platforms, on machines in various state of disrepair, and under the precarious conditions of competing standards, formats, and protocols. The *Programming Historian 2* has a good guide on "what to do if you get stuck."^[http://programminghistorian.org/lessons/troubleshooting] In addition to the many helpful tips there, we suggest learning about IRC channels which provide near-instantaneous help on many of the tools discussed in this article. Generally, your troubleshooting heuristic should start with googling the exact error message (by using double quotes in Google), asking the IRC channel, and finally posting to relevant Q&A sites like Stack Exchange or Digital Humanities Questions & Answers.

We this in mind, we purposefully omit some of the granular, platform- or hardware-bound details. For example, it makes no sense to provide installation instructions for Notepad++, when the Notepad++ website will always have instructions that are both more current and more complete. Similarly, the mechanics of Pandoc markdown are best explored by searching for "Pandoc markdown" on Google, with the likely first result being Pandoc's homepage.

Instead of following the tutorial in a mechanical way, you should strive to understand the solutions offered here as a methodology, which may need to be tailored further to fit your own environment. 

# Software requirements
You will need to following software installed on your computer:

* A **plain text editor**. For beginners, we recommend Text Wrangler on a Mac, Notepad++ for Windows, and gEdit for Linux. More advanced users may want to experiment with flavors of Emacs of Vim. It does not matter what you use as long as it is explicitly a plain text editor.

* **Unix terminal emulator**. Working "in the command line" is equivalent to typing commands into the terminal. On a Mac you simply need to use your finder for "terminal". On windows, you may need to install TERMINAL ON WINDOWS. Linux users are likely to be familiar with their terminals already.

* **Pandoc**.^[Detailed, platform-specific installation instructions available at <http://johnmacfarlane.net/pandoc/installing.html>.] Pandoc was created and is maintained by John MacFarlane at the University of California Berkeley. This is humanities computing at its best and will serve as the engine of our workflow. With Pandoc, you will be able to compile text and bibliography into beautifully formatted and flexible documents. To verify that Pandoc is installed, enter ```pandoc --version``` into the command line.

* **LaTeX**.^[Detailed, platform-specific installation instructions available at <http://johnmacfarlane.net/pandoc/installing.html>.] Although LaTeX is not covered in this tutorial, it is used by Pandoc for .pdf creation. Advanced users will often convert into LaTeX directly to have more granular control over the typesetting of the .pdf. Type ```latex -v``` to see if LaTeX was installed correctly (you will get an error if it was not and some information on the version if it was).

# Folder Structure
Modern operating systems are designed to obscure the file structure on your hard drive. There is a good reason for this: many system files have long and confusing names, and should not be touched. Taking a few moments to get in touch with your filing system will help orient your activities immensely. You need to learn only a few commands to get started. First, open your terminal window. You should see a prompt that looks something like this: ```[~]$```. The tilde indicates your "home" directory,^[In fact you can type ```cd ~``` at any point to return to your home directory, while ```/``` indicates the root, or the top-most directory on your drive.] and the dollar sign is just a prompt for you to type something. It is very likely that your "Documents" folder is located here. Type ```pwd``` and then hit enter to list the full name of your working directory. Use ```pwd``` whenever you feel lost in the command prompt.

The next command is ```ls``` (list) which simply lists the files in the current directory.^[Remember to hit enter after every command.] Finally, you can type ```cd DIRECTORY_NAME``` to change directories. Once you start typing the directory name, use the *tab* key to auto complete the text--this is particularly useful for long directory names, or directories names that contain spaces. 

These three commands: ```pwd```,  ```ls```,  and ```cd``` are all you need for this tutorial. Practice them for a few minutes to navigate you documents folder, and think about they way you have organized your files. At some point you should raise your terminal awareness by going through ^[TERMINAL THE HARD WAY].

You are likely to have some system of organizing your documents, projects, illustrations, and bibliographies. But often, your document, its illustrations, and bibliography live in different folders, which makes them hard to track. Our goal is to create a single folder for each project, with all relevant materials included. 


# Case 1: Markdown basics
In this scenario, we will create a Markdown file and use Pandoc to publish that text in a formatted PDF.

Markdown is a convention for structuring your plain-text documents semantically. The idea is to identify logical structures in your document (title, section, subsection, footnote, etc.), 
mark them with some unobtrusive characters, and then "compile" the resulting text with a typesetting interpreter which will format the document consistently, according to a specified style. 

Markdown conventions come in several "flavors"


To begin, create a folder into which you will save all of your projects. Open a new file using your plain text editor and save it as "project.md". In this file, type the following:

```
---
title: Plain Text Workflow 
author: Dennis Tenen, Grant Wythoff
date: November 4, 2013
---

# Philosophy 
Authoring, storing, and retrieving are *central* activities.^[Clarified here.] 

But many scholars work with [proprietary](http://bit.ly/HDjFBs) tools.

## Principles

- **Preference for Plain Text** and human readable formats.
- **Platform Independence** means share and edit in any environment.

# Use Cases

In this tutorial, you will learn the following.
```

At the top, nested between lines containing --- and ... you have a YAML metadata header. It declares the project's title, author, and date. Once this header has been created, you can begin with the body of your document.

A hashtag signals a header (```# Introduction```) while a double hashtag signals a subheader (```## Background```).  Italic text is encased in ```*single*``` asterisks, while bold text is signaled by ```**two**``` asterisks. A carat and brackets contains text that will be included as a footnote, and hyperlinks can be made by putting the text to be linked in brackets and the URL directly next to it in parentheses.

Save this file, and navigate to the directory containing it in your Terminal. If you are unfamiliar using the command line, the Scholars' Lab [Command Line Bootcamp](http://praxis.scholarslab.org/tutorials/bash/) will prove useful.  At the command prompt for your project directory, type:

```
pandoc -o project.pdf project.md
```

This command uses Pandoc to output (-o) a PDF filename of your choice, based on your Markdown file.  In order to number your section headers, include ```--number-sections``` and to include a table of contents at the top of the document, include ```--toc``` in your command, directly after the PDF file you have named.  Thus, the new command would look like this:

```
pandoc -o project.pdf --number-sections --toc project.md
```

If everything has gone smoothly, you will now have a PDF file in your project folder that looks like this:

![](PDFoutput1.png)

# Case 2: Working with Bibliographies

In this scenario, we will use Zotero to export our bibliographic references as BibTex files, and call those formatted citations in our project document using simple @tags, or "citation keys." [BibTex](http://www.bibtex.org/) is a plain text format for listing references. Though this export process can be automated using Robin Wilson's [AutoZotBib](http://rtwilson.com/academic/autozotbib) plugin (which provides the option to edit the default format of those citation keys), we will be covering a project-by-project model here.

If you are not using a reference manger like Endnote or Zotero, you should. Zotero slots into the free software toolkit much better than most of its for-profit competitors. Like Pandoc, it was created by the academic community, which welcomes efforts to extend the tool's functionality. Most modern reference managers are able to export bibliographies in BibTeX (.bib) format, which, being plain text and human-readible, complies with our stated principles. We will return to bibliographies in Case 2 of the tutorial.

Open Zotero (standalone or Firefox plugin version) and navigate to the folder containing the references you would like to use in your project. Click the settings wheel and select "export." Select BibTex, and save the resulting .bib file to the same directory containing your project. Let's call it "project.bib" for simplicity's sake.

If you open this project.bib file with your plain text editor, you will see a list of items in the following format:

```
@article{fyfe_digital_2011,
	title = {Digital Pedagogy Unplugged},
	volume = {5},
	url = {http://digitalhumanities.org/dhq/vol/5/3/000106/000106.html},
	number = {3},
	urldate = {2013-09-28},
	author = {Fyfe, Paul},
	year = {2011},
	file = {fyfe_digital_pedagogy_unplugged_2011.pdf}
}
```

Each entry contains a unique cite key, which is listed in the first row of text for each item. The default cite key exported by Zotero takes on the form "@fyfe_digital_2011," being the primary author's last name, first word in the title, and year separated by underscores.^[Again, the only good solution for editing this default cite key export format is the [AutoZotBib](http://rtwilson.com/academic/autozotbib) plugin. For the moment, however, it only lets you export your *entire* Zotero library, rather than an individual collection.]

Go back to your project.md file. In order for Pandoc to match your cite keys with full citation data, you will have to edit your metadata header so that it points to your BibTeX file.  Edit the header so that it now reads:

```
---
title: Plain Text Workflow 
author: Dennis Tenen, Grant Wythoff
date: November 4, 2013
bibliography: project.bib
---
```

Now you can quickly type a cite key into your project, and upon "typesetting" your document, that key will be replaced by a full citation. Edit the footnote in the first line of our project.md file to look as follows.

```
central activities.^[@fyfe_digital_2011 argues that too.]
```

Here, the text "central activities." will be followed by an automatically numbered footnote, which contains the text "Gratzinger argues that too."  At the bottom of your document, if you add another section with the heading #Bibliography, a formatted bibliography of all works you have cited will be placed at the bottom of your document. So, our new project.md text file will look like this:

```
---
title: Plain Text Workflow 
author: Dennis Tenen, Grant Wythoff
date: November 4, 2013
bibliography: project.bib
---

# Philosophy 
Authoring, storing, and retrieving are *central* activities.^[@fyfe_digital_2011 argues that too.]

But many scholars work with [proprietary](http://bit.ly/HDjFBs) tools.

## Principles

- **Preference for Plain Text** and human readable formats.
- **Platform Independence** means share and edit in any environment.

# Use Cases

In this tutorial, you will learn the following.

# Bibliography
```

The default citation format in Pandoc is Chicago author-date format. If you want to typeset using a different format (this will be immensely useful when submitting a project to a journal that requires specific citation styles), you can download a "citation style language" file and save it to your directory.^[John Macfarlane recommends the primer on creating and modifying CSL styles at <http://citationstyles.org/downloads/primer.html>. All available CSL styles can be searched and downloaded at <https://github.com/citation-style-language/styles>.]

Once the .csl file of your choice is downloaded into your directory, reference it in the YAML header in the same manner as your bibliography: ```csl: mla.csl```

Now, to compiling. In your terminal, type:

```pandoc -o project.pdf --filter pandoc-citeproc --number-sections --toc project.txt```

We have simply added ```--filter pandoc-citeproc``` to our last compile command, which will format all of our citations.

To tinker with the format of your PDF output (i.e. narrower margins, double spacing, etc.), you can reference a LaTeX style file (plain text as well, saved with a .sty extension) saved in the same directory, and referenced in your typesetting command:

```pandoc -H format.sty -o project.pdf --filter pandoc-citeproc --number-sections --toc project.txt``` 

# Case 3: Slides 

In this scenario, we will 
