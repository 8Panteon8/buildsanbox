# Practice in creating an environment for project  on  React

>_I wanted to dive into [`create-react-app`](https://create-react-app.dev) work_


**Setting up [babel](https://babeljs.io)**

Inastall presets and configuration setting
```shell
npm install --save-dev @babel/preset-env
npm install --save-dev @babel/preset-react
```


```json

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

Creating a dynamically selected server in the folder `.browserslistrc`(for the experiment)
```
last 2 firefox versions
last 1 edge versions
```

**Set up [webpack](https://webpack.js.org)**

Install webpack and webpack-cli

```shell
npm install --save-dev webpack webpack-cli
```

Creat config webpack for production and development
```node.js
const HtmlWebpackPlugin = require("html-webpack-plugin");
const MiniCssExtractPlugin = require("mini-css-extract-plugin");
const { CleanWebpackPlugin } = require("clean-webpack-plugin");

module.exports = (env = {}) => {
  const { mode = "development" } = env;

  const isProd = mode === "production";
  const isDev = mode === "development";

  const getStyleLoaders = () => {
    return [
      isProd ? MiniCssExtractPlugin.loader : "style-loader",
      "css-loader",
    ];
  };

  const getPlugins = () => {
    const plugins = [
      new CleanWebpackPlugin(),
      new HtmlWebpackPlugin({
        title: "Hello World",
        buildTime: new Date().toISOString(),
        template: "public/index.html",
      }),
    ];

    if (isProd) {
      plugins.push(
        new MiniCssExtractPlugin({
          filename: "main-[hash:8].css",
        })
      );
    }

    return plugins;
  };

  return {
    mode: isProd ? "production" : isDev && "development",

    output: {
      filename: isProd ? "main-[hash:8].js" : undefined,
    },

    module: {
      rules: [
        {
          test: /\.js$/,
          exclude: /node_modules/,
          loader: "babel-loader",
        },

        // Loading images
        {
          test: /\.(png|jpg|jpeg|gif|ico)$/,
          use: [
            {
              loader: "file-loader",
              options: {
                outputPath: "images",
                name: "[name]-[sha1:hash:7].[ext]",
              },
            },
          ],
        },

        // Loading fonts
        {
          test: /\.(ttf|otf|eot|woff|woff2)$/,
          use: [
            {
              loader: "file-loader",
              options: {
                outputPath: "fonts",
                name: "[name].[ext]",
              },
            },
          ],
        },

        // Loading CSS
        {
          test: /\.(css)$/,
          use: getStyleLoaders(),
        },

        // Loading SASS/SCSS
        {
          test: /\.(s[ca]ss)$/,
          use: [...getStyleLoaders(), "sass-loader"],
        },
      ],
    },

    plugins: getPlugins(),

    devServer: {
      open: true,
    },
  };
};
```

Add `scripts` in `package.json`for a quick start

```json
 "scripts": {
    "start": "webpack-dev-server",
    "buildD": "webpack --env mode=production",
    "buildP": "webpack --env mode=development",
  },
```
