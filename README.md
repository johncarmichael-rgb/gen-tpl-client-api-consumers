# generate-it-typescript-client-to-server

Used in conjunction with generate-it to output api clients for server to server or browser to server.

The template expects you to inject your own service called "HttpService" which it will then use to call your APIs.

Within the classes created, you set the url before the path to be called with {{basePath}} which if not set defaults to '/'.

If you need to import additional files to either get or calculate the basePath, you can do so with additionalImports injectable array of objects.

## Full example by js script:

```js

const apiNames = [
  'apiAuth',
  'apiNotifications'
];

const generateIt = require('../node_modules/generate-it/build/generateIt').default;
const generate = (configArray) => {
  if (configArray.length > 0) {
    const apiName = configArray.shift();
    const config = {
      swaggerFilePath: '../../' + apiName + '/swagger/build/api_spec.yml',
      targetDir: './src/apis/' + apiName,
      template: 'https://github.com/acr-lfr/generate-it-typescript-client-to-server.git',
      // Inject dynamic content to the tpl here:
      variables: {
        httpServiceImport: '@/services/HttpService',
        basePath: `config.${apiName}`
      },
      dontRunComparisonTool: false,
      dontUpdateTplCache: false,
      mockServer: false,
      segmentsCount: 1,
    };
    generateIt(config)
      .then(() => {
        console.log(`API generated for: ${apiName}`);
        generate(configArray);
      })
      .catch((e) => {
        console.log('API generation error: ', e);
      });
  }
};

generate(apiNames);


```
