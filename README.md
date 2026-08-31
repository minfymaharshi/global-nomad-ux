# Global Nomad — design files

Food delivery for emergencies. Two files, no build step, no server, no
downloads. Open either one in a browser and it works.

| File | What it is |
|---|---|
| `Global_Nomad_UX.html` | The working prototype. Switch place, person, view and phone. |
| `Global_Nomad_Design_System.html` | The kit: components, states, sizes, icons, movement, handover notes. Desktop/Phone switch at the top. |
| `VERSION_HISTORY.md` | What changed, when, and by whom. Add a row when you commit. |

## Open it

Download and double-click either file. Nothing to install. Works offline.

## The rules these files keep

1. **Made-up data, real arithmetic.** No figure on screen is typed by hand.
   Kit contents multiply out, stock is set aside oldest-use-by first, weights
   add up, vehicle limits are checked, distances and drive times are computed.
2. **No server.** Waiting, failing and going offline are faked in one place, so
   any flow can be made to break on purpose and the way out can be shown.
3. **Plain words.** No system language in front of a person. There is a banned
   word list and a check that enforces it.
4. **Greys only** until a look is agreed. Meaning never comes from colour alone.
5. **Real movement, small and switchable.** Five movements, none over 320ms,
   nothing that costs layout work, all off under "reduce motion".
6. **WCAG 2.2 AA** on phone and desktop, no exceptions.

## Check it yourself

Open the prototype, then in the browser console:

```
GN.CHECK.all()
```

Four checks run and print numbers: words, figures, movement, and access
(contrast, text sizes, touch sizes). All four should be clean.

Individually: `GN.A11Y.audit()` · `GN.CHECK.words()` · `GN.CHECK.numbers()` ·
`GN.CHECK.motion()`

## Working on it with other people

Both files are single HTML files, which keeps them easy to share but makes
merge conflicts likely if two people edit the same one at once. Two habits
avoid nearly all of it:

- **Say which file and which section you are taking** before you start. Each
  file is split into numbered sections that only depend on lower numbers.
- **One task per commit**, and add its row to `VERSION_HISTORY.md` in the same
  commit.

If several people need to work in parallel regularly, the files are already cut
along the lines a split would follow — the section headers mark exactly where.

## A link to share

GitHub Pages serves these files as a normal web page that updates whenever you
push. Once the repository exists and Pages is switched on, the links are:

```
https://<account>.github.io/<repository>/Global_Nomad_UX.html
https://<account>.github.io/<repository>/Global_Nomad_Design_System.html
```

**Before switching Pages on, decide this:** a GitHub Pages site is readable by
anyone with the link, and search engines can find it. On a free account Pages
also requires the repository itself to be public. These files contain no real
personal data — every name, phone number, address and figure is invented — but
they do name the client and describe the work.

If that is not acceptable, the alternatives are a private repository with the
files shared some other way, or a host that supports password protection.

## Where the numbers come from

Nothing in these files is real except the arithmetic and the shape of the data.
Names, phone numbers, addresses, plate numbers and coordinates are all
invented. Phone numbers use ranges that cannot be assigned.
