# MicrofrontendProjects

#Outputs - >

#MF -> Product MF , which was set to 3001 
<img width="960" alt="Screenshot 2024-08-27 at 3 04 14 AM" src="https://github.com/user-attachments/assets/380c5e7a-c7ec-4c4e-8452-2136e386ea29">

#MF -> Cart MF , which was set to localhost:3002 
<img width="1063" alt="Screenshot 2024-08-27 at 3 04 24 AM" src="https://github.com/user-attachments/assets/a0607725-8201-4efb-a495-ef40b17ca0c2">

#Root-> Container , which was set to localhost: 3000

<img width="1031" alt="Screenshot 2024-08-27 at 3 08 04 AM" src="https://github.com/user-attachments/assets/4cacf8e3-c3f1-4316-9b59-2b6766746ce2">

                         Micro-Frontend
Steps to start -
Pnpx create-mf-app -> pnpx is like npx and create mf app is to create module federation app

npm init -y -> Initial package configuration 

npm install webpack webpack-cli webpack-dev-server html-webpack-plugin -> Initial webpack faker

webpack.config.js
const HtmlWebpackPlugin = require('html-webpack-plugin');


module.exports= {
   mode: "development",
   devServer: {
       port: 3001
   },
   plugins: [
       new HtmlWebpackPlugin({
           template: './public/index.html'
       })
   ]
}


**Module Federation Plugin**

Import Module Federation Plugin -> const ModuleFederationPlugin = require('webpack/lib/contaiber/ModuleFederationPlugin')
Configure MFE in webpack config -> 
           new ModuleFederationPlugin({
           name: 'products',
           filename: 'remoteEntry.js',
           exposes: {
               './ProductIndex':
               './src/index.js'
           }
       })


//The complete webpack config looks like below
const HtmlWebpackPlugin = require('html-webpack-plugin');
const ModuleFederationPlugin = require('webpack/lib/contaiber/ModuleFederationPlugin')
module.exports= {
   mode: "development",
   devServer: {
       port: 3001
   },
   plugins: [
       new HtmlWebpackPlugin({
           template: './public/index.html'
       }),
       new ModuleFederationPlugin({
           name: 'products',
           filename: 'remoteEntry.js',
           exposes: {
               './ProductIndex':
               './src/index.js'
           }
       })
   ]
}


Use MFE in container/shell 
       plugins: [
       new HtmlWebpackPlugin({
           template: './public/index.html'
       }),
       new ModuleFederationPlugin({
          name: 'container',
           remotes: {
               products: 'products@http://localhost:3001/remoteEntry.js',
               cart: 'cart@http://localhost:3002/remoteEntry.js'
           }
       })
   ]

Import MFE’s 
import 'products/ProductIndex';
import 'carts/CartIndex';
Bootstrapping-> It should be inside index.js of container.
 import('./bootstrap.js');
console.log('container');










