## Design Principles

- **The drop zone is the primary interaction surface, not the file input.** A large dashed-border area with clear instructional text invites drag-and-drop; the hidden `<input type="file">` is triggered programmatically.
- **Invalid files are rejected immediately with an inline error.** Type and size validation fires before files enter the list -- never silently discard files the user selected.
- **Upload progress is visible per-file.** A slim progress bar on each file item transforms the upload from an uncertain wait into a trackable process.
- **Files can be removed before and during upload.** Each file item has a dedicated remove button with an aria-label that includes the file name.

### CSS
```css
.upload {
  display: flex;
  flex-direction: column;
  gap: var(--space-3);
}
.upload-zone {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: var(--space-3);
  padding: var(--space-7) var(--space-5);
  border: 2px dashed var(--border);
  border-radius: var(--radius-md);
  background: var(--bg);
  cursor: pointer;
  transition: border-color var(--dur-base) var(--ease), background var(--dur-base) var(--ease);
  text-align: center;
  min-height: 160px;
}
.upload-zone:hover {
  border-color: var(--accent);
  background: var(--accent-tint);
}
.upload-zone--dragover {
  border-color: var(--accent);
  background: var(--accent-tint);
  border-style: solid;
}
.upload-zone__icon {
  color: var(--fg-muted);
}
.upload-zone__text {
  font-family: var(--font-body);
  font-size: var(--fs-body-sm);
  color: var(--fg-muted);
}
.upload-zone__text strong {
  color: var(--accent);
  font-weight: var(--fw-semibold);
}
.upload-zone__hint {
  font-family: var(--font-body);
  font-size: var(--fs-label);
  color: var(--fg-subtle);
}
.upload-file-list {
  display: flex;
  flex-direction: column;
  gap: var(--space-2);
}
.upload-file {
  display: flex;
  align-items: center;
  gap: var(--space-3);
  padding: var(--space-2) var(--space-3);
  background: var(--bg);
  border: 1px solid var(--border);
  border-radius: var(--radius-sm);
}
.upload-file__name {
  flex: 1;
  font-family: var(--font-body);
  font-size: var(--fs-body-sm);
  color: var(--fg);
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;
}
.upload-file__size {
  font-family: var(--font-body);
  font-size: var(--fs-label);
  color: var(--fg-muted);
  flex-shrink: 0;
}
.upload-file__progress {
  flex-shrink: 0;
  width: 64px;
  height: 4px;
  background: var(--border);
  border-radius: var(--radius-full);
  overflow: hidden;
}
.upload-file__progress-bar {
  height: 100%;
  background: var(--accent);
  border-radius: var(--radius-full);
  transition: width var(--dur-base) var(--ease);
}
.upload-file__remove {
  flex-shrink: 0;
  background: transparent;
  border: none;
  color: var(--fg-muted);
  cursor: pointer;
  padding: var(--space-1);
  border-radius: var(--radius-sm);
  transition: color var(--dur-fast) var(--ease), background var(--dur-fast) var(--ease);
}
.upload-file__remove:hover {
  color: var(--error);
  background: rgba(220,38,38,0.08);
}
```

### HTML Pattern
```html
<div class="upload" data-upload data-max-size="10485760" data-accept=".pdf,.png,.jpg">
  <input type="file"
         id="file-input"
         class="upload__input"
         multiple
         accept=".pdf,.png,.jpg"
         aria-label="Upload files"
         style="position:absolute;width:1px;height:1px;overflow:hidden;clip:rect(0,0,0,0);white-space:nowrap;" />
  <div class="upload-zone"
       role="button"
       tabindex="0"
       aria-label="Drop files here or click to browse"
       data-upload-zone>
    <svg class="upload-zone__icon" width="40" height="40" viewBox="0 0 40 40" aria-hidden="true">
      <path d="M20 6v20M12 14l8-8 8 8" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
      <path d="M6 26v6a2 2 0 002 2h24a2 2 0 002-2v-6" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round"/>
    </svg>
    <p class="upload-zone__text"><strong>Click to upload</strong> or drag and drop</p>
    <p class="upload-zone__hint">PDF, PNG, or JPG up to 10MB</p>
  </div>
  <div class="upload-file-list" data-upload-file-list aria-live="polite">
    <!-- File items injected dynamically -->
    <div class="upload-file">
      <svg width="16" height="16" viewBox="0 0 16 16" aria-hidden="true">
        <path d="M4 2h5l4 4v8a1 1 0 01-1 1H4a1 1 0 01-1-1V3a1 1 0 011-1z" fill="none" stroke="var(--fg-muted)" stroke-width="1.5"/>
      </svg>
      <span class="upload-file__name">report-q3.pdf</span>
      <span class="upload-file__size">2.4 MB</span>
      <div class="upload-file__progress">
        <div class="upload-file__progress-bar" style="width: 75%"></div>
      </div>
      <button class="upload-file__remove" type="button" aria-label="Remove report-q3.pdf">
        <svg width="16" height="16" viewBox="0 0 16 16"><path d="M4 4l8 8M12 4l-8 8" stroke="currentColor" stroke-width="1.5" stroke-linecap="round"/></svg>
      </button>
    </div>
  </div>
</div>
```

### JS Behavior
- **Triggers**: Click on the drop zone opens the file browser (click the hidden `<input type="file">`). Drag-and-drop via `dragenter`, `dragover`, `dragleave`, `drop` events on the zone.
- **Drag-over state**: Add `.upload-zone--dragover` on `dragenter`/`dragover`. Remove on `dragleave`/`drop`.
- **File validation**: Check file type against `accept` attribute and file size against `data-max-size`. Show an inline error for invalid files. Do not add invalid files to the list.
- **Progress simulation**: For client-side uploads, show an animated progress bar. For real uploads, bind the progress bar width to the `XMLHttpRequest.upload.progress` or `fetch` with `ReadableStream`.
- **Remove**: Click the remove button to remove the file from the list and any associated `File` reference.
- **Keyboard**: `Enter` / `Space` on the drop zone (which has `tabindex="0"`) triggers the file input.

### Accessibility
- The hidden `<input type="file">` remains in the DOM and accessible to assistive technology
- The drop zone has `role="button"`, `tabindex="0"`, and a descriptive `aria-label`
- The file list area uses `aria-live="polite"` to announce added/removed files
- Each remove button has `aria-label` including the file name (e.g., "Remove report-q3.pdf")
- File type and size restrictions are communicated visually and in the input's `accept` attribute
- Progress bars use `role="progressbar"` with `aria-valuenow`, `aria-valuemin`, `aria-valuemax` when the upload is active
