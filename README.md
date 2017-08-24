# Now you see me, now you don't

Have you ever been in a situation where:

```javascript
// index.js
const A = 'a';
export default A;

git add index.js
git commit -m "Adds super new feature A (no overengineering)"

# some automatic log message post commit or comment on a PR
# and suddenly you need to apply format changes

// index.js
const A = 'a'
export default A

git add index.js
git commit -m "Adds code format: removes unnecesary semicolons"
```

Only to find your git logs don't really need that last commit and you wish you'd
remembered to format your code before committing.

**git rebase -i HEAD~N**

[Git Rebase Interactive](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History)

Awesome no? Let's use a one liner (very carefully and only for scenarios similar
to the one outlined above).

```sh
git reset --soft HEAD~2 && git commit -m"$(git log --format=%B --reverse HEAD..HEAD@{1})"
```

**Shell alias**

```sh
# add to your own dotfiles
alias git-fix='git reset --soft HEAD~2 && git commit -m"$(git log --format=%B --reverse HEAD..HEAD@{1})"'
```

## See it in action

This repo has the following commits (```git log```):

- I wish I'd remembered to format my code before commit 3
- Adds commit 3
- Adds commit 2
- Adds commit 1 
- Now you see me, now you don't: adds initial commit  

Clone the repo ```git clone https://github.com/AquiGorka/nysm-nyd``` and run the
command ```git reset --soft HEAD~2 && git commit -m"$(git log --format=%B
--reverse HEAD..HEAD@{1})"```

Take a look at the commit log now (```git log```):

- Adds commit 3
- Adds commit 2
- Adds commit 1
- Now you see me, now you don't: adds initial commit  

But the code format changes are there. Sweet.
