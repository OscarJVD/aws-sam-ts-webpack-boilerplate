sam local invoke "Test-Lambda" --event event.json --env-vars env.json --log-file logfile.txt

tsc && npx tsc-alias && cp -R node_modules dist/ && cp -R package-lock.json dist/ && cp -R package.json dist/ && npm run build

sam build

sam package --template-file template.yaml --output-template-file packaged.yaml --s3-bucket social-publisher

sam deploy --guided o para actualizar: sam deploy --template-file packaged.yaml --stack-name social-publisher-stack  --s3-bucket social-publisher --capabilities CAPABILITY_IAM



This code is published as part of the [corresponding blog article](https://www.toptal.com/aws/typescript-jest-aws-sam-tutorial) at the Toptal Engineering Blog.

Visit https://www.toptal.com/blog and subscribe to our newsletter to read great articles!

---

## Enable development mode with hot reloading

```bash
NODE_ENV=development npm run watch
```

## Start the API server locally

```bash
sam local start-api
```
const path = require("path")
const TsconfigPathsPlugin = require("tsconfig-paths-webpack-plugin")

module.exports = {
  mode: "development", // Ajusta este valor según sea necesario (development, production)
  entry: "./src/app.ts", // Asegúrate de que este punto de entrada es correcto
  target: "node", // Específico para AWS Lambda
  resolve: {
    extensions: [".ts", ".js"], // Resuelve estos tipos de archivos
    plugins: [
      new TsconfigPathsPlugin({ configFile: "./tsconfig.json" }), // Usando TsconfigPathsPlugin para resolver los paths
    ],
  },
  output: {
    devtoolModuleFilenameTemplate: "[absolute-resource-path]",
    path: path.resolve(__dirname, "dist"), // Salida de los archivos compilados
    filename: "app.js", // Nombre del archivo de salida
    libraryTarget: "commonjs2", // Configuración recomendada para módulos Node.js
    clean: true, // Un-comment this to clean the output directory on each build
  },
  module: {
    rules: [
      {
        test: /\.ts$/, // Procesa archivos .ts
        use: "ts-loader", // Utiliza ts-loader para manejar TypeScript
        exclude: /node_modules/, // Ignora node_modules
      },
    ],
  },
  optimization: {
    minimize: false, // Minimizar el código puede ser opcional
  },
}
