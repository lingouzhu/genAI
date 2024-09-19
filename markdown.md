# Setup Markdownn in VSCode

## Install following vscode extensions

- markdownlint
- Markdown Preview Mermaid Support
- Markdown Checkboxes

# Edit Markdown

## Outline View

The Outline view is a separate section in the bottom of the File Explorer. When expanded, it shows the symbol tree of the currently active editor. For Markdown files, the symbol tree is the Markdown file's header hierarchy.

## Preview Markdown

VS Code supports Markdown files out of the box. You just start writing Markdown text, save the file with the .md extension and then you can toggle the visualization of the editor between the code and the preview of the Markdown file; To switch between views, press Ctrl+Shift+V in the editor. You can view the preview side-by-side (Ctrl+K V) with the file you are editing and see changes reflected in real-time as you edit.

# Markdown Syntax

## Basic Syntax

| Element | Markdown Syntax |
| ----------- | ----------- |
| Heading | <code># H1 <br>## H2 <br>### H3</code> |
| Bold | `**bold text**` |
| Italic | `*italicized text*` |
| Blockquote | `> blockquote` |
| Ordered List | <code>1. First item <br> 2. Second item <br> 3. Third item </code> |
| Unordered List | <code>- First item <br> - Second item <br> - Third item` </code> |
| Horizontal Rule | `---`  |
| Link | `[title](https://www.example.com)`  |
| Image | `![alt text](image.jpg)`  |


## Extended Syntax

<table class="table table-bordered">
  <thead class="thead-light">
    <tr>
      <th>Element</th>
      <th>Markdown Syntax</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>Table</td>
      <td><code>
          | Syntax      | Description |<br>
          | ----------- | ----------- |<br>
          | Header      | Title       |<br>
          | Paragraph   | Text        |
      </code></td>
    </tr>
   <tr>
      <td>Fenced Code Block</td>
      <td><code>```<br>
      {<br>
      &nbsp;&nbsp;"firstName": "John",<br>
      &nbsp;&nbsp;"lastName": "Smith",<br>
      &nbsp;&nbsp;"age": 25<br>
      }<br>
      ```
      </code></td>
    </tr>  
   <tr>
      <td>Footnote</td>
      <td><code>
        Here's a sentence with a footnote. [^1]<br><br>
        [^1]: This is the footnote.
      </code></td>
    </tr>
    <tr>
      <td>Heading ID</td>
      <td><code>### My Great Heading {#custom-id}</code></td>
    </tr>
    <tr>
      <td>Definition List</td>
      <td><code>
        term<br>
        : definition
      </code></td>
    </tr>
    <tr>
      <td>Strikethrough</td>
      <td><code>~~The world is flat.~~</code></td>
    </tr>
    <tr>
      <td>Task List</td>
      <td><code>
        - [x] Write the press release<br>
        - [ ] Update the website<br>
        - [ ] Contact the media
      </code></td>
    </tr>
    <tr>
      <td>Emoji<br></td>
      <td><code>
        That is so funny! :joy:
      </code></td>
    </tr>
    <tr>
      <td>Highlight</td>
      <td><code>
        I need to highlight these ==very important words==.
      </code></td>
    </tr>
    <tr>
      <td>Subscript</td>
      <td><code>
        H~2~O
      </code></td>
    </tr>
    <tr>
      <td>Superscript</td>
      <td><code>
        X^2^
      </code></td>
    </tr>          
  </tbody>
</table>    

