# React-editor

A react editor that let you use your custom react component and render it for you.
Save data in a JSON structure. It's up to you to store and render your data as you want.

Inspired by the easy integration of [react-page](https://react-page.github.io/) for the developper, and the ease of use of [editorjs](https://editorjs.io/) for the end user.

## Nota Bene

⚠ This is a proof of concept. But i will try to implement it as much as i can, and maybe, build a framework agnostic solution.

## Features

- Use React Components as plugin for rich editing
- Inline tool options with a toolbar

By default, it expose an editable component `Editor` where you need to provide an array of tools.
A tool is a React Component, wrapped inside s simple object.

```js
const tool = {
  name: "Bold",
  icon: FaBold,
  component: Bold,
  options: {
    inline: true,
    multiline: false,
    toolbar: true,
  },
};
```

As of today, here is the structure:

- `name`: The name displayed on the UI
- `icon:` The icon diplayed in the list of tools and in the toolbar
- `component`: The React component to use, or a standard function
- `options` (all boolean):
  - `inline`: whether or not it's an inline function
  - `multiline`: if true, break lines and send as an array to the component
  - `toolbar`: display or not in the toolbar

If `inline` is set to true, the editor will treat the component as a normal JS function. You can do what you want with the content, but you must return the same type. This function can be async.

The `icon` can be in a various format:

- a react component. ex: react-icons
- a string: an url or a class-name

## Exemple

```js
import { Editor } from "@polynux/react-editor";
import { useState } from "react";
import { FaBold, FaUnderline, FaListUl } from "react-icons/fa";

function Bold({ text }) {
  return <strong>{text}</strong>;
}

function Underline({ text }) {
  return <u>{text}</u>;
}

function List({ text }) {
  return (
    <ul>
      {text.map((el) => (
        <li>{el}</li>
      ))}
    </ul>
  );
}

const tools = [
  {
    name: "Bold",
    icon: FaBold, // can be component or img
    component: Bold,
    options: { inline: true },
  },
  {
    name: "Underline",
    icon: FaUnderline,
    component: Underline,
    options: { inline: true },
  },
  {
    name: "List",
    icon: FaListUl,
    component: List,
    options: { multiline: true }, // send input as array of string
  },
];

function Component() {
  const [data, setData] = useState();
  return (
    <div>
      <div>
        <p>Here is the editor:</p>
        <Editor tools={tools} data={data} onChange={setData} />
      </div>
      <div>
        <p>And here is the render:</p>
        <Editor tools={tools} data={data} render />
      </div>
    </div>
  );
}
```
