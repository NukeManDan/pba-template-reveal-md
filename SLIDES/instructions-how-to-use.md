# Instructions for Template
 
---

## What are we going to see:

- How to use this Template
  - Speaker Notes & View
- Code Highlight & Transitions

---

## How to use this Template

Press the `down/up` keys to navigate _vertical_ slides

Press `Esc` or `o` to see an `overview` view that your arrow keys can navigate in, click one to open at this slide.

Try doing down a slide.
 
----

## Speaker Notes

> Press the `s` key to bring up a popup window with speaker view

Note:
This is a note just for you.
Set under a line in your slide starting with "`Note`:" 
all subsequent lines are just seen in speaker view.

---

## Code Highlight & Transitions

> The first rule of functions is that they should be small. The second rule, is that they should be smaller than that. - Robert C. Martin
 
----

## Bad example

Example: HtmlUtils.java from framework 'FitNesse'

<pre><code style="font-size: 0.5em !important" data-trim data-noescape data-line-numbers="0|1|4|5|10-12|19-21" class="java">
public static String testableHtml(PageData pageData, boolean includeSuiteSetup) throws Exception {
  WikiPage wikiPage = pageData.getWikiPage();
  StringBuffer buffer = new StringBuffer();
  if (pageData.hasAttribute("Test")) {
    if (includeSuiteSetup) {
      WikiPage suiteSetup = PageCrawlerImpl.getInheritedPage(SuiteResponder.SUITE_SETUP_NAME, wikiPage);
      if (suiteSetup != null) {
        WikiPagePath pagePath = suiteSetup.getPageCrawler().getFullPath(suiteSetup);
        String pagePathName = PathParser.render(pagePath);
        buffer.append("!include -setup .")
          .append(pagePathName)
          .append("\n");
      }
    }
    WikiPage setup = PageCrawlerImpl.getInheritedPage("SetUp", wikiPage);
    if (setup != null) {
      WikiPagePath setupPath = wikiPage.getPageCrawler().getFullPath(setup);
      String setupPathName = PathParser.render(setupPath);
      buffer.append("!include -setup .")
        .append(setupPathName)
        .append("\n");
    }
  }
  buffer.append(pageData.getContent());
  if (pageData.hasAttribute("Test")) {
    WikiPage teardown = PageCrawlerImpl.getInheritedPage("TearDown", wikiPage);
    if (teardown != null) {
      WikiPagePath tearDownPath =
        wikiPage.getPageCrawler().getFullPath(teardown);
      String tearDownPathName = PathParser.render(tearDownPath);
      buffer.append("\n")
        .append("!include -teardown .")
        .append(tearDownPathName)
        .append("\n");
    }
    if (includeSuiteSetup) {
      WikiPage suiteTeardown = PageCrawlerImpl.getInheritedPage(SuiteResponder.SUITE_TEARDOWN_NAME, wikiPage);
      if (suiteTeardown != null) {
        WikiPagePath pagePath = suiteTeardown.getPageCrawler().getFullPath(suiteTeardown);
        String pagePathName = PathParser.render(pagePath);
        buffer.append("!include -teardown .")
          .append(pagePathName)
          .append("\n");
      }
    }
  }
  pageData.setContent(buffer.toString());
  return pageData.getHtml();
}
</pre></code>
<!-- .element: class="fragment" data-fragment-index="2" -->

Note:
Eurk. On va vite fais essayer de comprendre.
WTF!
 
----

## Smaller is better

<pre><code style="font-size: 0.5em !important" data-trim data-noescape data-line-numbers class="java">
public static String renderPageWithSetupsAndTeardowns(PageData pageData, boolean isSuite) throws Exception {
  boolean isTestPage = pageData.hasAttribute("Test");
  if (isTestPage) {
    WikiPage testPage = pageData.getWikiPage();
    StringBuffer newPageContent = new StringBuffer();
    includeSetupPages(testPage, newPageContent, isSuite);
    newPageContent.append(pageData.getContent());
    includeTeardownPages(testPage, newPageContent, isSuite);
    pageData.setContent(newPageContent.toString());
  }
  return pageData.getHtml();
}
</pre></code>


- Functions should be short, 20 lines is already a lot !
<!-- .element: class="fragment" data-fragment-index="8" -->
- Functions should have 1 (or 2) indentation level.
<!-- .element: class="fragment" data-fragment-index="9" -->
- One function = One abstraction level.
<!-- .element: class="fragment" data-fragment-index="10" -->

Note: 
Laisser quelques secondes pour comprendre.
On lit de haut en bas, comme une histoire

Abstraction Level: The amount of complexity by which a system is viewed or programmed. The higher the level, the less detail. The lower the level, the more detail.
Si je ton neveu demande comment fonctionne une voiture et que tu m'explique comment sont fabriqués les freins, tu es au mauvais niveau d'abstraction.
 
----

## Even smaller

> FUNCTIONS SHOULD ONLY DO ONE THING. THEY SHOULD DO IT WELL. THEY SHOULD DO IT ONLY. - Robert C. Martin
<!-- .element: class="fragment" data-fragment-index="2" -->

<pre><code style="font-size: 0.5em !important" data-trim data-noescape data-line-numbers class="java">
public static String renderPageWithSetupsAndTeardowns(PageData pageData, boolean isSuite) throws Exception {
  if (isTestPage(pageData))
    includeSetupAndTeardownPages(pageData, isSuite);
  return pageData.getHtml();
}
</pre></code>

- "Pour... Si... Alors..."
<!-- .element: class="fragment" data-fragment-index="3" -->
 
----

## Some other rules...

```typescript
function async getUser(id: string): Promise<User> {
  const user = await db.find({ user: id });
  await db.close();
  return user;
}
```

- Be careful with "side effects". A function should only do what the name suggests.
<!-- .element: class="fragment" data-fragment-index="2" -->

 
----

## Some other rules...

```javascript
let numbers = [1,2,3,4,5];
const firstThreeNumbers = numbers.splice(0,3);
const lastThreeNumbers = numbers.splice(2,5);
```

```javascript
let numbers = [1,2,3,4,5]; // 1,2,3,4,5

const firstThreeNumbers = numbers.splice(0,3); 
// firstThreeNumbers = 1,2,3
// AND numbers = 4,5

const lastThreeNumbers = numbers.splice(2,5);
// lastThreeNumbers = undefined
```
<!-- .element: class="fragment" data-fragment-index="2" -->

- Limit "in/out" parameters. Or don't use them.
<!-- .element: class="fragment" data-fragment-index="3" -->

 
----

## Quelques autres regles

```typescript
function initDb(config: Config, useHttps: boolean) {
  ...
}
```

- Limit booleans parameters
<!-- .element: class="fragment" data-fragment-index="2" -->
- Limit the numbers of parameters
<!-- .element: class="fragment" data-fragment-index="3" -->
 
---

# More help needed? 

Please reach out to the academy content & docs team on element for support!