# 项目优化

## Global State 

app 

- `title`
- `lang` => `language` => { `name`, `pack` }
- `GameID` 
- `Role`
- `views`
- `MenuRole`
- `menuId`
- `GameList`
- `currentUserId`

server 

- `APPKEY`
- `TOKEN`
- `serverURL`
- `testURL`
- `prdURL`
- `wayURL`

permission

- `hasManager`
- `hasOPADMIN`
- `hasUserManager`
- `hasSysConfPerm`

cache

- `cacheTemplate` => `CacheTemplateList`
- `cacheConfig`   => `CacheConfigList` 
- `cacheZone`     => `CacheServerList`
- `cacheWay`      => `CacheWayList`
- `cacheSelect`   => `CacheSelect`  => 下拉菜单
- `cacheOptions`  => `CacheOptions` => 枚举内容LIST

## Vue Redux 数据流程优化

> 项目引入Redux优化整个数据流程，保持单向数据流可追溯。需要开发基于Vue的Redux插件，才能够有效确保系统数据流的稳定性。为此规范了项目规范及约束内容。

**组件重新划分**

- 智能组件（smart Component）：提供数据、函数方法（Actions）等业务支撑。
- 机械组件（machine Component）：处理智能组件传递的数据和函数方法，完成数据展示和控制业务

**组件区分和描述**

> 智能组件需提供 $SmartRedux 的 Vue属性，即vm.$SmartRedux。其包含以下内容：

- State: 当前状态，其值为一个Object。由于vue本身不具备react的diff dom。故此vue-redux会逐个做对比，若有不同，则自动启用vm.$set更新该值。组件初始化时，会自动将State合并至vm.$data里。
- Actions：智能函数，提供具有store.dispatch的actions函数，用于操纵数据流
- 注意：初始化时，即vm.created，会使用store.subscribe绑定到$updateStore方法里，即store有更新时，自动更新到$updateStore。当组件初销毁时，即vm.destroyed，会使用unsubscribe解除绑定.

> 机械组件

- 允许内部有自己可控制的事件、函数及私有变量。
- 涉及Redux数据流时，数据及函数actions须走离它最近的父级智能组件。禁止直接从Store获取数据。
- 涉及Global State，可通常ConfigStore.getStore获取。

> 约束规范

- 禁用`vm.inherit`继承父级数据，原有项目存在多处继承，必须移除，仅能通过智能组件传递数据
- 一切涉及业务数据（例如全局数据、数据库、环境数据等等）必须走redux的actions、Reducer、middleware
- 按需加载设置分割点时，禁用以机械组件为分割点，必须以智能组件做为分割点。为防止加载失败，需设置fallback备用方案。
