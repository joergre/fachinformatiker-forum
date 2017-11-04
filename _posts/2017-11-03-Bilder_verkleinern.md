---
layout: post
title: Bilder verkleinern
featured: true
author: joerg_reuter
tags: [Bilder]
image: '/images/posts/verkl.jpg'
---
Bildquelle: https://www.flickr.com/photos/60614290@N08/14848580426

Immer wieder müssen Bilder an bestimmte Größen angepasst werden. Wie z.B. die Bilder der Artikel in diesem Blog. Dies fnktioniert auf dem Raspberry Pi am besten mit dem Tool ImageMagick.

Das Tool installieren wir mit:

```bash
sudo apt install imagemagick
```

Wir möchten ein Bild z. B. auf 320 Pixel reduzieren:

```bash
mogrify -resize  320 test.png 
```