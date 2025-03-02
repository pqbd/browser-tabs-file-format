# Browser tabs file format

File format specification for files designed to carry information about specific URLs selection.

Files of this type are designed to store and recovery focused browsing sessions:
- Quick browsing session backup
- Some bookmarks to share
- Project starter (messanger chat, code repository, project management system, QA/DEV/PROD environment links)
- Used resources references to attach to research output
- Theme focused grouping of URLs
- etc.

## Why

I strongly believe that this has to be a part of browser from the very beginning of its history. "File > Save" should save current "session" into a file like all other programs do for their documents. Instead it is trying to save current webpage, that should be more like "File > Save Page" or you always have a "Save Page in PDF" option avaialble through printing options.

My main painpoint is that machine restarts, browser crashes and recovering options are not delivered or time limited. Also I was looking for a tool to share useful URL collections like to attach resources to a research output documentation, etc.

File format should:
- be automation friendly still human readable
- be extensible
- support multiple use cases and implications

Minimum information needed to save a tab is its URL. Basically tabs are no more than a list of URLs listed in an appearence order.
However, there is also some useful structures how they may be organized: groups, windows.

Basically, groups are like folders where top level folder is a window. Adding support of unlimited nested groups makes format compatibable with storing bookmarks.

Some additional metadata information is added to ease automation activities.

## Basics

This is a minimal base type for all extentions like:
- Comments support
- Browser (or other program) specific metadata information (favicons, importance, etc.)
- [Not recommended] session metadata serialization 
- [Not recommended] Page preview
- etc.

### Minimal one-URL document

JSON:
```json
{
  "kind": "BrowserTabs",
  "version": "1.0",
  "content": [
    {
      "kind": "TabGroup",
      "tabs": [
        {
          "kind": "Tab",
          "url": "https://example.com"
        }
      ]
    }
  ]
}
```

YAML:
```yml
kind: BrowserTabs
version: "1.0"
content:
- kind: TabGroup
  tabs:
  - kind: Tab
    url:  https://example.com
```

### Type definitions

TypeScript:
```ts
declare namespace BrowserTabsJson
{
  interface Tab
  {
    kind: 'Tab';
    title?: string;
    url: string;
  }
  interface TabGroup
  {
    kind: 'TabGroup';
    title?: string;
    tabs: TabElement[];
  }
  type TabElement = TabGroup | Tab;
}
interface BrowserTabsJson
{
  kind: 'BrowserTabs';
  version: '1.0';
  content: BrowserTabsJson.TabGroup[];
}
```