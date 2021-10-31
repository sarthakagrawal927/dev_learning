# intermediate_web

## Performance 1 - Network Requests

* HTML, CSS, JS - Compressed via webpack (auto usually)
* Images - SVG op, JPG - most colors, PNG -> Transparency, GIF- Animations (always compress), MEDIA-QUERY must
* Limit number of files by combining (auto usually - webpack)
* Script after CSS & make sure only necessary (visible page) style sheets are imported
* Conditional loading based on Media query, less specificity means less work for browser to make sense of the tag
* Inline styles much faster, scripts are blockage.
  * Async - loads JS in parallel w another thread - followed by immediate execution - Core functionality needs JS
  * Defer - loads JS in parallel w another thread - executed after page loads -  Not important to core functionality
  * NILL - loads JS (blocking main) - executes it - Core scripts

## Performance 2 - React Mode

* Code splitting via Lazy loading.
*Performance optimizing via memoization, rerendering only necessary files.
