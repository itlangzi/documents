# NPM 发布包注意事项
## 版本升级
1. 升级主要版本号 `npm version major`  
2. 升级次要版本号 `npm version minor`  
3. 升级补丁版本号 `npm version patch`  

## 使用淘宝镜像需注意  
- 登录/发布注意事项    
添加命令参数 `--registry=http://registry.npmjs.org/`  
登录示例：`npm login --registry=http://registry.npmjs.org/`  
发布示例：`npm publish --registry=http://registry.npmjs.org/`  
