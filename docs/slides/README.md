## How to create slides with markdown and remark.js

#### Slides are separated by:

```
---
```

#### For title slides or slides with a single statement/keyword, use

```
---
class: middle,center
```

#### For title with bullet points, use

```
---
class: middle
```

#### If the code is too long, try to make the content wider

```
---
class: wide
```

#### If text or code are too tall to fit, wrap them with `.small[` and `]`

```
.small[
   content here
]
```

#### For something even smaller, use `.smaller[..]` or `.tiny[..]`.

#### To insert image,

```
![text](image.jpg)
```

with scale

```
![:scale 50%](image.jpg)
```

#### For background image (fits the whole slide)

Add this just after the `class:`

```CSS
background-image: url(image.png)
```
