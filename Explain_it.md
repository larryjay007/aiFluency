# Explain It Like You Built It

One real thing that happened while building my ML Study Coach agent: it gave me a real,
correctly-worded quote, but cited it as coming from the wrong document.

The quote itself wasn't made up — it really exists, word for word, in a real file in my
GitHub repo. The problem was that the agent said it came from a different file, one that
actually never had that sentence in it at all. It's like citing a real Shakespeare quote
but crediting it to a Charles Dickens novel — the words are accurate, but the source is
wrong.

I caught this by actually checking the quote against the document it claimed to come from,
instead of just trusting the citation because it sounded confident and specific.

Why this matters: the whole reason I built this agent was so I could trust its answers more
than a plain chat session, since every claim was supposed to trace back to something real
and checkable. One wrong citation breaks that promise completely — if it can confidently
misattribute one thing, I have no way to know which of its other answers might have the
same problem, without manually checking every single one myself. At that point, the
grounding feature isn't actually protecting me from anything.
