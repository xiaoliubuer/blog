---
layout: post
title:  "Markdown CheatSheet"
date:   2017-08-14 13:27:30 -0400
categories: articles
---

# 1. HEADER

```markdown
# This is an <h1> tag
#### This is an <h4> tag
###### This is an <h6> tag
```
Result:
# This is an <h1> tag
#### This is an <h4> tag
###### This is an <h6> tag

---

# 2. EMPHASIS
* __Use: \*  content  \*__

  **Syntax:** _* content *_

  **Example:**
  
  ```markdown
  *This text will be italic*
  ```
  **Result:**
  
  *This text will be italic*


* __Use: \_  content  \___

  **Syntax:**  _\_ content \__

  **Example:**
  
  ```markdown
  _This will also be italic_
  ```
  
  
  **Result:**
  
  _This will also be italic_

---

# 3. BLOCKQUOTES
**Syntax:** _> + "content"_

**Example:**
```markdown
> Hello World! This is a block.
> Yes, it is a block!
```
**Result:**
> I’ve always been more interested 

> in the future than in the past.

---

# 4. LISTS
#### 1) Unordered
**Syntax:** _* + "item"_

**Example:**
```markdown
* Item 1
* Item 2
	* Item 2a 
	* Item 2b
```
**Result:**
* Item 1
* Item 2
	* Item 2a 
	* Item 2b

#### 2) Ordered
**Syntax:** _number + "item"_

**Example:**
```markdown
1. Item 1 
2. Item 2 
3. Item 3
	* Item 3a 
	* Item 3b
```
**Result:**
1. Item 1 
2. Item 2 
3. Item 3
	* Item 3a 
	* Item 3b

---

# 5. IMAGES
**Syntax:** _\!\[Alt Text\]\(url \=width\*height)_

**Example:**
```markdown
![adam's avatar](https://avatars2.githubusercontent.com/u/15256898?v=4&s=100)
```
**Result:**
![adam's avatar](https://avatars2.githubusercontent.com/u/15256898?v=4&s=100)

---

# 6. LINKS
**Syntax:** _\[CONTENT\]\(URL\)_

**Example:**
```markdown
[GitHub](http://github.com)
```
**Result:**
[GitHub](http://github.com)

---

# 7. FENCED CODE BLOCKS
**Syntax:** 

_\`\`\`language-name_

_YOUR CODES_

_\`\`\`_

**Example:**

_```javascript_

_var name = function() {_

_console.log("Hello World!");_

_}_

_```_

**Result:**
```javascript
var name = function() {
	console.log("Hello World!");
}
```


---
*References*
1. [Adam Pritchard's Markdown Cheatsheet](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet#images)
2. [Markdown Cheatsheet PDF](https://guides.github.com/pdfs/markdown-cheatsheet-online.pdf)


