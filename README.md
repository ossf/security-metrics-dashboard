Security Risk Dashboard

`*alpha software*`

This is a simple dashboard that shows the security risk of open-source projects. Target audiences include:
- Software Developers -> quickly evaluate the risk of OSS they plan to use in their product
- Security Engineering Teams -> incorporate the rich set of metrics into their risk assessment process
- Engineering Management -> understand ecosystem-wide risk and make informed decisions about OSS usage

The dashboard is customizable and incorporates data from the following sources:
- OpenSSF Scorecard
- Libraries.io
- git
- GitHub
- more to come...

Usage

`1.` raw data
```
$ dash generate --repo=github.com/project/repo --output=json

{
    "scorecard": { ... },
    "libraries_io": { ... },
    "git": { ... },
    "github": { ... }
}
```

`2.` dashboard
```
$ dash generate --repo=github.com/project/repo --output=html --config=dashboard.yaml
```
sample config file
```
dashboard:
  title: "Security Risk Dashboard"
  description: "This is a sample dashboard"
  shape:
    x: 4
    y: 4
  metrics:
    - source: scorecard
      attributes: # these are pulled from the raw json output; use jq syntax
        - name: A
          path: .score
          component:
            - type: card
              x0: 0
              y0: 0
              x: 2
              y: 2 
        - name: B
          path: .checks[0].{score: score}
          component:
            - type: donut
              x0: 2
              y0: 0
              x: 2
              y: 2 
    - source: libraries_io
      attributes:
        - name: C
          path: .checks[0].{value: values}
          component:
            - type: card
              x0: 0
              y0: 2
              x: 4
              y: 2 
```

corresponding view
```
title
description
---------
| A | B |
---------
|   C   |
---------
```