# gfwlist-plain

Plaintext GFWList rules, automatically synced from upstream daily via GitHub Actions, for browser extensions that only accept plaintext rule lists.

## How it works

The upstream [gfwlist/gfwlist](https://github.com/gfwlist/gfwlist) publishes `gfwlist.txt` as base64-encoded AutoProxy rules, which some tools can't consume directly. A GitHub Actions workflow in this repo automatically performs the following every day:

1. Fetch the latest upstream `gfwlist.txt`
2. Decode it from base64 to plaintext
3. Merge it line-by-line with custom rules in `custom-rules.txt` (custom rules take precedence)
4. Commit only when the content actually changed

## Subscription URL

Accessible from mainland China without a proxy (jsDelivr mirror, cached for ~12 hours):

```
https://cdn.jsdelivr.net/gh/yfy69kyhrf-ui/gfwlist-plain@main/gfwlist.txt
```

Mirror fallbacks (swap the hostname if the main one is unreachable):

```
https://fastly.jsdelivr.net/gh/yfy69kyhrf-ui/gfwlist-plain@main/gfwlist.txt
https://testingcf.jsdelivr.net/gh/yfy69kyhrf-ui/gfwlist-plain@main/gfwlist.txt
```

Original URL (not directly reachable from mainland China):

```
https://raw.githubusercontent.com/yfy69kyhrf-ui/gfwlist-plain/main/gfwlist.txt
```

## Adding custom rules

Edit `custom-rules.txt` directly on github.com and commit — rules follow AutoProxy / ABP syntax and are merged into `gfwlist.txt` on the next update cycle:

```
||example-blocked.example^      block
@@||example-ok.example^         exception (allow)
```

## Files

| File | Description |
|---|---|
| `gfwlist.txt` | Generated output — **do not edit manually**, it gets overwritten on every update |
| `custom-rules.txt` | Custom rules, maintained by hand |
| `.github/workflows/update.yml` | The automation workflow |

## Update frequency

Once a day (cron `0 3 * * *`, i.e. 03:00 UTC / 11:00 Beijing time). To change it, edit the cron expression in the workflow; the minimum interval is 5 minutes (`*/5 * * * *`). Note that GitHub scheduled workflows can be delayed by several minutes under high load.

## License

Upstream rules are licensed under [LGPL-2.1](https://github.com/gfwlist/gfwlist/blob/master/COPYING.txt); this repository follows the same license.

---

[中文说明](README-EN.md 同目录的 README.md) → see [README.md](README.md) for the Chinese version.
