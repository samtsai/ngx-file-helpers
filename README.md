# ngx-file-helpers
Angular File Helpers

## Installation
Add the package to your application.

```
npm install --save ngx-file-helpers
```

## Getting started

Import the file helpers module to your application module.

```
import { BrowserModule } from '@angular/platform-browser';
import { NgModule } from '@angular/core';
import { FileHelpersModule } from 'ngx-file-helpers';

import { AppComponent } from './app.component';

@NgModule({
  declarations: [AppComponent],
  imports: [
    BrowserModule,
    FileHelpersModule
  ],
  bootstrap: [AppComponent]
})
export class AppModule { }
```

## File Picker

Add the file picker directive to an element, like a button.

```
<button type="button" ngxFilePicker>Browse</button>
```

Select how the file should be read; by default the mode is dataUrl. Available read modes are exposed through the ReadMode enum.

```
<button type="button" [ngxFilePicker]="readMode">Browse</button>
```

```
enum ReadMode {
  arrayBuffer,
  binaryString,
  dataURL,
  text
}
```

Bind to the `filePick` event to get the picked file from the `$event` variable.

```
<button
  type="button"
  ngxFilePicker
  (filePick)="file = $event">
  Browse
</button>
```

Use the optional `accept` attribute to indicate the types of files that the control can select.

```
<button
  type="button"
  ngxFilePicker
  accept="image/*"
  (filePick)="file = $event">
  Browse
</button>
```

Use the optional `multiple` attribute to indicate whether the user can pick more than one file.

```
<button
  type="button"
  ngxFilePicker
  accept="image/*"
  multiple
  (filePick)="file = $event">
  Browse
</button>
```

The picked file implements the following interface:

```
interface ReadFile {
  lastModifiedDate: Date;
  name: string;
  size: number;
  type: string;
  dataURL: string;
}
```

The directive also has a `reset()` method that unset the selected file. This is useful if you want to force the `filePick` event to trigger again even if the user has picked the same file.

```
export class MyComponent {
  ...
  @ViewChild(FilePickerDirective)
  private filePicker;
  ...

  onReadEnd(fileCount: number) {
    this.status = `Read ${fileCount} file(s) on ${new Date().toLocaleTimeString()}.`;
    this.filePicker.reset();
  }
}
```

There are two more events that can be listened to:
- `readStart`: triggered when the directive start to read files;
- `readEnd`: triggered when the directive has read all the files.

These two events emit the number of file (`$event` variable) to be or that has been read.
