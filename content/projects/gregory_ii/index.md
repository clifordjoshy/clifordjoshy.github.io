---
title: "Gregory II"
subtitle: "an app to help me learn GRE words"
date: 2025-03-31T11:31:38+05:30
tools: ["react", "bootstrap"]
---

## Links

{{<iconlink "github" "GitHub" "https://github.com/clifordjoshy/gregory_ii">}}

## About

An app to help improve your vocabulary. While the word lists above are specific to GRE, it's easy enough to swap out the `words.json` with any word list you want. The repo above currently contains deduplicated words from the [GregMat](https://www.gregmat.com/recommended-resources) and [Magoosh](https://s3.amazonaws.com/magoosh.resources/magoosh-gre-1000-words_oct01.pdf) word lists.

{{<image src="word.png" height="300px" caption="the word page">}}

The `"SHOW"` button displays the definition of the word, following which you can mark it as `"GOT IT"` or `"NOPE"`, following which a new word is displayed. The number in the top left indicates how many words you have left in the word list. This info is saved to local storage, so your vocabulary choices are saved across sessions. Once you've classified every word in the list is when the real use comes in.

{{<image src="menu.png" height="300px" caption="the main menu and the control panel">}}

The "Confused Words" page will allow you to go through all the words you've ever marked as something you didn't get. This is great if you want to quickly review all the confusing words at any point. "Forgotten Words" does something similar, where it allows you to go through all the words you're confused by, but marking a word as "`GOT IT`" will prevent it from showing up again. This is very useful when you're in the learning phase, allowing you to keep repeating words that are especially confusing and avoid clicking through words you've learnt since you started using Gregory II.

The "Control Panel" page allows you to do some admin stuff
1. Exporting and importing your current status on the word list. Who needs the "cloud" when you can download a JSON file and pass it along across all your devices.
2. Reset the "Forgotten Words" section (to include all the confusing words), so that you can re-review all the confusing words.
3. Reset the status of a particular word in case you mark a word mistakenly.

## Personal Notes

This was the first "low effort" project I'd worked on in a while and I found it quite useful in my GRE prep. Plus, it's a nice feeling to be able to solve these simple problems by yourself, so much so that I wrote a whole [post]({{<relref "/posts/side-quests">}}) about it.