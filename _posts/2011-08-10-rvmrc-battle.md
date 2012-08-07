## Are you fighting with the rest of your team on custom .rvmrc settings?

Recently I had a discussion about the use of *gemsets*.
<span comment="here at [Mikamai](http://mikamai.com)"/>
*“questionable personal taste”* I said, *“vital company standards”*
said others…

## A local `.rvmrc` to the rescue

Put this in the common `.rvmrc`,
put your personal prefs (like gemsets) in your (`.gitignore`'d)
`.rvmrc.local` and make peace!

```bash
# Load a local .rvmrc
test -f .rvmrc.local && source .rvmrc.local
```
