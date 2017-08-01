---
layout: post
title:  "Listing all possible images using digitalocean api"
categories: python digitalocean
---

It’s hard to find what digital ocean images are available on digitalocean. Using following python script we can easily get all required data. Before execution, you should export DIGITALOCEAN_ACCESS_TOKEN to your environment.

{% highlight python %}
import os
import json
import requests

url = "https://api.digitalocean.com/v2/images?per_page=999"
headers = {"Authorization" : "Bearer " + os.environ['DIGITALOCEAN_ACCESS_TOKEN']}

r = requests.get(url, headers=headers)
distros = {}

for entry in r.json()["images"]:
    s = "(" + str(entry["id"]) + ") " + entry["name"]
    if entry["distribution"] in distros:
        distros[entry["distribution"]].append(s)
    else:
        distros[entry["distribution"]] = [s]

for key in distros:
    print key + ":"
    for list_elem in distros[key]:
        print "\t" + list_elem
{% endhighlight %}

We should get:
{% highlight plaintext %}
Fedora:
    (18027532) 24 x64
    (23220211) 25 x64 Atomic
    (24914827) 26 x64 [BETA/UNSUPPORTED]
    (25570414) 26 x64 [BETA]
    (25909008) 25 Atomic x64
    (26041969) 25 x64
    (26399442) 26 x64
    (26599847) 26 Atomic x64
CentOS:
    (14782899) 6.7 x32
    (14782952) 6.7 x64
    (17384153) 7.2 x64
    (18325354) 7.2 x64
    (18835303) 7.2 x64
    (21419098) [EOL] 7.2.1511 x64
    (25718224) 6.9 x32
    (25718228) 6.9 x64
    (25718515) 7.3.1611 x64
CoreOS:
    (26425038) 1465.2.0 (beta)
    (26424713) 1409.7.0 (stable)
    (26424733) 1478.0.0 (alpha)
........
    and many other lines
{% endhighlight %}


