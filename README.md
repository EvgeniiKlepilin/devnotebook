# devnotebook
A place where I record things that I find useful, helpful, or insightful for a software developer. Most of the notes are excerpts from already existing material on the web. This is a summarized collection that I found useful or interesting to take a note of.

## Miscellaneous
### Framebusting
Framebusting is a way to *prevent clickjacking*, which occurs when a *malicious web site pulls a page originating from another domain into a frame and overlays it with a counterfeit page*, allowing only portions of the original, or clickjacked, page (for example, a button) to display. When users click the button, they in fact are clicking a button on the clickjacked page, causing unexpected results.

For example, say your application is a web-based application that resides in `DomainA`, and a web site in `DomainB` clickjacks your page by creating a page with an IFrame that points to a page in your web application at `DomainA`. When the two pages are combined, the page from `DomainB` covers most of your page in the IFrame, and exposes only a button on your page that deletes all records in your web application. Users, not realizing they are actually in the web application, may click the button and inadvertently delete all records.

#### References:
- https://docs.oracle.com/en/applications/jd-edwards/administration/9.2.x/eotsc/framebusting.html#u30144326
