
# Style Guide

To maintain the quality of The Cloud Security Testing Guide (CSTG), please follow these general rules.

1. Be factual, specific, and ensure paragraphs are focused on their heading.
2. Ensure information is creditable and up to date. Provide links and citations where appropriate.
3. Avoid duplicating content. To refer to existing content, link to it inline.

## Write for the Reader

Readers of the CSTG come from many different countries and have varying levels of technical expertise. Write for an international audience with a basic technical background. Use words that are likely understood by a non-native English speaker. Use short sentences that are easy to understand.

The web tool [Hemingway](https://hemingwayapp.com/) can help you write with clarity.

## Formatting

Use consistent formatting to help us review and publish content, and help readers to digest information. Write all content using [Markdown syntax](https://guides.github.com/features/mastering-markdown/#examples).

Please follow these further guidelines for formatting.


### Project Folder Structure

When adding articles and images, please place articles in the appropriate sub-section directory. Place images in an `images/` folder within the article directory. Here is an example of the project structure:

```sh
document/
 ├───Images/
 │   └───Chapters/
 |	 	└───0x0[example]/
 |	 		└───example.jpg
 ├───0x01-Overview.md
 ├───0x02-Amazon-AWS-Testing-Guide.md
 ├───0x02a-Platform-Overview.md
 ├───0x02b-Basic-Security-Testing.md
 ├───0x02c-Access-Methods.md
 ├───0x02d-Resource-Inventory.md
 ├───0x02.1-Compute-Services.md
 ├───0x02.1a-Amazon-EC2.md
 ├───0x02.1b-AWS-Lambda.md
 ├───0x02-1c-Amazon-ECR.md
 ├───0x02.2-Cloud-Data-Storage.md
 ├───0x02-2a-Amazon-S3.md
 ├───0x03-Microsoft-Azure-Testing-Guide.md
 ├───0x03a-Platform-Overview.md
 ├───0x03b-Basic-Security-Testing.md
 ├───0x03c-Access-Methods.md
 ├───0x03d-Resource-Inventory.md
 ├───0x03.1-Compute-Services.md
 ├───0x03.1a-Azure-Virtual-Machine.md
 ├───0x03.1a-Azure-Functions.md

```

### Code Syntax Highlighting

Use code fences with syntax highlighting for snippets. For example:

```md
    ```javascript
    if (isAwesome){
        return true
    }
    ```
```


### Inline Links

Add links inline. Use words in the sentence to describe them, or include their specific title. For example:

```md
This project provides a [style guide](style_guide.md). Some style choices are taken from the [Chicago Manual of Style](https://www.chicagomanualofstyle.org/).
```

### Bold, Italic, and Underline

Do not use bold, italic, or underlined text for emphasis.

You may italicize a word when referring to the word itself, though the need for this in technical writing is rare. For examples, see the section [Use Correct Words](#use-correct-words). Use underscores: `_italic_`.

## Language and Grammar

To make the CSTG consistent and pleasant to read, please check your spelling (we use American English) and use proper grammar.

The sections below describe specific style choices to follow.


### Second Person

Do not write in the first or third person, such as by using _I_ or _he_. When giving technical instruction, address the reader in the second person. Use a [zero or implied subject](https://en.wikipedia.org/wiki/Subject_(grammar)#Forms_of_the_subject), or if you must, use _you_.

> Bad: "He/she/an IT monkey would run this code to test..."  
> Better: "By running this code, you can test..."  
> Best: "Run this code to test..."

### Numbering Conventions

For numbers from zero to ten, write the word. For numbers higher than ten, use integers. For example:

> One broken automated test finds 42 errors if you run it ten times.

Describe simple fractions in words. For example:

> Half of all software developers like petunias, and a third of them like whales.

When describing an approximate magnitude of monetary value, write the whole word and do not abbreviate. For example:

> Bad: "Security testing saves companies $18M in beer every year."  
> Good: "Security testing saves companies eighteen million dollars in beer every year."

For specific monetary value, use currency symbols and integers. For example:

> A beer costs $6.75 today, and $8.25 tomorrow.

### Abbreviations

Explain abbreviations the first time they appear in your document. Capitalize the appropriate words to indicate the abbreviated form. For example:

> This project contains the source code for the Cloud Security Testing Guide (CSTG). The CSTG is a nice and accurate book.

### Lists and Punctuation

Use bulleted lists when the order is unimportant. Use numbered lists for sequential steps. For each line, capitalize the first word. If the line is a sentence or completes a sentence, end with a period. For example:

> Testing this scenario will:
>
> - Make the application safer.
> - Improve overall security posture.
> - Keep customers happy.
>
> To test this scenario:
>
> 1. Copy the code.
> 2. Open a terminal.
> 3. Run the code as root.
>
> Here are some foods to snack on while testing.
>
> - Apples
> - Beef jerky
> - Chocolate

For lists in a sentence, use serial or [Oxford commas](https://www.grammarly.com/blog/what-is-the-oxford-comma-and-why-do-people-care-so-much-about-it/). For example:

> Test the application using automated tests, static code review, and penetration tests.

### Use Correct Words

The following are frequently misused words and how to correct them.

#### _and/or_

While sometimes used in legal documents, _and/or_ leads to ambiguity and confusion in technical writing. Instead, use _or_, which in the English language includes _and_. For example:

> Bad: "The code will output an error number and/or description."  
> Good: "The code will output an error number or description."

The latter sentence does not exclude the possibility of having both an error number and description.

If you need to specify all possible outcomes, use a list:

> "The code will output an error number, or a description, or both."

#### _frontend, backend_

While it's true that the English language evolves over time, these are not yet words.

When referring to nouns, use _front end_ and _back end_. For example:

> Security is equally important on the front end as it is on the back end.

As a descriptive adverb, use the hyphenated _front-end_ and _back-end_.

> Both front-end developers and back-end developers are responsible for application security.

#### _whitebox_, _blackbox_, _greybox_

These are not words.

As nouns, use _white box_, _black box_, and _grey box_. These nouns rarely appear in connection with cybersecurity.

> My cat enjoys jumping into that grey box.

As adverbs, use the hyphenated _white-box_, _black-box_, and _grey-box_. Do not use capitalization unless the words are in a title.

> While white-box testing involves knowledge of source code, black-box testing does not. A grey-box test is somewhere in-between.

#### _ie_, _eg_

These are letters.

The abbreviation _i.e._ refers to the Latin `id est`, which means "in other words." The abbreviation _e.g._ is for `exempli gratia`, translating to "for example." To use these in a sentence:

> Write using proper English, i.e. correct spelling and grammar. Use common words over uncommon ones, e.g. "learn" instead of "glean."

