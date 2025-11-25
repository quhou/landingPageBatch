# CHANGELOG

## 2025/11/25

### Added

* Added `base.html` template to provide a unified layout structure and reduce redundancy.
* Added feature-specific templates:

  * `ai_cartoonizer.html`
  * `remove_objects_from_photos.html`
* Added URL routes for new feature pages.
* Added view functions for serving feature pages and loading JSON content:

  * `views.ai_cartoonizer_view`
  * `views.remove_objects_view`
* Added shared JSON loader utility to standardize reading/parsing feature content.
* Added feature JSON data files:

  * `json/ai_cartoonizer.json`
  * `json/remove_objects_from_photos.json`

### Changed

* Refactored template structure to use `base.html` with Django template inheritance.
* Split templates into smaller partials including hero, examples, testimonial, etc.
* Updated URL configuration to group feature routes under `urls.features` and apply namespace for clarity.
* Updated views to dynamically load JSON and provide structured context values such as `hero`, `features`, `testimonial`, `faq`.

### Fixed

* Fixed invalid HTML in the testimonial section and ensured all Django control tags (`for`, `if`, `block`) are properly closed.
* Fixed template tag loading order to ensure `{% load extras %}` appears before `{% extends %}`, preventing `Invalid block tag` errors.

### Notes / Implementation Details

* Feature JSON files follow a shared schema:

  * `language`, `title`, `nav`, `hero`, `trustIndicators`, `examples`, `features`, `howItWorks`, `testimonial`, `faq`, `footer`
* Example JSON loading pattern:

  ```python
  data = load_json("json/ai_cartoonizer.json")
  return render(request, "features/ai_cartoonizer.html", data)
  ```
* Example URL patterns:

  ```python
  path("ai-cartoonizer/", views.ai_cartoonizer_view, name="ai_cartoonizer")
  path("remove-objects/", views.remove_objects_view, name="remove_objects")
  ```

### Migration / Developer Actions

* Move hard-coded text from templates into JSON files following the new schema.
* Update any internal references pointing to old templates to use new base-inherited structure.
* Ensure templates using custom tags load them with `{% load extras %}` before extending `base.html`.
* After deployment, clear cached templates and restart the application server.

### Impact

* Changes affect frontend template structure and view handling; backend APIs are unchanged.
* Localization and content updates now happen entirely through JSON files.
* Developers should pull new JSON assets; existing branches modifying templates may require minor merge fixes.
