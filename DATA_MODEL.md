# DATA_MODEL.md — quote-app

## Overview

`quote-app` has **no database**. Quotes are stored as a hardcoded JavaScript array in `server/src/data/quotes.js`. This is an intentional design decision for a read-only, single-purpose micro-app.

---

## Quote Object Schema

```js
{
  id:     number,  // 1-based, matches array index + 1
  text:   string,  // The quote body
  author: string   // Attribution
}
```

---

## Data: `server/src/data/quotes.js`

```js
module.exports = [
  { id: 1,  text: "The only way to do great work is to love what you do.",                 author: "Steve Jobs" },
  { id: 2,  text: "In the middle of every difficulty lies opportunity.",                   author: "Albert Einstein" },
  { id: 3,  text: "It does not matter how slowly you go as long as you do not stop.",      author: "Confucius" },
  { id: 4,  text: "Life is what happens when you're busy making other plans.",             author: "John Lennon" },
  { id: 5,  text: "The future belongs to those who believe in the beauty of their dreams.",author: "Eleanor Roosevelt" },
  { id: 6,  text: "Spread love everywhere you go.",                                        author: "Mother Teresa" },
  { id: 7,  text: "When you reach the end of your rope, tie a knot in it and hang on.",   author: "Franklin D. Roosevelt" },
  { id: 8,  text: "Always remember that you are absolutely unique.",                       author: "Margaret Mead" },
  { id: 9,  text: "Do not go where the path may lead; go instead where there is no path.",author: "Ralph Waldo Emerson" },
  { id: 10, text: "You will face many defeats in life, but never let yourself be defeated.",author: "Maya Angelou" },
];
```

---

## Future Considerations

If quotes need to be editable or user-contributed in a future version, the natural migration path would be a SQLite table with `id`, `text`, `author`, `created_at` columns — consistent with the `todo-app` data model pattern.
