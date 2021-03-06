[![Build Status](https://travis-ci.org/webdeveloperpr/draft-js-color-picker.svg?branch=master)](https://travis-ci.org/webdeveloperpr/draft-js-color-picker)
# draft-js-color-picker

This package allows you to use dynamic colors on your draft-js editor
 
## Table of Contents

- [Installation](#installation)
- [Usage](#usage)
- [API](#api)
- [Example](#example)
- [Support](#support)
- [Contributing](#contributing)

## Installation

```sh
npm i --save draft-js-color-picker
```

## API
inside the constructor initialize the picker functions
```javascript
    this.updateEditorState = editorState => this.setState({ editorState });
    this.getEditorState = () => this.state.editorState;

    // Step 2: run the colorPickerPlugin function
    this.picker = colorPickerPlugin(this.updateEditorState, this.getEditorState);

```
**this.picker**
 - .addColor() - Adds color to the text selection
 - .currentColor() - Gets the current color for the selection
 - .removeColor() - Removes the color from the selection.
 - .customStyleFn() - Used by the editor to apply the colors
 - .exporter() - Export the editorsState inlineStyles
 
**component**
 1. ColorPicker props:
   - presetColors  { array } - swatch preset colors
   - toggleColors { function } - function that adds the color
   - color { string } - the current color
 
## Usage

```javascript
import React, { Component } from 'react';
import { Editor } from 'draft-js';
import ColorPicker, { colorPickerPlugin } from 'draft-js-color-picker';
// optionals
import { stateToHTML } from 'draft-js-export-html';
import Raw from 'draft-js-raw-content-state';

// Add preset colors to the picker
const presetColors = [
  '#ff00aa',
  '#F5A623',
  '#F8E71C',
  '#8B572A',
  '#7ED321',
  '#417505',
  '#BD10E0',
  '#9013FE',
  '#4A90E2',
  '#50E3C2',
  '#B8E986',
  '#000000',
  '#4A4A4A',
  '#9B9B9B',
  '#FFFFFF',
];

// Initialize the contentState
class RichEditor extends Component {
  constructor(props) {
    super(props);
    this.state = { editorState: new Raw().addBlock('Hello World', 'header-two').toEditorState() };

    // Step 1: Create the functions to get and update the editorState
    this.updateEditorState = editorState => this.setState({ editorState });
    this.getEditorState = () => this.state.editorState;

    // Step 2: run the colorPickerPlugin function
    this.picker = colorPickerPlugin(this.updateEditorState, this.getEditorState);
  }

  render() {
    const { editorState } = this.state;

    // Optional: you can use an export the color styles if you plan to render html
    const inlineStyles = this.picker.exporter(editorState);
    const html = stateToHTML(this.state.editorState.getCurrentContent(), { inlineStyles });

    return (
      <div style={{ display: 'flex', padding: '15px' }}>
        <div style={{ flex: '1 0 25%' }}>

          {/* Step 4: pass the functions to the color picker */}
          <ColorPicker
            toggleColor={color => this.picker.addColor(color)}
            presetColors={presetColors}
            color={this.picker.currentColor(editorState)}
          />
          <button onClick={this.picker.removeColor}>clear</button>
        </div>

        <div style={{ flex: '1 0 25%' }}>
          <h2>Draft-JS Editor</h2>

          {/* Step 5 pass the customStyleFn */}
          <Editor
            customStyleFn={this.picker.customStyleFn}
            editorState={editorState}
            onChange={this.updateEditorState}
            placeholder="Tell a story..."
            readOnly={this.state.readOnly}
          />
        </div>

        <div style={{ flex: '1 0 25%' }}>
          <h2>Exported HTML</h2>
          <div dangerouslySetInnerHTML={{ __html: html }}/>
        </div>
      </div>
    );
  }
}
export default RichEditor;
```
## Support

Please [open an issue](https://github.com/webdeveloperpr/draft-js-color-picker/issues) for support.

## Contributing

Please contribute using [Github Flow](https://guides.github.com/introduction/flow/). Create a branch, add commits, and [open a pull request](https://github.com/webdeveloperpr/draft-js-custom-styles/pulls).

