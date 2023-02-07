# Practice in creating an environment for project  on  React

>_I wanted to dive into [`create-react-app`](https://create-react-app.dev) work_


**Setting up [babel](https://babeljs.io)**

Inastall presets and configuration setting
```shell
npm install --save-dev @babel/preset-env
```


```javascript

  "presets": [
    [
      "@babel/env",
      {
        "corejs": 3,
        "useBuiltIns": "usage",
        "debug": true,
        "modules": false
      }
    ],
    "@babel/react"
  ]

```
