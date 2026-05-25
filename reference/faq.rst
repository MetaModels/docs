FAQ
===

.. _faq-searchable-pages:

Indexing (Searchable Pages)
----------------------------

| Q: Can I configure the sitemap and the search index independently of each other?
| A: No, both are fed by the same function. This is implemented in the Contao Core, so the same configuration applies to both.

| Q: Can I use Geo-Protection?
| A: In general, filters that use browser data, IP addresses, or other user data for filtering should not be used. Instead, a filter that is universally applicable should be used.

| Q: When is the function used?
| A: Whenever a page is saved, the sitemap is regenerated — this is exactly where the new function hooks in. The same applies when the search index is rebuilt.

.. _faq-allgemein:

General
-------

| Q: I have two MetaModels filters — one on the home page and one on the search results page. I cannot get the filter on the results page to be controlled by the one on the home page. Both filters are otherwise identical.
| A: If I create the filter as a module and use it in both places, it works. The POST request includes the form ID, which means only this specific filter can process the POST data. Once everything goes via GET data, it doesn't matter.
