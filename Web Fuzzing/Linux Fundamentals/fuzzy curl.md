[[Linux Cheat Sheet]]

This curl command can be used to search for all unique URL's for a given website, such as inlanefreight.com for the example shown below. 

`'curl "https://www.inlanefreight.com" | grep -Eo "https:\/\/.{0,3}\.inlanefreight\.com[^\"\']*" | sort -u | wc -l'

`-E` uses extended regular expressions and only prints the matched portions of the input `-o`
`"https\/\/"` matches all URL's that start with https.
`{0,3}` before the domain allows for potential subdomains or variants such as www, org, etc.
`sort -u` sorts by URL's