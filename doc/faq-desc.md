### 如何实现不让框架的授权认证拦截我的接口？
> 方法一：pom中不依赖`authentication`模块  
> 方法二：修改`authentication.ShiroConfig.loadShiroFilterChain`通用拦截配置，代码如下
```java
class ShiroConfig{
    private void loadShiroFilterChain(ShiroFilterFactoryBean shiroFilterFactoryBean){
        /*下面这些规则配置最好配置到配置文件中 */
        Map<String, String> filterChainDefinitionMap = new LinkedHashMap<>();
        /**
         *  cas单点登录 securityFilter
         *  token拦截   jwt
         *  不拦截接口 annon
         *  cas单点登出  logout
         */

        filterChainDefinitionMap.put("/userInfo/refreshToken", "securityFilter");
        filterChainDefinitionMap.put("/userInfo/**", "jwt");
        filterChainDefinitionMap.put("/notice/**", "jwt");
        //cas单点登出 logout
        filterChainDefinitionMap.put("/cas/logout", "logout");
        //cas回调 callbackFilter
        filterChainDefinitionMap.put("/callback", "callbackFilter");
        //jwt认证 jwt
        filterChainDefinitionMap.put("/test/**", "jwt");
        filterChainDefinitionMap.put("/druid/**","anon");
        filterChainDefinitionMap.put("/**","anon");
        //预先权限配置
        // filterChainDefinitionMap.put("/user/edit/**", "authc,perms[user:edit]");
        shiroFilterFactoryBean.setFilterChainDefinitionMap(filterChainDefinitionMap);
    }
}
```

### 如何自定义单点登录的页面效果？
> 复制cas登录页面到sso-cas中resources/static/中，用于替换原有的登录页面，对登录页面做自定义的修改
> 详细操作请自行百度，overlay替换cas登录页面。