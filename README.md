# TakeOnRules Hugo Theme

This [Hugo](https://gohugo.io) began as a fork of the [Hugo Tufte](https://github.com/shawnohare/hugo-tufte) theme.

Since that time, I've made extensive changes, and consider this to be it's own theme.

You can see this template at [Take on Rules](https://takeonrules.com).  _Note: To someone adopting this theme whole-cloth, there's a few pieces in my private repo that I also leverage.  If you are interested, [contact me](https://takeonrules.com/contact-me) and I can help._

For colors, I've adopted the palette from [Protesilaos Stavrou](https://protesilaos.com/)'s [Modus themes](https://protesilaos.com/modus-themes/); The colors are WCAG compliant and bring me a bit of joy.

## Semantic Markup and Linked Data

I strive for semantic markup; My goal is that I create an accessible site.  Also, as a benefit, if you disable the stylesheet the page will continue to render in a meaningful way.  _In fact, I encourage you to disable the stylesheet on most sites and see how they look._

I also include inline Linked Data, using the RDFa format.  I'm looking to provide machine-readable context which others can query; This also means that when I say "I'm writing about Stars without Number", I link to a disambiguator (e.g. Wikidata) to confirm "This is the Stars without Number" I'm writing about.

I had previously tried JSON+LD which may be more machine readable, but I found that this meant the Linked Data was separate from the associated HTML nodes that it represented.  I didn't like that.

## Partials

There are several partials that help build; Some rely on data files in my private repository.  Others help manage paragraph structures for content passed as markdown.

### Paragraph Management

The three primary paragraph management partials are:

- [unparagraphy.html](layouts/partials/unparagraphy.html) :: ensures that the partial does not return wrap the content in a P-tag.  In otherwords, useful for ensuring inline content (see display [CSS Flow Layout](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flow_Layout)).
- [paragraphy.html](layouts/partials/paragraphy.html) :: sibling to `unparagraphy.html`, this partial ensures that the returned content'a top-level DOM nodes are P-tags.  Useful for ensuring block content.
- [smallParagraphy.html](layouts/partials/smallParagraphy.html) :: sibling to `paragraphy.html`, this partial ensures that the top-level DOM nodes are block display elements, and the children of those elements are wrapped in [small](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/small) tags.  This is useful for marginalia (e.g. side comments).

### Dates and Tags

- [dates-and-tags.html](layouts/partials/dates-and-tags.html) :: Each post has a published date and a last modified date.  I also tag each post I write with a variety of tags.  This partial helps create a uniform date and tag inline HTML.  This partial also helps add RDFa metadata to the tags.

## Shortcodes

Similar to partials, I leverage several shortcodes to help manage my content.

### Marginalia

- [sidenote.html](layouts/shortcodes/sidenote.html) :: this shortcode creates an inline numbered sidenote in the margins; It uses the `unparagraphy.html`.
- [marginnote.html](layouts/shortcodes/marginnote.html) :: this shortcode creates a block margin note.  It uses the `smallParagraphy.html`.
- [marginfigure.html](layouts/shortcodes/marginfigure.html) :: a shortcode to ensure well formed images in the marginalia.

Semantically, I don't treat these as an [ASIDE](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/aside); Though I do add the dom attribute `role="note"`.  I have found that reader plugins don't render an aside, and I consider the marginalia important to the document.

Two other shortcodes (table.html and update.html) provide marginalia options.  See below for more on these two shortcodes.

### Glossary

Throughout TakeOnRules.com, I make extensive use of the ABBR tag. I use this markup to improve the accessibility of my site.

- [abbr.html](layouts/shortcodes/abbr.html) :: Creates the [ABBR](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/abbr) tag and links to the glossary.
- [dfn.html](layouts/shortcodes/dfn.html) :: Creates a [DFN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/dfn) along with an ABBR tag and link to glossary.
- [linkToGame.html](layouts/shortcodes/linkToGame.html) :: Creates a [CITE](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/cite) tag with a link to an offer to purchase (as defined in the [data/glossary.yml](data/glossary.yml) file).
- [citePage.html](layouts/shortcodes/citePage.html) :: A shortcode to conditionally generate a `cite a` or `a` tag to the page at the given filename (relative to the Hugo content directory).  **Note:** I use [`tor-page-relative-pathname-list`](https://github.com/jeremyf/dotzshrc/blob/dd289492248b0b2719297220e4dc1127ee7c89df/emacs/jnf-blogging.el#L100-L106) to generate a list of all the filenames.

_**Note:** The `abbr.html` shortcode preceeded the `dfn.html` shortcode.  They include somewhat duplicated logic, which could be further normalized._

Related is the [data/glossary.yml](data/glossary.yml) file which contains the data which I use for the `abbr.html` and `dfn.html` shortcodes.  This is a live data set that I use for TakeOnRules.com.

I wrote about the Glossary in <cite><a href="http://takeonrules.com/2020/12/20/many-small-tools-make-light-work-in-emacs/" class="u-url p-name" rel="cite">Many Small Tools Make Light Work (in Emacs)</a></cite>.

These short codes are written such that I won't render the same help link twice; Hence the use of testing a Page's Scratch variable.

#### Todo

The `linkToGame.html` shortcode does similar work as the `dfn.html` shortcode; Which does similar work to `abbr.html`.  I believe I can consolidate these.  Here are the scenarios:

- When I first reference a game, then it should cite the work, link to the offer, and introduce an abbreviation. For example: `<cite><a>Stars without Number: Revised Edition</a></cite> (<abbr title="Stars without Number: Revised Edition">SWN</abbr>). In <abbr title="Stars without Number: Revised Edition">SWN</abbr>...`
- When I first reference an abbreviation, then it should render the RDFa information.
- On the first rendering of an abbreviation OR game; In other words, both of these should use the same cache key to determine if I've already rendered the assistive links to content.

In further reading of DFN, I need to remove that shortcode from my blog posts and instead use it for the glossary page.

### Other Stuff

- [table.html](layouts/shortcodes/table.html) :: Helps ensure a well-formed table; one parameter allows for table rendering in the margins.
- [ogc.html](layouts/shortcodes/ogc.html) :: I sometimes write Open Game Content (OGC), released under the Open Game License (OGL).  This shortcode helps wrap that content.
- [blockquote.html](layouts/shortcodes/blockquote.html) :: I quote sources, and this shortcode helps create well-formed quotes; I also repurpose it for epigraphs.g One of my favorite books is <cite>Dune</cite> by Frank Herbert.  <cite>Dune</cite> introduces each chapter with an epigraph.  I like that style, and on occassion use that.
- [update.html](layouts/shortcodes/update.html) :: a shortcode that helps create consistent document updates, via the [INS-tag](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/ins).  One parameter allows for updates rendered in the margins.
- [maincolumn.html](layouts/shortcodes/maincolumn.html) :: a shortcode to help render an image in the maincolumn.
